# Start with a Go builder image
FROM golang:1.23-alpine AS build_base

RUN apk add --no-cache git

# Set the Current Working Directory inside the container
WORKDIR /tmp/app

# Populate the module cache based on the go.{mod,sum} files.
COPY go.mod .
COPY go.sum .

RUN go mod download

COPY . .

# Build the Go app
RUN go build -o ./out/basic-auth-to-oauth2-transformer ./cmd

# Now use a Distroless image for the final runtime container
FROM gcr.io/distroless/base:nonroot

# Copy the binary from the builder stage
COPY --from=build_base /tmp/app/out/basic-auth-to-oauth2-transformer /app/basic-auth-to-oauth2-transformer

# This container exposes port 8080 to the outside world
EXPOSE 8080

# Run the binary program
CMD ["/app/basic-auth-to-oauth2-transformer"]
