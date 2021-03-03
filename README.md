# Next website AWS S3

An Ansible role for publishing a Next.js website on AWS S3 as a static website

## Requirements

This role depends on multiple AWS modules so you will need to install the `boto` python libraries in the ansible python environment. These are the versions that the role is tested with:

```
boto==2.49
boto3==1.10
botocore==1.13
```

## Role Variables

### Required Variables

These variables must be set either as part of a play or passed when invoking `ansible-playbook`. If these variables are not set, the role will fail.

- `bucket_name` - The name of the S3 bucket to upload the website's static files to. This will also be the domain for the created DNS record
  - e.g. `test.benpartridge.me`
- `r53_hosted_zone` - The name of the AWS Route53 hosted zone to create DNS records for the website in
  - e.g. `benpartridge.me`

### Optional Variables

These variables have default values but you may need to override them for your specific use case

- `aws_region` - The AWS region to make requests from. This should be the region your S3 bucket is hosted in. Defaults to `ap-southeast-2`
  - e.g. `us-east-1`
- `website_root_dir` - The path to the root directory of your Next.js website. This should be the directory with the `package.json` file in it. Defaults to current working directory.
  - e.g. `./frontend`

### Other Variables

These are variables that you probably won't need to change unless you have very specific requirements (for example your AWS region isn't supported by this role).

- `s3_region_to_hosted_zone_map` - A map which maps an AWS region to the hosted zone and domain of the AWS S3 website endpoint for that region. This is required for creating S3 website Route53 records. Currently the only supported region is `ap-southeast-2`. You can find out more about S3 endpoints from the [AWS S3 Documentation](https://docs.aws.amazon.com/general/latest/gr/s3.html#s3_website_region_endpoints). For example:

```
s3_region_to_hosted_zone_map:
  ap-southeast-2:
    hosted_zone_id: Z1WCIGYICN2BYD
    endpoint: s3-website-ap-southeast-2.amazonaws.com
  us-east-1:
    hosted_zone_id: Z3AQBSTGFYJSTF
    endpoint: s3-website-us-east-1.amazonaws.com
```

## Example Playbook

Typically this playbook only needs to run on localhost as there are no remote machines to configure, only API calls and a local build.

```
---
- name: Deploy to test.benpartridge.me
  hosts: localhost
  connection: local

  vars:
    bucket_name: test.benpartridge.me
    aws_region: ap-southeast-2
    r53_hosted_zone: benpartridge.me
    website_root_dir: ../frontend

  roles:
    - next-website
```

## License

TODO: open source?

## Author Information

Author: Ben Partridge

I'm a software engineer who also likes to dabble in game development. Check out my work on my website and Follow me on twitter for updates on what I'm working on.

- [Website](http://benpartridge.me) (deployed using this role!)
- [Twitter](https://twitter.com/@BenPartridge249)
