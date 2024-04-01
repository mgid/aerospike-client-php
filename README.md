# Aerospike PHP Client
[![Build Status](https://travis-ci.org/aerospike/aerospike-client-php.svg?branch=master)](https://travis-ci.org/aerospike/aerospike-client-php)
[![License](https://img.shields.io/packagist/l/aerospike/aerospike-client-php.svg)](https://img.shields.io/packagist/l/aerospike/aerospike-client-php.svg)

Note: This client supports PHP versions >= 7 . If you are looking for the Legacy client which supports PHP versions up through 5, it can be found at the [aerospike-client-php5 repo](https://github.com/aerospike/aerospike-client-php5)

## Differences from the previous Aerospike PHP Client:

- LDT Support has been removed.
- Type checking in general is stricter for method parameters. If you are not sure whether an argument to a function is an integer or a string, we recommend casting it to the type specified by the method. Running with strict_types enabled may help to catch some issues.
- An exception will be raised if the constructor fails to connect to the cluster.
- \Aerospike\Bytes will be stored to the server as type AS\_BYTES\_BLOB instead of AS\_BYTES\_PHP. This change allows better compatability with other clients.
- Correspondingly, data stored in the server as AS\_BYTES\_BLOB will be returned as Aerospike\Bytes,  if no deserializer has been registered. The Previous version of the Aerospike PHP Client returned a string if AS_BYTES_BLOB was encountered with no registered deserializer. **Note** It is not recommended to combine the use of \Aerospike\Bytes and user specified deserializers, as this may cause errors.
- Support for PHP versions < 7 has been removed.
- The INI entry `aerospike.serializer` now takes an integer value. 0 for No Serializer, 1 for default PHP serialization, and 2 for user specified serializer. See [Configuration](doc/phpdoc/aerospike.php) for additional information on the code values.
- The constructor will no longer attempt to create a unique SHM key for the user. If a key is not specified in the shm configuration array, the default value will be used. A key provided in the constructor takes precedence over a value specified by INI.
- The layout of the shared memory used by the client when using an SHM key has changed. The default key has changed as well in order to prevent accidental sharing between new and old clients.
- The formatting of the response from an info call may have changed. It now includes the request at the beginning of the response.
- When using initKey with a digest, the digest must now be exactly 20 bytes.
- The integer values of the `Aerospike::LOG_LEVEL_*` constants have changed. This should not effect the user unless they were providing log levels as integers rather than using the constants.
- `Aerospike::LOG_LEVEL_OFF` has been removed. It no longer had any effect.


## Documentation

Documentation of the Aerospike PHP Client may be found in the [doc directory](doc/README.md).
Notes on the internals of the implementation are in [doc/internals.md](doc/internals.md).

[Example PHP code](examples/) can be found in the `examples/` directory.

Full documentation of the Aerospike database is available at http://www.aerospike.com/docs/

## Dependencies

### CentOS and RedHat (yum)

    sudo yum groupinstall "Development Tools"
    sudo yum install openssl-devel
    # You will need PHP7 development headers. If PHP7 was manually installed, these should be available
    # by default; Otherwise, you will need to fetch them from a repository, the package name may vary.
    sudo yum install php-pear # unless PHP was manually installed

### Ubuntu (Focal)

    sudo apt-get install build-essential autoconf libssl-dev git software-properties-common
    sudo add-apt-repository ppa:ondrej/php
    sudo apt-get update
    sudo apt-get install php8.3-dev php-pear

### OS X

By default OS X will be missing command line tools. On Mavericks (OS X 10.9)
and higher those [can be installed without Xcode](http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/).

    xcode-select --install # install the command line tools, if missing

The dependencies can be installed through the OS X package manager [Homebrew](http://brew.sh/).

    brew update && brew doctor
    brew install automake
    brew install openssl

To switch PHP versions [see this gist](https://gist.github.com/rbotzer/198a04f2315e88c75322).

Do to the dependence of this Library upon OpenSSL, there will ocassionally be linking issues which will show up during installation.
To Solve these we recommend running
    brew info openssl

This command will show some information about where the openssl headers and library are located. In particular there should a stanza which looks something like:
    For compilers to find this software you may need to set:
    LDFLAGS:  -L/usr/local/opt/openssl/lib
    CPPFLAGS: -I/usr/local/opt/openssl/include

in order to properly link and compile against the library you can set the two environment variables `AS_OSX_OPENSSL_INC` and `AS_OSX_OPENSSL_LINK` to the respective values (if the paths were as specified above):
    export AS_OSX_OPENSSL_INC="-I/usr/local/opt/openssl/include"
    export AS_OSX_OPENSSL_LINK="-L/usr/local/opt/openssl/lib"

Windows is currently not supported.

## Installation

### Building Manually

To build the PHP extension manually you will need to fetch the
[library](https://github.com/mgid/aerospike-client-php.git)
from Github, then run the `build.sh` script in the `src/` directory:

    git clone https://github.com/mgid/aerospike-client-php.git
    cd aerospike-client-php/src
    export AEROSPIKE_C_CLIENT=4.6.23
    ./build.sh

This will download the Aerospike C client SDK if necessary into
`src/../aerospike-client-c/`, and initiate `make`.

## Installing the PHP Extension

To install the PHP extension do:

    sudo make install
    php -i | grep ".ini "

Now edit the php.ini file.  If PHP is configured --with-config-file-scan-dir
(usually set to `/etc/php.d/`) you can create an `aerospike.ini` file in the
directory, otherwise edit `php.ini` directly. Add the following directive:

    echo '[aerospike]' > /etc/php/8.3/mods-available/aerospike.ini
    echo 'extension=aerospike.so' >> /etc/php/8.3/mods-available/aerospike.ini
    phpenmode aerospike

The *aerospike* module should now be available to the PHP CLI:

    php -m | grep aerospike
    aerospike

## License

The Aerospike PHP Client is made availabled under the terms of
the Apache License, Version 2, as stated in the file [LICENSE](./LICENSE).

Individual files may be made available under their own specific license,
all compatible with Apache License, Version 2. Please see individual files for
details.


