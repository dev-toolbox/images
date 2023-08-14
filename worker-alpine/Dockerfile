FROM alpine/git as git-semver-checkout
WORKDIR /go
RUN git clone https://github.com/mdomke/git-semver.git ./src


FROM golang:1.21-alpine as git-semver-builder
WORKDIR /go
COPY --from=git-semver-checkout /go/src /go/
RUN cd ./src
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -ldflags='-w -s -extldflags="-static"' -a -o /go/bin/git-semver

FROM alpine:3
RUN apk add --no-cache \
  bash \
  curl \
  git \
  jq \
  yq
COPY --from=git-semver-builder /go/bin/git-semver /bin/git-semver
