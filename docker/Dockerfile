FROM golang:1.23-alpine AS build
WORKDIR /src
COPY . .
RUN CGO_ENABLED=0 go build -trimpath -ldflags "-s -w" -o /out/jango-bulk ./cmd/jango-bulk

FROM alpine:3.20
RUN apk add --no-cache ca-certificates && adduser -D -u 1000 app
WORKDIR /data
COPY --from=build /out/jango-bulk /usr/local/bin/jango-bulk
USER app
EXPOSE 20581
ENTRYPOINT ["jango-bulk"]
