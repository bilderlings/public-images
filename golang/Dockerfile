FROM golang:1.25-alpine AS build

RUN apk add --no-cache build-base git

RUN go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest \
    && go install github.com/jstemmer/go-junit-report@latest

FROM golang:1.25-alpine

RUN addgroup -g 1000 appgroup \
    && adduser -u 1000 -S -G appgroup -h /home/appuser appuser \
    && mkdir -p /home/appuser \
    && chown -R appuser:appgroup /home/appuser

COPY --from=build /go/bin /usr/local/bin
USER appuser

WORKDIR /home/appuser/app
