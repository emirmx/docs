---
title: "Amazon S3 SDK Upgrade"
url: /refguide10/amazon-s3-sdk-upgrade/
description: "Describes breaking changes cause by Amazon S3 SDK library upgrade."
---

### Amazon S3 SDK Upgrade

In Mendix 9.24.41 we upgraded the AWS SDK used for accessing S3 storage from version 1 to version 2. SDK version 2 has some [differences](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/migration-s3.html) which affects our S3 storage implementation.

#### `com.mendix.storage.s3.Region` / `com.mendix.storage.s3.EndPoint` Settings

SDK version 2 is stricter with these settings.

The `com.mendix.storage.s3.Region` setting must always be set to the region matching the region of the bucket.

The `com.mendix.storage.s3.EndPoint` setting must either not be set or set to an endpoint matching the region, for example: `s3.eu-west-1.amazonaws.com`.

When the region is not specified or there is an incompatibility between the two settings above, error logs will contain entries similar to following:

```
- Unable to load region from any of the providers in the chain.
- The bucket you are attempting to access must be addressed using the specified endpoint.
- The authorization header is malformed; the region 'us-east-1' is wrong.
```

#### AWS Signature V2 Support (`com.mendix.storage.s3.UseV2Auth` Setting)

SDK version 2 does not support AWS Signature v2 which is enabled by `UseV2Auth` setting. This signature type is deprecated, and is not supported by new regions. For more information, see [AWS's Documentation](https://docs.aws.amazon.com/AmazonS3/latest/API/specify-signature-version.html).

We do not expect this to have any effect when using Amazon S3. It will, however, prevent use of S3 compatible solutions which only support the v2 signature type. In situations like that, you need to switch to either Amazon S3 or a compatible solution that supports newer signature types.

#### Client Side Encryption Changes

Client side encryption can be enabled using the `com.mendix.storage.s3.EncryptionKeys` setting. Previously, any encryption algorithm supported by the JDK could be used. With the new SDK only AES is supported.

An error similar to the following will be printed in logs when an algorithm other than AES is used:

```
- Unsupported algorithm: DES
```

If you use an encryption algorithm other than `AES`, then all existing files should be migrated to use `AES` before upgrading to Mendix 11.6. This can be done by configuring a new `AES` key and rewriting all file documents.
