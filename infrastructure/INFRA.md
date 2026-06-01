# Infrastructure

```mermaid
---
title: example
config:
  theme: neutral
  flowchart:
    nodeSpacing: 10
    rankSpacing: 30
---
flowchart LR

internet@{img: "https://api.iconify.design/material-symbols/globe-asia.svg",label: "internet",pos: "b",w: 60,h: 60,constraint: "on"}

subgraph aws["aws(us-east-1)"]
  subgraph edge["Edge & Distribution"]
    subgraph group-route53[" "]
      route53@{img: "https://api.iconify.design/logos/aws-route53.svg",label: "Route53",pos: "b",w: 60,h: 60,constraint: "on"}
    end
    subgraph group-cloudfront[" "]
      cloudfront@{img: "https://api.iconify.design/logos/aws-cloudfront.svg",label: "CloudFront",pos: "b",w: 60,h: 60,constraint: "on"}
    end
  end
  subgraph store["Data Stores"]
    subgraph group-s3[" "]
      s3-hosting@{img: "https://api.iconify.design/logos/aws-s3.svg",label: "S3<br/>Frontend",pos: "b",w: 60,h: 60,constraint: "on"}
      s3-terraform-state@{img: "https://api.iconify.design/logos/aws-s3.svg",label: "S3<br/>TerraformState",pos: "b",w: 60,h: 60,constraint: "on"}
    end
    subgraph group-database[" "]
      dynamodb@{img: "https://api.iconify.design/logos/aws-dynamodb.svg",label: "DynamoDB",pos: "b",w: 60,h: 60,constraint: "on"}
    end
  end
  subgraph security["Security & Auth"]
    subgraph group-security[" "]
      waf@{img: "https://api.iconify.design/logos/aws-waf.svg",label: "WAF",pos: "b",w: 60,h: 60,constraint: "on"}
      cognito@{img: "https://api.iconify.design/logos/aws-cognito.svg",label: "Cognito",pos: "b",w: 60,h: 60,constraint: "on"}
    end
  end
  subgraph api["API & Compute"]
    subgraph group-api-gateway[" "]
      api-gateway@{img: "https://api.iconify.design/logos/aws-api-gateway.svg",label: "API Gateway",pos: "b",w: 60,h: 60,constraint: "on"}
    end
    subgraph group-lambda-authorizer[" "]
      lambda-authorizer@{img: "https://api.iconify.design/logos/aws-lambda.svg",label: "Lambda<br/>Authorizer",pos: "b",w: 60,h: 60,constraint: "on"}
    end
    subgraph group-lambda-api[" "]
      lambda-bff@{img: "https://api.iconify.design/logos/aws-lambda.svg",label: "Lambda<br/>BFF",pos: "b",w: 60,h: 60,constraint: "on"}
    end
  end
end


internet ----- route53 --- cloudfront
cloudfront --- waf
cloudfront --- s3-hosting

cloudfront --- api-gateway
api-gateway --- waf

api-gateway --- lambda-authorizer
api-gateway --- lambda-bff --- dynamodb
lambda-authorizer --- cognito
lambda-bff --- cognito


classDef default fill:#fff
style aws fill:#fff,color:#345,stroke:#345
classDef group fill:none,stroke:none
classDef vpc fill:#fff,color:#0a0,stroke:#0a0
class vpc-vvvv1 vpc
class stripe,group-route53,group-cloudfront,group-s3,group-database,group-security,group-lambda-authorizer,group-lambda-api,group-lambda,group-api-gateway group
```
