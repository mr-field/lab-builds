FROM docker.io/rockylinux/rockylinux:9.5

RUN dnf group install -y "Development Tools"; \
    dnf install -y cmake hwloc-devel libdb-devel motif-devel ncurses-devel openssl-devel pam-devel rsync systemd-devel wget; \
    dnf install -y https://dl.rockylinux.org/pub/rocky/9/CRB/x86_64/os/Packages/l/libtirpc-devel-1.3.3-9.el9.x86_64.rpm

RUN git clone https://github.com/daimh/sge; \
    cd sge; \
    cmake -S . -B build -DCMAKE_INSTALL_PREFIX=; \
    cmake --build build -j; \
    cmake --install build
