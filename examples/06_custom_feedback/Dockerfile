# build environment -- feel free to adapt to your needs.

FROM alpine:edge as base

RUN apk --no-cache upgrade \
  && apk add --no-cache --initramfs-diskless-boot \
  python3 gdb musl-dbg \
  curl tar \
  build-base perf \
  clang lld

COPY ./harness/ /opt/
RUN cd /opt/ && make clean && make

#### switch to snapchang container ####
FROM ghcr.io/fuzzinglabs/snapchange

# copy whole root filesystem from build environment to this container layer
# SNAPSHOT_INPUT is the location that the snapshotting script will create.
COPY --from=base / "$SNAPSHOT_INPUT"

# set the path to the target within the root filesystem of the build environment
ENV SNAPSHOT_ENTRYPOINT="/opt/maze"
# optionally: set the working directory of the target
ENV SNAPSHOT_ENTRYPOINT_CWD="/opt/"
