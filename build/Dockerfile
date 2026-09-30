FROM quay.io/openshift/origin-must-gather:4.22 as builder

FROM registry.access.redhat.com/ubi9-minimal:latest

# BUILD_DATE differs on every build. The update below references it, so it
# re-runs rather than reusing a cached layer from an earlier build.
ARG BUILD_DATE=unknown
RUN echo "Updating packages for build ${BUILD_DATE}" && \
    microdnf -y update && \
    microdnf -y install rsync tar gzip jq findutils && \
    microdnf -y clean all

COPY --from=builder /usr/bin/oc /usr/bin/oc
COPY collection-scripts/* /usr/bin/

LABEL \
        com.redhat.component="forklift-must-gather-container" \
        io.k8s.display-name="Forklift" \
        io.k8s.description="Forklift - Must Gather" \
        io.openshift.tags="migration,mtv,forklift" \
        name="Forklift" \
        summary="Forklift - Must Gather" \
        description="Forklift - Must Gather" \
        maintainer="Migration Toolkit for Virtualization Team <migtoolkit-virt@redhat.com>"

ENTRYPOINT /usr/bin/gather
