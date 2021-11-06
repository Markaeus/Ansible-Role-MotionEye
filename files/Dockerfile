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
    # Change uid/gid of user/group motion to match our desired IDs.  This will
    # make it easier to use execute motion as our desired user later.
    pip install motioneye && \
    # Cleanup
    apt-get purge --yes python-setuptools python-wheel && \
    apt-get autoremove --yes

FROM debian:buster-slim
WORKDIR /app/motioneye
COPY --from=builder /app/motioneye /app/motioneye
