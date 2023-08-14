FROM alpine/git as git-semver-checkout
WORKDIR /go
RUN git clone https://github.com/mdomke/git-semver.git ./src
RUN cd ./src
RUN ls -l


FROM golang:1.21-alpine as git-semver-builder
RUN go version
WORKDIR /go
COPY --from=git-semver-checkout /go/src /go/src/github.com/mdomke/git-semver
WORKDIR /go/src/github.com/mdomke/git-semver
RUN ls -l
RUN set -Eeux && \
    go mod download && \
    go mod verify
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 \
  go build \
  -trimpath \
  -ldflags='-w -s -extldflags="-static"' -a \
  -o /go/bin/git-semver

FROM alpine:3
RUN apk add --no-cache \
  bash \
  curl \
  git \
  jq \
  yq
COPY --from=git-semver-builder /go/bin/git-semver /bin/git-semver
