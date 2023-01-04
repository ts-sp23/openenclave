# Dockerfile for attested build

The build of OpenEnclave is done in two stage; the first stage
(Dockerfile.base) creates the base container with all dependencies (including
Intel SGX packages), while the second stage (Dockerfile) builds the SDK
itself. See
[https://github.com/ts-sp23/attested-build-container/tree/main/samples/oe] for
the associated container configurations.

