#
# Copyright IBM Corp. All Rights Reserved.
#
# SPDX-License-Identifier: Apache-2.0
#
ARG GO_VER
ARG ALPINE_VER

FROM golang:${GO_VER}-alpine as builder
ARG GO_LDFLAGS
ARG GO_TAGS

RUN apk add --no-cache \
	gcc \
	git \
	musl-dev;

ADD . /build/fabric-ca
WORKDIR /build/fabric-ca
RUN go install -tags "${GO_TAGS}" -ldflags "${GO_LDFLAGS}" \
	github.com/hyperledger/fabric-ca/cmd/fabric-ca-server \
	&& go install -tags "${GO_TAGS}" -ldflags "${GO_LDFLAGS}" \
	github.com/hyperledger/fabric-ca/cmd/fabric-ca-client


FROM alpine:${ALPINE_VER}
RUN apk add --no-cache \
	tzdata;
ENV FABRIC_CA_HOME /etc/mihongtech/mh-ca-server
COPY --from=builder /go/bin/fabric-ca-server /usr/local/bin/mh-ca-server
EXPOSE 7054
CMD mh-ca-server start -b admin:adminpw