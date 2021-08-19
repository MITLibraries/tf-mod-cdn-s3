Terraform module to provision an AWS CloudFront CDN with an S3 origin. Originally from CloudPosse's [module](https://github.com/cloudposse/terraform-aws-cloudfront-s3-cdn)

Currently have logging and SSL disabled.

## Requirements

| Name | Version |
|------|---------|
| terraform | >= 0.13 |
| aws | ~> 2.0 |

## Providers

| Name | Version |
|------|---------|
| aws | ~> 2.0 |
| template | n/a |

## Modules

| Name | Source | Version |
|------|--------|---------|
| distribution\_label | github.com/mitlibraries/tf-mod-name | 0.13 |
| origin\_label | github.com/mitlibraries/tf-mod-name | 0.13 |

## Resources

| Name | Type |
|------|------|
| [aws_cloudfront_distribution.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudfront_distribution) | resource |
| [aws_cloudfront_origin_access_identity.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudfront_origin_access_identity) | resource |
| [aws_route53_record.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route53_record) | resource |
| [aws_s3_bucket.origin](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket) | resource |
| [aws_s3_bucket_policy.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_policy) | resource |
| [aws_iam_policy_document.origin](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [aws_region.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/region) | data source |
| [aws_route53_zone.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/route53_zone) | data source |
| [aws_s3_bucket.selected](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/s3_bucket) | data source |
| [template_file.default](https://registry.terraform.io/providers/hashicorp/template/latest/docs/data-sources/file) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| acm\_certificate\_arn | Existing ACM Certificate ARN | `string` | `""` | no |
| aliases | List of FQDN's - Used to set the Alternate Domain Names (CNAMEs) setting on Cloudfront | `list(string)` | `[]` | no |
| allowed\_methods | List of allowed methods (e.g. GET, PUT, POST, DELETE, HEAD) for AWS CloudFront | `list(string)` | ```[ "DELETE", "GET", "HEAD", "OPTIONS", "PATCH", "POST", "PUT" ]``` | no |
| bucket\_domain\_format | Format of bucket domain name | `string` | `"%s.s3.amazonaws.com"` | no |
| cached\_methods | List of cached methods (e.g. GET, PUT, POST, DELETE, HEAD) | `list(string)` | ```[ "GET", "HEAD" ]``` | no |
| comment | Comment for the origin access identity | `string` | `"Managed by Terraform"` | no |
| compress | Compress content for web requests that include Accept-Encoding: gzip in the request header | `bool` | `"false"` | no |
| cors\_allowed\_headers | List of allowed headers for S3 bucket | `list(string)` | ```[ "*" ]``` | no |
| cors\_allowed\_methods | List of allowed methods (e.g. GET, PUT, POST, DELETE, HEAD) for S3 bucket | `list(string)` | ```[ "GET" ]``` | no |
| cors\_allowed\_origins | List of allowed origins (e.g. example.com, test.com) for S3 bucket | `list(string)` | `[]` | no |
| cors\_expose\_headers | List of expose header in the response for S3 bucket | `list(string)` | ```[ "ETag" ]``` | no |
| cors\_max\_age\_seconds | Time in seconds that browser can cache the response for S3 bucket | `number` | `"3600"` | no |
| custom\_error\_response | List of one or more custom error response element maps | ```list(object({ error_caching_min_ttl = string error_code = string response_code = string response_page_path = string }))``` | `[]` | no |
| default\_root\_object | Object that CloudFront return when requests the root URL | `string` | `"index.html"` | no |
| default\_ttl | Default amount of time (in seconds) that an object is in a CloudFront cache | `number` | `"60"` | no |
| enabled | Select Enabled if you want CloudFront to begin processing requests as soon as the distribution is created, or select Disabled if you do not want CloudFront to begin processing requests after the distribution is created. | `string` | `"true"` | no |
| evaluate\_target\_health | Set to true if you want Route 53 to determine whether to respond to DNS queries | `bool` | `"false"` | no |
| ext\_aliases | List of External FQDN's (e.g. a .mit.edu CNAME)- Used to set the Alternate Domain Names (CNAMEs) setting on Cloudfront | `list(string)` | `[]` | no |
| forward\_cookies | Time in seconds that browser can cache the response for S3 bucket | `string` | `"none"` | no |
| forward\_header\_values | A list of whitelisted header values to forward to the origin | `list(string)` | ```[ "Access-Control-Request-Headers", "Access-Control-Request-Method", "Origin" ]``` | no |
| forward\_query\_string | Forward query strings to the origin that is associated with this cache behavior | `bool` | `"false"` | no |
| geo\_restriction\_locations | List of country codes for which  CloudFront either to distribute content (whitelist) or not distribute your content (blacklist) | `list(string)` | `[]` | no |
| geo\_restriction\_type | Method that use to restrict distribution of your content by country: `none`, `whitelist`, or `blacklist` | `string` | `"none"` | no |
| is\_ipv6\_enabled | State of CloudFront IPv6 | `bool` | `"true"` | no |
| log\_expiration\_days | Number of days after which to expunge the objects | `number` | `"90"` | no |
| log\_glacier\_transition\_days | Number of days after which to move the data to the glacier storage tier | `number` | `"60"` | no |
| log\_include\_cookies | Include cookies in access logs | `bool` | `"false"` | no |
| log\_prefix | Path of logs in S3 bucket | `string` | `""` | no |
| log\_standard\_transition\_days | Number of days to persist in the standard storage tier before moving to the glacier tier | `number` | `"30"` | no |
| max\_ttl | Maximum amount of time (in seconds) that an object is in a CloudFront cache | `number` | `"31536000"` | no |
| min\_ttl | Minimum amount of time that you want objects to stay in CloudFront caches | `number` | `"0"` | no |
| minimum\_protocol\_version | Cloudfront TLS minimum protocol version | `string` | `"TLSv1"` | no |
| name | Name  (e.g. `bastion` or `app`) | `string` | n/a | yes |
| null | an empty string | `string` | `""` | no |
| origin\_bucket | Name of S3 bucket | `string` | `""` | no |
| origin\_force\_destroy | Delete all objects from the bucket  so that the bucket can be destroyed without error (e.g. `true` or `false`) | `bool` | `"false"` | no |
| origin\_path | An optional element that causes CloudFront to request your content from a directory in your Amazon S3 bucket or your custom origin. It must begin with a /. Do not add a / at the end of the path. | `string` | `""` | no |
| parent\_zone\_id | ID of the hosted zone to contain this record  (or specify `parent_zone_name`) | `string` | `""` | no |
| parent\_zone\_name | Name of the hosted zone to contain this record (or specify `parent_zone_id`) | `string` | `""` | no |
| price\_class | Price class for this distribution: `PriceClass_All`, `PriceClass_200`, `PriceClass_100` | `string` | `"PriceClass_100"` | no |
| static\_s3\_bucket | aws-cli is a bucket owned by amazon that will perminantly exist. It allows for the data source to be called during the destruction process without failing. It doesn't get used for anything else, this is a safe workaround for handling the fact that if a data source like the one `aws_s3_bucket.selected` gets an error, you can't continue the terraform process which also includes the 'destroy' command, where is doesn't even need this data source! Don't change this bucket name, it's a variable so that we can provide this description. And this works around a problem that is an edge case. | `string` | `"aws-cli"` | no |
| tags | Additional tags (e.g. map(`BusinessUnit`,`XYZ`) | `map(string)` | `{}` | no |
| use\_regional\_s3\_endpoint | When set to 'true' the s3 origin\_bucket will use the regional endpoint address instead of the global endpoint address | `string` | `"false"` | no |
| viewer\_protocol\_policy | allow-all, redirect-to-https | `string` | `"redirect-to-https"` | no |

## Outputs

| Name | Description |
|------|-------------|
| cf\_arn | ARN of AWS CloudFront distribution |
| cf\_domain\_name | Domain name corresponding to the distribution |
| cf\_etag | Current version of the distribution's information |
| cf\_hosted\_zone\_id | CloudFront Route 53 zone ID |
| cf\_id | ID of AWS CloudFront distribution |
| cf\_status | Current status of the distribution |
| s3\_bucket | Name of S3 bucket |
| s3\_bucket\_arn | ARN of S3 bucket |
| s3\_bucket\_domain\_name | Domain of S3 bucket |