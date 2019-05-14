FROM alpine:3.9 as watchdog

RUN addgroup -S app && adduser -S -g app app

# Alternatively use ADD https:// (which will not be cached by Docker builder)
RUN apk --no-cache add curl ca-certificates \
    && echo "Pulling watchdog binary from Github." \
    && curl -sSLf https://github.com/openfaas-incubator/of-watchdog/releases/download/0.5.0/of-watchdog > /usr/bin/fwatchdog \
    && chmod +x /usr/bin/fwatchdog \
    && apk del curl

FROM kubesec/kubesec:35b1aba

COPY --from=watchdog /usr/bin/fwatchdog /usr/bin/fwatchdog

ENTRYPOINT []

ENV upstream_url="http://127.0.0.1:8082"
ENV mode=http
ENV fprocess="/home/app/kubesec http 8082"

CMD ["fwatchdog"]
