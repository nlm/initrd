FROM scaleway/ubuntu:amd64-xenial
MAINTAINER Nicolas Limage <github@xephon.org>

# Prepare rootfs for builds
RUN /usr/local/sbin/scw-builder-enter


# Install packages
COPY packages.txt /etc
RUN apt-get -qq update \
 && apt-get -o Dpkg::Options::='--force-confold' dist-upgrade -y
RUN apt-get -y install \
	curl \
	dropbear \
	kexec-tools \
	nfs-common \
	ntpdate \
	parted \
	btrfs-tools \
	udhcpc \
	wget \
	python3 \
	python3-pip


# Clean packages to make image lighter
RUN apt-get clean


# Install busybox-static
RUN wget -O /tmp/busybox.deb                                                                        \
      http://ftp.fr.debian.org/debian/pool/main/b/busybox/busybox-static_1.22.0-9+deb8u1_amd64.deb  \
 && dpkg -i /tmp/busybox.deb                                                                        \
 && rm -f /tmp/busybox.deb


# Fetch ldd-rec.pl
RUN wget https://raw.githubusercontent.com/moul/mbin/master/ldd-rec.pl  \
      -O /usr/local/bin/ldd-rec.pl                                      \
 && chmod +x /usr/local/bin/ldd-rec.pl


# Copy local assets
COPY ./export-assets /usr/local/bin/
RUN export-assets \
	/bin/busybox \
	/bin/mkfs.btrfs \
	/lib/arm-linux-gnueabihf/libnss_dns.so.2 \
	/lib/arm-linux-gnueabihf/libnss_files.so.2 \
	/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2 \
	/lib/x86_64-linux-gnu/libc.so.6 \
	/lib/x86_64-linux-gnu/libnss_dns.so.2 \
	/lib/x86_64-linux-gnu/libnss_files.so.2 \
	/lib/x86_64-linux-gnu/libresolv.so.2 \
	/sbin/kexec \
	/sbin/mkfs.btrfs \
	/sbin/mkfs.ext4 \
	/sbin/parted \
	/usr/bin/dropbearkey \
	/usr/lib/klibc/bin/ipconfig \
	/usr/sbin/dropbear \
	/usr/sbin/ntpdate

# Export some packages
COPY ./export-pkgs /usr/local/bin/
RUN export-pkgs \
	python3 \
	python3-minimal \
	python3-pip \
	python3-chardet \
	python3-pkg-resources \
	python3-requests \
	python3-setuptools \
	python3-six \
	python3-wheel

COPY sysassert /usr/src/sysassert
RUN pip3 install /usr/src/sysassert

COPY ./export-dirs /usr/local/bin/
RUN export-dirs /usr/lib/python3 /usr/local/lib/python3.5

# Final Command
CMD ["cat", "/dependencies.tar"]
