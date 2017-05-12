# The FROM will be replaced when building in OpenShift
FROM rhel7/rhel

MAINTAINER Siamak Sadeghianfar <ssadeghi@redhat.com>

# Install headless Java
USER root

RUN yum clean all && \
    yum-config-manager --disable rhel* 1>/dev/null && \
#    yum-config-manager --disable rhel-7-server-rt-beta-rpms && \
    yum-config-manager --enable rhel-7-server-rpms && \
    yum-config-manager --enable rhel-7-server-ose-3.2-rpms && \
    export INSTALL_PKGS="wget java-1.8.0-openjdk-headless \
        java-1.8.0-openjdk-devel wget gettext tar git" && \
    yum clean all && \
    yum install -y --setopt=tsflags=nodocs install $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all && \
    mkdir -p /var/lib/jenkins && \
    chown -R 1001:0 /var/lib/jenkins && \
    chmod -R g+w /var/lib/jenkins
RUN wget http://11.120.148.211/nss_wrapper-1.1.3-1.el7.x86_64.rpm

RUN yum -y localinstall nss_wrapper-1.1.3-1.el7.x86_64.rpm

# Copy the entrypoint
COPY configuration/* /var/lib/jenkins/
USER 1001

# Run the JNLP client by default
# To use swarm client, specify "/var/lib/jenkins/run-swarm-client" as Command
ENTRYPOINT ["/var/lib/jenkins/run-jnlp-client"]
