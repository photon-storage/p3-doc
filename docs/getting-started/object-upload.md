---
layout: default
title: Object Upload
parent: Getting Started
nav_order: 2
---

# Upload Objects

Objects can be uploaded to P3 through its REST endpoint using `PUT` method. The endpoint URL has the form `http://p3.photon.storage/gateway/v1/KEY`, where `KEY` is the P3 storage path for the object being uploaded.

Object is treated as byte array and sent by the `PUT` request body. The following table shows a list of additional parameters to define in request header.

| Parameter Name    | Required | Usage                           |
|:------------------|:---------|:--------------------------------|
| x-p3-bucket       |  Yes     | Target bucket to upload object. |
| x-p3-content-md5  |  Yes     | MD5 digest of the object data. Fallback to `Content-MD5` if missing. |
| x-p3-content-type |  No      | Content MIME type of the object data. Fallback to `Content-Type` if missing. |
| x-p3-unixtime     |  Yes     | Unix timestamp at the time of the request. Fallback to `Date` if missingr. |
| Authorization     |  Yes     | See [Authenication](/docs/getting-started/auth.html) |
| Content-MD5       |  No      | Fallback for `x-p3-content-md5` |
| Content-Type      |  No      | Fallback for `x-p3-content-type` |
| Date              |  No      | Fallback for `x-p3-unixtime` |

## CLI
