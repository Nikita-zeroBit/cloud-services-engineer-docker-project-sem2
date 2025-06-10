FROM golang:latest AS build
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . /app
RUN CGO_ENABLED=0 GOOS=linux go build -o /app/go_app ./cmd/api

FROM alpine:latest
RUN apk add --no-cache ca-certificates && update-ca-certificates
RUN addgroup -g 1001 -S appgroup && adduser -u 1001 -S appuser -G appgroup
WORKDIR /app
COPY --from=build /app/go_app .
USER appuser
EXPOSE 8081
CMD ["./go_app"]
