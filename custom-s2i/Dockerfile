# custom-s2i
FROM registry.access.redhat.com/ubi8/ubi

# TODO: Put the maintainer name in the image metadata
# LABEL maintainer="Your Name <your@email.com>"

# TODO: Rename the builder environment variable to inform users about application you provide them
ENV APP_ROOT=/opt/app-root \
    XDG_CACHE_HOME=off

# TODO: Set labels used in OpenShift to describe the builder image
LABEL io.k8s.description="Platform for building xyz" \
      io.k8s.display-name="builder x.y.z" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,x.y.z,etc." \
      io.openshift.s2i.scripts-url="image:///usr/libexec/s2i"

# TODO: Install required packages here:
# RUN yum install -y ... && yum clean all -y
RUN yum install -y --nodocs --disableplugin subscription-manager golang && \
  yum clean all -y --disableplugin subscription-manager 

# TODO (optional): Copy the builder files into /opt/app-root
# COPY ./<builder_folder>/ /opt/app-root/

# TODO: Copy the S2I scripts to /usr/libexec/s2i, since openshift/base-centos7 image
# sets io.openshift.s2i.scripts-url label that way, or update that label
COPY ./s2i/bin/ /usr/libexec/s2i

# TODO: Drop the root user and make the content of /opt/app-root owned by user 1001
RUN mkdir -p ${APP_ROOT} && \
  chown -R 1001:1001 ${APP_ROOT} && \
  chgrp -R 0 ${APP_ROOT} && \
  chmod -R g=u ${APP_ROOT} && \
  chmod +x /usr/libexec/s2i/assemble /usr/libexec/s2i/run /usr/libexec/s2i/usage

# This default user is created in the openshift/base-centos7 image
USER 1001

# TODO: Set the default port for applications built using this image
EXPOSE 8080

WORKDIR ${APP_ROOT}

# TODO: Set the default CMD for the image
CMD ["/usr/libexec/s2i/usage"]
