# The add-on image is built from this repository.  The application source is
# vendored under src/, so the image can be built without a checkout of the
# Osprey repository.
ARG BUILD_FROM=python:3.13-slim-bookworm
FROM ${BUILD_FROM}

ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1 \
    PYTHONPATH=/opt/osprey/pyunifiwire/src:/opt/osprey/osprey

RUN apt-get update \
    && apt-get install --yes --no-install-recommends ca-certificates ffmpeg tini \
    && rm -rf /var/lib/apt/lists/* \
 && printf '%s\n' \
      'paho-mqtt==2.1.0 --hash=sha256:6db9ba9b34ed5bc6b6e3812718c7e06e2fd7444540df2455d2c51bd58808feee' \
      > /tmp/requirements-runtime.txt \
    && python -m pip install --no-cache-dir --require-hashes -r /tmp/requirements-runtime.txt \
    && rm /tmp/requirements-runtime.txt

COPY osprey/src/osprey /opt/osprey/osprey
COPY osprey/src/pyunifiwire /opt/osprey/pyunifiwire
COPY osprey/run.sh /run.sh

RUN chmod 0755 /run.sh \
    && mkdir -p /data/osprey \
    && addgroup --system osprey \
    && adduser --system --ingroup osprey osprey \
    && chown -R osprey:osprey /opt/osprey /data/osprey

WORKDIR /opt/osprey/osprey
USER osprey
ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["/run.sh"]
