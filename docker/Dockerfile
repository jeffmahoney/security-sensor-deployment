# Defines the tag for OBS and build script builds:
#!BuildTag: linux-security-sensor:%PKG_VERSION%.%GIT_OFFSET% linux-security-sensor:%PKG_VERSION% linux-security-sensor:%PKG_VERSION%.%GIT_OFFSET%.%RELEASE% linux-security-sensor
FROM opensuse/leap:15.4

# Need to build on SLE first -- it's mostly static but depends on glibc
#FROM registry.suse.com/suse/sle15:latest

# labelprefix=org.opensuse.linux-security-sensor
LABEL org.opencontainers.image.title="Linux Security Sensor Server Container"
LABEL org.opencontainers.image.description="This contains Linux Security Sensor %PKG_VERSION%.%GIT_OFFSET%"
LABEL org.opensuse.version="%PKG_VERSION%.%GIT_OFFSET%"
LABEL org.openbuildservice.disturl="%DISTURL%"
LABEL org.opencontainers.image.created="%BUILDTIME%"
LABEL org.opensuse.reference="registry.opensuse.org/security/sensor/containers/linux-security-sensor:%PKG_VERSION%.%GIT_OFFSET%.%RELEASE%"
# endlabelprefix

VOLUME /data
VOLUME /logs
VOLUME /config

# API
EXPOSE 8801

# GUI
EXPOSE 8889

# Frontend
EXPOSE 8000

# Monitoring
EXPOSE 8003

COPY entry-point.sh generate-config.sh obs-signing-key.key /
COPY init-config.json /etc/velociraptor/

RUN chmod a+x /entry-point.sh /generate-config.sh
RUN rpm --import /obs-signing-key.key
RUN zypper -q ar obs://security:sensor/ "obs://security/sensor" && \
    zypper -q --non-interactive refresh && \
    zypper -q --non-interactive install velociraptor catatonit && \
    zypper clean -a && \
    rm -f /obs-signing-key.key

CMD ["/usr/bin/catatonit", "--", "/entry-point.sh" ]
