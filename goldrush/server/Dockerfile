FROM golang:alpine as build

COPY . /goldrush/
WORKDIR /goldrush
RUN go build -ldflags "-X '$(go list -m)/pkg/def.ver=$(gitver)'" -o bin/ "$@" ./cmd/*

FROM powerman/alpine-runit-volume:v0.4.1 as run

LABEL org.opencontainers.image.source="https://github.com/Djarvur/allcups-itrally-2020-task"

ENV VOLUME_DIR=/home/app/var/data
ENV SYSLOG_DIR=$VOLUME_DIR/syslog
VOLUME $VOLUME_DIR

EXPOSE 8000

HEALTHCHECK --interval=5s --timeout=5s \
    CMD wget -q -O - http://$HOSTNAME:8000/health-check || exit 1

COPY . .
COPY --from=build /goldrush/bin bin
RUN ln -nsf "$PWD"/init/* /etc/service/
