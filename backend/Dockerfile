FROM golang:1.25-alpine AS development

WORKDIR /app

# Install air for hot reload
ENV PATH="/go/bin:${PATH}"

RUN go install github.com/cosmtrek/air@latest

# Copy go mod files
COPY go.mod go.sum ./
RUN go mod download

# Copy source code
COPY . .

# Expose port
EXPOSE 8080

# Default command (can be overridden in docker-compose)
CMD ["air"]

# Production stage
FROM golang:1.25-alpine AS builder

WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux go build -o /lms-ziad-quiz-server .

FROM alpine:latest AS production

WORKDIR /root/
RUN apk --no-cache add ca-certificates wget

COPY --from=builder /lms-ziad-quiz-server .

EXPOSE 8080
CMD ["./lms-ziad-quiz-server"]
