# AGENTS.md

@AGENTS.local.md

# Development Spec

- Open API Specification Version: `3.0.3`

# Structure

- `{version}/openapi.yaml`: entry point
- `{version}/paths`: each path item is written to a separate file
- `{version}/components`: components
- `shared/components`: shared components

Under the `components` directory, shared schemas, responses, parameters, examples, headers, requestBodies, links, callbacks, and securitySchemes are each split into subdirectories

# Commands

- `mise lint`: lint openapi yaml
- `mise build`: bundles split files
- `mise codegen`: generate code
