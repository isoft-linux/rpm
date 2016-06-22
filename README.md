This is RPM, the RPM Package Manager.

The latest releases are always available at:

	http://rpm.org/releases/

Additional RPM documentation (papers, slides, HOWTOs) can also be
found at the same site: http://rpm.org.

http://rpm.org/wiki/Communicate lists all rpm releated mailing lists.

RPM was originally written by:

    Erik Troan <ewt@redhat.com>
    Marc Ewing <marc@redhat.com>

See the CREDITS file for a list of folks who have helped us out
tremendously.  RPM is Copyright (c) 1998 by Red Hat Software, Inc.,
and may be distributed under the terms of the GPL and LGPL (see  the
file COPYING for details).

## Build

```
wget http://download.oracle.com/berkeley-db/db-6.1.23.tar.gz
tar xvf db-6.1.23.tar.gz
ln -s db-6.1.23 db
```

```
export CPPFLAGS="$CPPFLAGS `pkg-config --cflags nss` -DLUA_COMPAT_APIINTCASTS"
export CFLAGS="$RPM_OPT_FLAGS -DLUA_COMPAT_APIINTCASTS"
./autogen.sh --prefix=/usr   \
    --sysconfdir=/etc   \
    --localstatedir=/var    \
    --with-archive  \
    --with-lua  \
    --with-cap  \
    --enable-plugins    \
    --enable-python \
    --with-vendor=isoft \
    --enable-debug=yes
```

## Db

```
rpm --initdb --dbpath /var/lib/isoft-app
```

## RPM Next

1.

```
rpm --eval %__cmake
rpm --eval %_libdir
rpm --eval %_unitdir
```

```
sudo rpm -e firefox-i18n-40.0-1.x86_64 firefox-40.0-12.x86_64
```

```
sudo rpm -ivh --isoftapp firefox-40.0-12.x86_64.rpm
```

```
rpm -q firefox
rpm -q --isoftapp firefox
```

```
rpm -V firefox
rpm -V --isoftapp firefox
```

```
sudo rpm -e firefox
sudo rpm -e --isoftapp firefox
```

```
sudo rpm --import --isoftapp ISOFT-APP-GPG-KEY
rpm --resign /data/download/firefox-40.0-12.x86_64.rpm 
rpm -qip --isoftapp /data/download/firefox-40.0-12.x86_64.rpm
```

2.

```
rpmbuild --isoftapp -ba rakudo-star.spec 
错误：构建依赖失败：
        libtommath-devel 被 rakudo-star-0.0.2015.09-1.x86_64 需要
        moarvm-devel 被 rakudo-star-0.0.2015.09-1.x86_64 需要
        nqp >= 0.0.2015.09 被 rakudo-star-0.0.2015.09-1.x86_64 需要
        sha-devel 被 rakudo-star-0.0.2015.09-1.x86_64 需要
sudo rpm --isoftapp -ivh libtommath*.rpm
rpmbuild --isoftapp -ba rakudo-star.spec
```

```
sudo rpm -ivh --isoftapp libtar-1.2.20-1.x86_64.rpm
```

```
rpm -q --isoftapp libtar
rpm -q libtar
```

```
sudo rpm -e libtar
sudo rpm -e --isoftapp libtar
```
