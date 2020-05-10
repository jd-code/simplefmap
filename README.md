# Simplefmap : Bulkrays example #

Simplefmap is a light c++ http server provided as an example of use of the [bulrays](https://github.com/jd-code/bulkrays) C++ library.

# Usage #
there's yet no typical service script for maintaining the daemon operations.
such a command line will launch the server binding on two addresses, *dropping root
credentials* for a less dangerously empowered user "bulkrays" right after binding ports
for example :
```
./simplefmap --bind=134.214.100.25:80 --bind=134.214.100.245:80 --user=simplefmap 2>&1 \
    >> /var/log/simplefmap/error.log & 
```

# Dependencies #
* [qiconn/libqiconn](https://github.com/jd-code/qiconn) a pool of "socket-enriched" derivative of iostreams.
* [bulkrays/libbulkrays](https://github.com/jd-code/bulkrays) light http server C++ library.

### Typical building dependencies ###
in order to compile on a debian buster :
* build-essential
* autotools-dev
* libtool
* expect (provides unbuffer used in the "vimtest" target)
* libmhash-dev	(computes md5 here and there ...)


# Building #
the following will bring a default build :
```
[ retrieve and install libqiconn ]
[ retrieve and install bulkrays ]
autoall && ./configure
make all
```
A real interesting build should contain additionnal cpp modules of yours, inspired by
`simplefmap.hcpp`, and duly referenced in `bootstrap.cpp`.
