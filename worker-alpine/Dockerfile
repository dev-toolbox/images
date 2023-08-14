FROM golang:1.21-alpine as git-semver-builder
WORKDIR /go/src/github.com/mdomke/git-semver
COPY . .
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -ldflags='-w -s -extldflags="-static"' -a -o /go/bin/git-semver

FROM alpine:3
RUN apk add --no-cache \
  bash \
  curl \
  git \
  jq \
  yq
COPY --from=git-semver-builder /go/bin/git-semver /bin/git-semver
