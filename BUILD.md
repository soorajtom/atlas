# Building Atlas

This document explains how to build the Atlas database management tool from source.

## Prerequisites

- **Go 1.22.7 or later** (project requires Go 1.22.7+, tested with Go 1.23.10)
- Git (to clone the repository if needed)

## Quick Build

The simplest way to build Atlas:

```bash
cd cmd/atlas
go build -o atlas .
```

This will create an `atlas` binary in the `cmd/atlas` directory.

## Detailed Build Steps

### 1. Install Dependencies

First, ensure all Go module dependencies are downloaded:

```bash
# From the project root
go mod download

# Or from cmd/atlas (which has its own go.mod)
cd cmd/atlas
go mod download
```

### 2. Build the Binary

Build the Atlas CLI tool:

```bash
cd cmd/atlas
go build -o atlas .
```

Or build from the project root:

```bash
go build -o atlas ./cmd/atlas
```

### 3. Install Globally (Optional)

To install Atlas system-wide:

```bash
# From cmd/atlas directory
go install .

# Or from project root
go install ./cmd/atlas
```

This will install the binary to `$GOPATH/bin` or `$HOME/go/bin` (if `$GOPATH` is not set).

### 4. Verify the Build

Test that the binary works:

```bash
./atlas version
./atlas help
```

## Build Options

### Cross-Compilation

Build for different platforms:

```bash
# Build for Linux
GOOS=linux GOARCH=amd64 go build -o atlas-linux-amd64 ./cmd/atlas

# Build for macOS
GOOS=darwin GOARCH=amd64 go build -o atlas-darwin-amd64 ./cmd/atlas
GOOS=darwin GOARCH=arm64 go build -o atlas-darwin-arm64 ./cmd/atlas

# Build for Windows
GOOS=windows GOARCH=amd64 go build -o atlas-windows-amd64.exe ./cmd/atlas
```

### Release Build

For a release build with optimizations and without debug symbols:

```bash
go build -ldflags="-s -w" -o atlas ./cmd/atlas
```

### Build with Version Information

To include version information in the binary:

```bash
go build -ldflags="-X 'ariga.io/atlas/cmd/atlas/internal/cmdapi.Version=v1.0.0'" -o atlas ./cmd/atlas
```

## Project Structure

- `cmd/atlas/` - Main CLI application entry point
- `internal/` - Internal packages (not for external use)
- `sql/` - SQL dialect implementations
- `schemahcl/` - HCL schema parsing

## Troubleshooting

### Module Replace Directive

The `cmd/atlas/go.mod` file uses a `replace` directive to reference the parent module:
```
replace ariga.io/atlas => ../..
```

This is normal and allows the nested module to use the parent module's code.

### Missing Dependencies

If you encounter missing dependencies:

```bash
go mod tidy
```

### CGO Dependencies

Some database drivers (like SQLite) may require CGO. If you encounter CGO-related errors, ensure you have:
- A C compiler (gcc, clang)
- CGO enabled: `export CGO_ENABLED=1`

## Development

For development, you can use:

```bash
# Run tests
go test ./...

# Run with race detector
go test -race ./...

# Format code
go fmt ./...

# Run linter (if configured)
golangci-lint run
```

