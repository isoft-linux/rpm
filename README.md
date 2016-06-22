# RPM Next

One day [Cjacker](https://github.com/cjacker) had an idea: there are OS layer 
(/var/lib/rpm) and AppStore layer (/var/lib/appstore) physical isolated. 
so rpm -ivh/-e only install/erase Apps into/from AppStore layer, OS is totally 
READ-ONLY. when installing Apps, check dependency in OS layer at first, if 
unsatisfied depend, then switch to AppStore layer for checking dependency again, 
if still unsatisfied depend, add dependency problem, otherwise installed Apps 
successfully.

![RPM isolatation design](https://raw.github.com/isoft-linux/rpm/isoftapp/doc/rpm-isolatation-design.png)

## Goal

* keep rpm original command unbroken;
* keep librpm original API unbroken;
* just like Mac ***make install*** is still able to pollute OS layer;

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
    --with-vendor=aosc \
    --enable-debug=yes
```

## isoftapp db

```
rpm --initdb --dbpath /var/lib/isoft-app
```

## RPM Next command

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
