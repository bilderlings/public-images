FROM alpine

RUN apk add --no-cache tar minio-client
RUN ln -s /usr/bin/mcli /usr/bin/mc

RUN addgroup -g 1000 appgroup \
    && adduser -u 1000 -S -G appgroup appuser

USER appuser
