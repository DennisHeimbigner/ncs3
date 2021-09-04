# Library usage

### s3_init

`struct S3 * s3_init(char *key_id, char *secret, char *host)`

Call to initialize a `struct S3` pointer.
  Example:
```
struct S3 *s3 = s3_init(aws_key_id, aws_secret, "s3.amazonaws.com");
```

### s3_free

`void s3_free(struct S3 *s3)`
Frees the S3 context.

### s3_bucket_entries

`struct s3_bucket_entries * s3_list_bucket(struct S3 *s3, char *bucket, char *prefix)`

List keys in `bucket` with optional `prefix`. Passing NULL as prefix
will list the top-level keys.

Returns a `struct s3_bucket_entry_head` pointer which is a TAILQ head
pointer of a list of `struct s3_bucket_entry` pointers. These must be
freed by the caller with `s3_bucket_entries_free`

```
struct s3_bucket_entry {
	char *key;
	char *lastmod;
	size_t size;
	char *etag;
	TAILQ_ENTRY(s3_bucket_entry) list;
};
```

Example:
```
	struct s3_bucket_entry_head *bkt_entries;
	struct s3_bucket_entry *e;

	bkt_entries = s3_list_bucket(s3, bucket, NULL);
	TAILQ_FOREACH(e, bkt_entries, list) {
		printf("Key %s\n", e->key);
	}
	s3_bucket_entries_free(bkt_entries);
```

### s3_bucket_entries

`void s3_bucket_entries_free(struct s3_bucket_entry_head *entries)`

Iterates through all entries in `entries` and free. Free the `entries` pointer.

### s3_string_init

`struct s3_string * s3_string_init()`

Initializes a `struct s3_string` pointer, which later can be freed
with `s3_string_free`.

This can be later outputted using `printf("%.*s\n", (int)str->len, str->ptr)`

### s3_string_free

`void s3_string_free(struct s3_string *str)`

Frees a `struct s3_string`.

### s3_get

`void s3_get(struct S3 *s3, char *bucket, char *key, struct s3_string *out)`

Download the contents of `key` in `bucket` into the string `out`.

Example:

```
	out = s3_string_init();
	s3_get(s3, bucket, "foo.txt", out);

	printf("Contents:\n%.*s\n", (int)out->len, out->ptr);

	s3_string_free(out);
```

### s3_put

`void s3_put(struct S3 *s3, char *bucket, char *content_type, char *contents, size_t len)`

Upload `len` bytes of `contents` into `key` in `bucket`.

Example:

```
	char *contents = "foo bar gazonk";
	s3_put(s3, bucket, "foo.txt", "text/plain", file_contents, strlen(file_contents));
```

### s3_delete

`void s3_delete(struct S3 *s3, char *bucket, char *key)`

Deletes `key` from `bucket`.

Example:

```
	s3_delete(s3, bucket, "foo.txt");
```

# Testing

`./s3test <bucketname>` will list a bucket's root keys, keys under `/foo/bar`,
upload `/foobar.txt` with contents from a char pointer, download the contents
and then delete the file.

Amazon secret ID and key are fetched from environment variables
`AWS_ACCESS_KEY_ID` and `AWS_SECRET_KEY`
