FROM golang:1.20.4 as builder

MAINTAINER Clint Ruoho clint@wtfismyip.com

WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY wtf.go ./

RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o wtf .

FROM alpine:edge

WORKDIR /app

RUN mkdir -p /usr/local/wtf/GeoIP && addgroup -S appuser && adduser -u 666 -S -G appuser appuser
COPY static/GeoIP /usr/local/wtf/GeoIP
COPY static/static /usr/local/wtf/static
COPY static/docker /docker
COPY static/docker/resolv.conf /etc/resolv.conf

COPY --from=builder /app/wtf .
COPY start.sh .

USER appuser
CMD [ "./start.sh" ]
