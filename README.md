# gRPC Client/Server in Go

Small Go project that demonstrates a gRPC server and a client with basic person operations:

- `Create`
- `Read`
- `Update`
- `Delete`

The server stores data in memory (map), so records reset when the server restarts.

## Project Structure

```text
.
├── client/
│   └── main.go
├── proto/
│   ├── person.proto
│   ├── person.pb.go
│   └── person_grpc.pb.go
├── server/
│   └── main.go
├── go.mod
└── README.md
```

## Prerequisites

- Go installed (`go version`)
- Protocol Buffers compiler installed (`protoc --version`)

macOS (Homebrew):

```bash
brew install protobuf
```

Install Go protoc plugins:

```bash
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```

Make sure Go bin path is in your shell PATH:

```bash
export PATH="$PATH:$(go env GOPATH)/bin"
```

If needed, add that line to your `~/.zshrc` and reload:

```bash
source ~/.zshrc
```

## Install Dependencies

From project root:

```bash
go mod tidy
```

## Generate gRPC Code from Proto

Run this from project root whenever you change `proto/person.proto`:

```bash
protoc --go_out=. --go_opt=paths=source_relative \
	--go-grpc_out=. --go-grpc_opt=paths=source_relative \
	proto/*.proto
```

## Run the App

Open two terminals at project root.

Terminal 1: start server

```bash
go run server/main.go
```

Expected server log:

```text
gRPC server listening at [::]:8080
```

Terminal 2: run client

```bash
go run client/main.go
```

The client will call Create, Read, Update, and Delete in sequence.

## API (PersonService)

Service package: `personservice`

### `Create`

- Request: `CreatePersonRequest`
	- `name` (string)
	- `email` (string)
	- `phoneNumber` (string)
- Response: `PersonProfileResponse`
	- `id` (int32)
	- `name` (string)
	- `email` (string)
	- `phoneNumber` (string)

### `Read`

- Request: `SinglePersonRequest`
	- `id` (int32)
- Response: `PersonProfileResponse`
	- `id` (int32)
	- `name` (string)
	- `email` (string)
	- `phoneNumber` (string)

### `Update`

- Request: `UpdatePersonRequest`
	- `id` (int32)
	- `name` (string)
	- `email` (string)
	- `phoneNumber` (string)
- Response: `SuccessResponse`
	- `response` (string)

### `Delete`

- Request: `SinglePersonRequest`
	- `id` (int32)
- Response: `SuccessResponse`
	- `response` (string)

## Proto Package Path

The module path in `go.mod` and import paths in code must match exactly:

```text
github.com/carloscfgos1980/client-server-gRPC
```

If you see errors like "no required module provides package .../proto", check for path typos or casing differences.

## Quick Troubleshooting

`zsh: command not found: protoc`

- Install protoc: `brew install protobuf`
- Verify: `protoc --version`

`protoc-gen-go: program not found or is not executable`

- Install plugin:
	`go install google.golang.org/protobuf/cmd/protoc-gen-go@latest`
- Ensure PATH includes `$(go env GOPATH)/bin`

`protoc-gen-go-grpc: program not found or is not executable`

- Install plugin:
	`go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest`
- Ensure PATH includes `$(go env GOPATH)/bin`

Build/resolve issues

```bash
go mod tidy
go list ./...
go build -o /tmp/server-bin ./server
```