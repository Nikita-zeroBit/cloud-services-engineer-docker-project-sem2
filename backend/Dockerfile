FROM golang:latest AS build
WORKDIR /app
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
COPY go.mod go.sum ./
RUN go mod download
COPY . /app
RUN CGO_ENABLED=0 GOOS=linux go build -o /app/go_app ./cmd/api

FROM alpine:latest
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
WORKDIR /app
COPY --from=build /app/go_app .
USER appuser
EXPOSE 8081
CMD ["./go_app"]
