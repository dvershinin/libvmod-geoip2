libvmod-geoip2
==============

[![Build Status](https://travis-ci.org/fgsch/libvmod-geoip2.svg?branch=devel)](https://travis-ci.org/fgsch/libvmod-geoip2)

## About

A Varnish master VMOD to query MaxMind GeoIP2 DB files.

For Varnish 4.1/6.0 and 6.1 refer to the oldstable and master
branches, respectively.

## Requirements

To build this VMOD you will need:

* make
* a C compiler, e.g. GCC or clang
* pkg-config
* python-docutils or docutils in macOS [1]
* Varnish master built from sources
* libmaxminddb-dev in recent Debian/Ubuntu releases, maxminddb in
  macOS [1]. See also https://github.com/maxmind/libmaxminddb

If you are building from Git, you will also need:

* autoconf
* automake
* libtool

You will also need to set `PKG_CONFIG_PATH` to the directory where
**varnishapi.pc** is located before running `autogen.sh` and
`configure`.  For example:

```
export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig
```

Finally, to use it you will need one or more GeoIP2 or GeoLite2
binary databases.  See https://dev.maxmind.com/.

## Installation

### From RPM (CentOS / RedHat 7)

#### For Varnish 4.1.x

    yum install https://extras.getpagespeed.com/release-el7-latest.rpm
    yum install vmod-geoip2

#### For Varnish 6.0.x

    yum install https://extras.getpagespeed.com/release-el7-latest.rpm yum-utils
    yum-config-manager --enable getpagespeed-extras-varnish60
    yum install vmod-geoip2

### From a tarball

To install this VMOD, run the following commands:

```
./configure
make
make check
sudo make install
```

The `make check` step is optional but it's good to know whether the
tests are passing on your platform.

### From the Git repository

To install from Git, clone this repository by running:

```
git clone --recursive https://github.com/fgsch/libvmod-geoip2
```

And then run `./autogen.sh` followed by the instructions above for
installing from a tarball.

## Example

```
import geoip2;

sub vcl_init {
	new country = geoip2.geoip2("/path/to/GeoLite2-Country.mmdb");
}

sub vcl_recv {
	if (country.lookup("country/names/en", client.ip) != "Japan") {
		...
	}
}
```

More examples available at https://github.com/fgsch/libvmod-geoip2/wiki.

## DB updates

To update the GeoIP2 DB, download the new file on the same filesystem
as the old one and move it over. See also
https://github.com/maxmind/geoipupdate.

## License

This VMOD is licensed under BSD license. See LICENSE for details.

### Note

1. Using Homebrew, https://github.com/Homebrew/brew/.
