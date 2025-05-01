---
title: "The internet infrastructure meme happened to me!"
layout: single
excerpt: "although it was snowflake and not some guy in Nebraska"
tags: [til,data]
---

Today I experienced this famous xkcd [meme](https://xkcd.com/2347/) for the first time in production

![](https://imgs.xkcd.com/comics/dependency_2x.png)

We noticed our data wasn't being transferred from s3 to snowflake, with the error saying

```
The certificate is revoked or could not be validated: hostname=sfc-eu-ds1-customer-stage.s3.amazonaws.com
```

After quickly asking ChatGPT how SSL certificates work, I found this GitHub issue with the [certifi](https://github.com/certifi/python-certifi) package. They had pushed new SSL certificates which exposed a bug in how snowflake authenticated such certificates.

> Hey all, Snowflake Engineering team here. We acknowledge that the impact to Snowflake-to-S3 use cases here stems from a bug in our implementation of chain validation in the Snowflake Python connector. The code ought to validate against the Amazon Root CA trust anchor instead of checking for the validity of all the certificates in the certificate chain. We are [fixing](https://github.com/snowflakedb/snowflake-connector-python/commit/243133602626bd38985b6ff44f4e6a8f5fa4bca3) this in the Python Connector and are on track to release a patch by Apr 30.

As they explained, snowflake is checking the validity of all certificates in the chain, rather than just the root. 

To be fair, they fixed the issue in less than 3 days but I now appreciate internet infrastructure better!
