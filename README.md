# Satring with gRPC server client app

## Install gRPC for go
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
export PATH="$PATH:$(go env GOPATH)/bin"

## initiate go app
go mod init github.com/carloscfgos1980/client-server-gRPC
