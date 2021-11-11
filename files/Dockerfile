FROM debian:buster-slim AS builder
WORKDIR /app/motioneye/
RUN echo "deb http://snapshot.debian.org/archive/debian/$(date +%Y%m%d) buster contrib non-free" >>/etc/apt/sources.list && \
    apt-get update && \
    DEBIAN_FRONTEND="noninteractive" apt-get -t buster --yes --option Dpkg::Options::="--force-confnew" --no-install-recommends install \
      curl \
      ffmpeg \
      libmicrohttpd12 \
      libpq5 \
      lsb-release \
      mosquitto-clients \
      python-jinja2 \
      python-pil \
      python-pip \
      python-pip-whl \
      python-pycurl \
      python-setuptools \
      python-tornado \
      python-tz \
      python-wheel \
      v4l-utils \
      motion \
      default-libmysqlclient-dev && \
      pip install motioneye && \
    # Cleanup
    apt-get purge --yes python-setuptools python-wheel && \
    apt-get autoremove --yes

ADD motioneye.conf.sample /usr/share/motioneye/extra/

# R/W needed for motioneye to update configurations
VOLUME /etc/motioneye

# Video & images
VOLUME /var/lib/motioneye

CMD test -e /etc/motioneye/motioneye.conf || \
    cp /usr/share/motioneye/extra/motioneye.conf.sample /etc/motioneye/motioneye.conf ; \
    # We need to chown at startup time since volumes are mounted as root. This is fugly.
    chown motion:motion /var/run /var/log /etc/motioneye /var/lib/motioneye /usr/share/motioneye/extra ; \
    su -g motion motion -s /bin/bash -c "/usr/local/bin/meyectl startserver -c /etc/motioneye/motioneye.conf"

EXPOSE 8765

FROM debian:buster-slim
WORKDIR /app/motioneye
COPY --from=builder /app/motioneye /app/motioneye
