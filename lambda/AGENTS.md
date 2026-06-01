@AGENTS.local.md

# Development Stack

- Golang 1.26.3

# Structure

- `cmd`: Lambda Entrypoint
- `cmd/private/{version}`: Private API
- `cmd/public/{version}`: Public API
- `cmd/infra/`: Internal Lambda Handler
- `internal`: Internal implementations
- `dist/bin`: `go build` output directory

# Rule

- Designing the Public API should be done with BFF (Backend For Frontend) in mind, ensuring the interface aligns with the frontend requirements.
- The `cmd` directory must only contain `main.go` and its corresponding test file, `main_test.go`. All other implementation details must be placed under the `internal` directory.

# Commands

- `mise build`: Builds the binaries under the `cmd` directory and outputs them to `dist/bin`.
- `mise lint`: Run Lint
- `mise format`: Run Formatter
