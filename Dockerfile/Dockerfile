FROM golang:1.14.2-alpine3.11 as builder

RUN adduser -D -g '' appuser

LABEL Maintainer="DevTeam <technology@sehatq.com>"

#RUN apk add tzdata
RUN apk update && apk add --no-cache git ca-certificates && update-ca-certificates

WORKDIR /apps/

COPY go.* ./
RUN go mod download

COPY . .
# RUN mv files/ /

RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -ldflags '-extldflags "-static"' -o /go/bin/cli cli/*

FROM alpine:3.17.3

RUN ls
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /etc/passwd /etc/passwd
COPY --from=builder /go/bin/sehatq /go/bin/sehatq

# COPY --from=builder /files /files

USER root

EXPOSE 3000
# CMD ["/go/bin/sehatq"]