# Preparation

Install [prerequisites](https://openwrt.org/docs/guide-developer/quickstart-build-images "Quick Image Building Guide"):

````
sudo apt install subversion g++ zlib1g-dev build-essential git python \
   python3 python3-distutils libncurses5-dev gawk gettext unzip file \
   libssl-dev wget libelf-dev ecj fastjar java-propose-classpath
 
sudo dnf install @c-development @development-tools @development-libs \
   zlib-static elfutils-libelf-devel gawk unzip file wget python3
````

Checkout the [OpenWRT source tree](https://git.openwrt.org/openwrt/openwrt.git):

````
umask 0022
git clone https://git.openwrt.org/openwrt/openwrt.git openwrt-git
cd openwrt-git
````

Update feeds and generate a minimal configuration: 

````
./scripts/feeds update  -a
./scripts/feeds install -a
 
make menuconfig
````

* Select the correct `Target System`, e.g. `Atheros ATH79 (DTS)`
* Select the correct `Subtarget`, e.g. `Generic`
* Select the correct `Target Profile`, i.e. your router where OpenWRT is
  installed.
* If needed, a newer [compiler](https://www.gnu.org/software/gcc/) can be
  selected as well: `Advanced configuration options` => `Toolchain Options` 
  => `GCC compiler Version`.

Before we can build _packages_, we need to build our 
[toolchain](https://openwrt.org/docs/guide-developer/single.package "How to Build a Single Package"):

````
make -j$(nproc) tools/install
make -j$(nproc) toolchain/install
````

If something goes wrong, we can add `V=s` to our
[make options](https://openwrt.org/docs/techref/buildroot#warnings_errors_and_tracing "OpenWrt Buildroot – Technical Reference")
to gather more details.

# Packages
## NRPE

As an example, let's build [NRPE](https://github.com/NagiosEnterprises/nrpe) 
for OpenWRT. We need the `Makefile` and some patches and auxillary files too:

````
$ find package/network/utils/nrpe/
package/network/utils/nrpe/
package/network/utils/nrpe/files
package/network/utils/nrpe/files/openwrt-init
package/network/utils/nrpe/files/openwrt-procd
package/network/utils/nrpe/patches
package/network/utils/nrpe/patches/100-nrpe.patch
package/network/utils/nrpe/Makefile
````

Generate a new `.config` and select either `m` or `y`:

````
 $ make oldconfig
 nrpe........... Nagios Remote Plugin Executor Server (PACKAGE_nrpe) [M/n/y/?] m
````

Build with:

````
 make -j$(nproc) package/nrpe/compile
 make -j$(nproc) package/nrpe/install
 make -j$(nproc) package/index
````

If all goes well, the ([stripped](https://blog.filippo.io/shrink-your-go-binaries-with-this-one-weird-trick/ "Shrink your Go binaries with this one weird trick"))
 binary and package will be available in our `build_dir`:

````
 $ tar -tvzf bin/packages/mips_24kc/base/nrpe*mips_24kc.ipk && \
   ls -go build_dir/target-mips_24kc_musl/nrpe*/ipkg-mips_24kc/nrpe/usr/sbin/nrpe 
 -rw-r--r-- root/root         4 2020-01-29 08:05 ./debian-binary
 -rw-r--r-- root/root     21956 2020-01-29 08:05 ./data.tar.gz
 -rw-r--r-- root/root       588 2020-01-29 08:05 ./control.tar.gz
 
 -rwxr-xr-x. 1 39445 Mar  7 12:48 build_dir/target-mips_24kc_musl/nrpe-4.0.0/ipkg-mips_24kc/nrpe/usr/sbin/nrpe
````


## `monitoring-plugins`

NRPE alone won't do much, let's install 
[monitoring-plugins](https://www.monitoring-plugins.org/) as well:

````
$ find package/network/utils/monitoring-plugins/
package/network/utils/monitoring-plugins/
package/network/utils/monitoring-plugins/Makefile
````

Generate a new `.config` and select either `m` or `y` for `monitoring-plugins`.

````
 $ make oldconfig
````

Build with:

````
 make -j$(nproc) package/monitoring-plugins/compile
 make -j$(nproc) package/monitoring-plugins/install
 make -j$(nproc) package/index
````

If all goes well, we should have something like this:

````
 $ tar -tvzf bin/packages/mips_24kc/base/monitoring-plugins*mips_24kc.ipk && \
   ls -go build_dir/target-mips_24kc_musl/monitoring-plugins*/ipkg-mips_24kc/monitoring-plugins/usr/libexec/nagios/ | tail -5
 -rw-r--r-- root/root         4 2020-01-29 08:05 ./debian-binary
 -rw-r--r-- root/root    639426 2020-01-29 08:05 ./data.tar.gz
 -rw-r--r-- root/root       507 2020-01-29 08:05 ./control.tar.gz
 
 -rwxr-xr-x. 1 33285 Mar  7 23:57 check_udp
 -rwxr-xr-x. 1 33029 Mar  7 23:57 check_ups
 -rwxr-xr-x. 1 24725 Mar  7 23:57 check_users
 -rwxr-xr-x. 1 28917 Mar  7 23:57 negate
 -rwxr-xr-x. 1 28805 Mar  7 23:57 urlize
````
