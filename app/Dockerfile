FROM golang:1.21-alpine AS builder
WORKDIR /app
COPY go.mod ./
RUN go mod download
COPY main.go ./
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o simpletimeservice .

FROM scratch
COPY --from=builder /app/simpletimeservice /simpletimeservice
USER 65534:65534
EXPOSE 8080
ENTRYPOINT ["/simpletimeservice"]
