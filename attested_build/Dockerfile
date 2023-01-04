FROM ts-sp23/openenclave-base:latest as build-image

COPY . /root/openenclave
WORKDIR /root/openenclave

# RUN attested_build/sgx_deps_pin.sh

# Build OE from source
RUN mkdir build && cd build && \
    cmake -GNinja -DCPACK_GENERATOR=DEB -DCMAKE_INSTALL_PREFIX:PATH=/opt/openenclave -DCMAKE_BUILD_TYPE=RelWithDebInfo -DLVI_MITIGATION=ControlFlow -DLVI_MITIGATION_BINDIR=/usr/local/lvi-mitigation/bin .. && \
    ninja && \
    ninja install && \
    cpack && \
    find . -name *.deb -type f -exec sha256sum {} \;

FROM ubuntu:20.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && \
    apt-get install -y build-essential linux-headers-generic linux-headers-5.4.0-1091-azure ninja-build libssl-dev git

COPY --from=build-image /root/openenclave/attested_build/sgx_deps_pin.sh /root/sgx_deps_pin.sh
COPY --from=build-image /root/openenclave/scripts /root/scripts
RUN /root/sgx_deps_pin.sh

# Should setup dependencies automatically
RUN /root/scripts/ansible/install-ansible.sh
RUN ansible-playbook /root/scripts/ansible/oe-contributors-acc-setup-no-driver.yml

COPY --from=build-image /root/openenclave/build/open-enclave_0.18.0_amd64.deb /root/open-enclave_0.18.0_amd64.deb 
RUN apt-get install -y /root/open-enclave_0.18.0_amd64.deb 
