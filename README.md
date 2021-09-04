# libncs3

A C language S3 client-side library.
This is intended primarily for use by the
[netcdf-c](https://github.com/Unidata/netcdf-c.git)
NCZarr implementation.

# Dependencies:
* OpenSSL
* libcurl
 
# TODO
In no particular order, features that are left are:

- Error checking.
- Bucket creation
- Bucket deletion
- Support for key prefixes / paths
- Make into an actual library
- ACLs
- Clean up and generalize CURL / signing code (partially done)

# Provenance

This library is based on the partially completed code developed by [Ian Delahorne's](https://github.com/iandelahorne) [s3client](https://github.com/iandelahorne/s3client) code.
The original README and license are in the file [s3client-README.md](s3client-README.md).

# Authors

- Ian Delahorne (<ian.delahorne@gmail.com>)
- Dennis Heimbigner (<dennis.heimbigner@gmail.com)

