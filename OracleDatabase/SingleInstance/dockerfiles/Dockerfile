# Cross-platform images
# https://vessel.apple.com/usage/docker-compatibility/compat-mode-examples/#building-an-x86-image-on-apple-silicon

# Create the cross-platform builder first.
# https://vessel.apple.com/usage/docker-compatibility/compat-mode-examples/#create-a-buildx-cross-platform-builder
# docker buildx create --bootstrap --platform linux/amd64,linux/arm64 --driver-opt image=docker.apple.com/amp-delivery/amp-buildkit:1.0 --name vessel-builder

# Then build the image with the builder.
# https://vessel.apple.com/usage/docker-compatibility/compat-mode-examples/#build-an-image
# docker buildx build --builder vessel-builder -t docker.apple.com/ocarlsen/empty-db-builder --platform linux/amd64 --progress=plain --load .
# docker buildx build --builder vessel-builder -t docker.apple.com/ocarlsen/empty-db-builder --platform linux/arm64 --progress=plain --load .

# Run to launch, connect TTY
# docker run --platform linux/amd64 --privileged -it -v /var/run/docker.sock:/var/run/docker.sock docker.apple.com/ocarlsen/empty-db-builder
# docker run --platform linux/arm64 --privileged -it -v /var/run/docker.sock:/var/run/docker.sock docker.apple.com/ocarlsen/empty-db-builder

FROM alpine:3.20.0

ARG DB_VERSION=19.3.0

# Preserve for run time.
ENV DB_VERSION=$DB_VERSION

# Assumes Oracle distros already inside DB_VERSION subdirectory.
COPY ./${DB_VERSION} /home/root/${DB_VERSION}

# Script to build image
COPY ./buildContainerImage.sh /home/root 

# Install Bash for script.
RUN apk add --no-cache bash
RUN apk add --no-cache docker

# Fix "Error: kernel does not support overlay fs: unable to create kernel-style whiteout: operation not permitted"
RUN apk add --no-cache fuse-overlayfs

# TODO: CMD /home/root/buildContainerImage.sh -v ${DB_VERSION} -e

# TODO: docker run -it --name ${container_name} -e ORACLE_PWD=${oracle_pwd} -p1521:1521 oracle/database:${DB_VERSION}-ee
# TODO: sha=$(docker commit ${container_name} | sed 's/sha256\://')
# TODO: docker tag "$sha" docker.apple.com/ets-comms-apps/database/empty_db:${DB_VERSION}-ee
