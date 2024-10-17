## Acknowledgements

This project is based on [fastify-s3-buckets](https://github.com/kibertoad/fastify-s3-buckets.git) by [kibertoad](https://github.com/kibertoad).


# fastify-s3
Fastify v5 plugin for ensuring existence of defined AWS S3 buckets on the application startup.

[![NPM Version](https://img.shields.io/npm/v/fastify-s3.svg)](https://npmjs.org/package/fastify-s3)
[![Build Status](https://github.com/mamirul47/fastify-s3/workflows/ci/badge.svg)](https://github.com/mamirul47/fastify-s3/actions)

## How to use?

```bash
npm install fastify-s3 @aws-sdk/client-s3 
```
Create file s3.ts ( if using fastify-cli to generate project please create file inside src/plugins/s3.ts )

```ts
import fp from 'fastify-plugin'
import { fastifyS3BucketsPlugin, BucketConfiguration } from 'fastify-s3';
import { S3Client , S3ClientConfigType } from '@aws-sdk/client-s3'

export default fp<BucketConfiguration>(async (fastify) => {
    const s3Config : S3ClientConfigType = {
        endpoint: 'https://<host>:<port>',
        forcePathStyle: true,
        region: '<region>',
        credentials: {
            accessKeyId: '<access_key>',
            secretAccessKey: '<secret_access_key>',
        },
    }
    const s3Client = new S3Client(s3Config)
    fastify.register(fastifyS3BucketsPlugin, {
        s3Client,
        buckets: [
            { Bucket: '<bucket_name>', ACL: 'public-read-write' },
        ]
    })
})

```

Note that if buckets already exist, they will not be recreated.
Existing buckets that are not specified will not be deleted.
