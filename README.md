# macOS missing headers
Missing headers for the Linux kernel header installation.

## Installation

Open Terminal and go to the directory where you checked out this repository and execute the following commands to copy the needed header files to `/usr/include`:

```console
cp *.h /usr/local/include
cp /usr/include/machine/endian.h /usr/local/include
```

You will need the GNU sed utility to build the Linux headers (macOS comes with a FreeBSD sed which doesn't have all the needed features). You will need GNU sed also for other GNU projects (like gcc) so let's install it in `/opt/local`.

```console
curl -L http://ftp.gnu.org/gnu/sed/sed-4.7.tar.xz | tar xJf -
mkdir build
cd build
../sed-4.7/configure --prefix=/opt/local
make -j8
sudo make install
```

Now download the Linux kernel source.

```console
curl -L https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-5.0.tar.gz | tar xzf -
```

The last things we need to do are adding the `/opt/local/bin` to the path (which contains the GNU sed) and building the headers (replace the `/opt/local/include` with the install location).

```console
export PATH=/opt/local/bin:${PATH}
cd linux-5.0
sudo make headers_install INSTALL_HDR_PATH=/opt/local/include
```