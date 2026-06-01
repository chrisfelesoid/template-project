# AGENTS.md

@AGENTS.local.md

# Development Stack

- Terraform

# Structure

- `environments`
- `environments/_shared`
- `environments/dev`
- `environments/prd`
- `modules`
- `modules/{module-name}`

# Rules

- **No Upper-Level References**: Do not reference higher-level directories or scopes.
- **Unique Variable Names**: Ensure variable names are distinct across the project.
  - *e.g.*, `vpc_id` (not `id`)
  - *e.g.*, `log_bucket_cloudfront` (not `log_bucket`)
- **No Default Values**: Avoid setting default values for variables.
- **Minimal Conditional Logic**: Avoid using variables to dynamically alter process flows wherever possible.
- **Environment Isolation**: Use feature flags to skip creating unsupported Floci services (e.g., CloudFront, WAF) in local environments.
- **No Self-Referential Suffixes**: Inside a module, omit suffixes that repeat its own purpose on internal variables and outputs.
  - *e.g.*, in `modules/lambda/bff/`, use `lambda_function_name` (not `lambda_function_name_bff`)

## 'environments' Directory Layout

```
environments
├── _shared
│   ├── 00_bootstrap
│   │   ├── main.tf
│   │   └── variables.tf
│   ├── 10_network
│   │   ├── main.tf
│   │   └── variables.tf
│   ├── 20_data
│   │   ├── main.tf
│   │   └── variables.tf
│   ├── 30_app
│   │   ├── main.tf
│   │   └── variables.tf
│   ├── main.tf
│   └── variables.tf
└── {environment-name}
    ├── 00_bootstrap
    │   ├── locals.tf
    │   ├── extra.tf
    │   ├── main.tf  -> ../../_shared_/00_bootstrap/main.tf
    │   └── variables.tf  -> ../../_shared_/00_bootstrap/variables.tf
    ├── 10_network
    │   ├── locals.tf
    │   ├── extra.tf
    │   ├── main.tf  -> ../../_shared_/10_network/main.tf
    │   └── variables.tf  -> ../../_shared_/10_network/variables.tf
    ├── 20_data
    │   ├── locals.tf
    │   ├── extra.tf
    │   ├── main.tf  -> ../../_shared_/20_data/main.tf
    │   └── variables.tf  -> ../../_shared_/20_data/variables.tf
    ├── 30_app
    │   ├── locals.tf
    │   ├── extra.tf
    │   ├── main.tf  -> ../../_shared_/30_app/main.tf
    │   └── variables.tf  -> ../../_shared_/30_app/variables.tf
    └── backend
        └── {backend-type-name}-backend.tfbackend
```

### Directory Structure and Execution Order

Apply the layers in sequential order (from `00` to `30`). The 10-step numbering interval allows for the flexible insertion of future infrastructure layers (e.g., security, shared management) without renaming existing directories.

1. **00_bootstrap**: Provisioning the S3 bucket and necessary resources for storing tfstate.
2. **10_network**: Networking infrastructure, including VPC and related components.
3. **20_data**: Database and storage resources, such as DynamoDB.
4. **30_app**: Application tier, including API Gateway and Lambda functions.

### Rules

- **File Naming**: Do not create files outside of the allowed file names.
- **Environment Consistency**: Keep `main.tf` and `variables.tf` consistent across all environments.
  - **Exception**: Allowing the placement of `extra.tf` only in the `dev` and `local` environments.
- **Local Variables**: Write values in each `locals.tf` even if they are identical.
- **Hardcoding Rules**: Hardcode values in `main.tf` to avoid bloating `locals.tf` if they meet all the following:
  - Same value across all environments.
  - (Almost) zero chance of changing during standard operations.
  - Highly likely to remain unchanged when adopted by other products.


## 'modules' Directory Layout

```
{module-name}
├── main.tf
├── outputs.tf
└── variables.tf
```

### Rules

- **File Naming**: Do not create files outside of the allowed file names.


# Exception Rules

## IAM

Created under {module-name}.

```
iam
├── main.tf
├── outputs.tf
└── variables.tf
```

## Lambda

```
lambda
└── {lambda-function-name}
    ├── main.tf
    ├── outputs.tf
    └── variables.tf
```

- All Lambda functions are deployed as ZIP archives, referencing the ZIP files uploaded to the S3 bucket.
- For Go, use the `provided.al2023` runtime and set the handler to `bootstrap`.
