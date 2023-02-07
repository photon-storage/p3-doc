---
layout: default
title: Object Download
parent: Getting Started
nav_order: 3
---

# Object Download

Objects can be uploaded to P3 through its REST endpoint using `PUT` method. The endpoint URL has the form `http://p3.photon.storage/gateway/v1/KEY`, where `KEY` is the P3 storage path for the object being uploaded.

Object is treated as byte array and sent by the `PUT` request body. The following table shows a list of additional headers to define for a request.

| Name              | Required | Usage                           |
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

Object can be uploaded to P3 using [curl](https://curl.se/) command. Here is an example:
```bash
curl -X PUT http://p3.photon.storage:13000/gateway/v1/[OBJECT_PATH] \
     -H "X-P3-Unixtime: [UNIX_TIMESTAMP]" \
     -H "Authorization: [YOUR_ACCESS_KEY_ID]:[SIGNATURE]" \
     -H "X-P3-Bucket: photon-archive-test" \
     -H "X-P3-Content-Md5: 0e7104bd7848d22bbc8adb6bed03f3a4" \
     -H "X-P3-Content-Type: application/octet-stream" \
     -H "Content-Type: application/octet-stream" \
     --data-binary "@[LOCAL_FILE_PATH]"
```

```bash
curl -X GET http://p3.photon.storage:13000/gateway/[OBJECT_KEY] \
     -H "X-P3-Bucket: [OBJECT_BUCKET]" \
     -H "X-P3-Unixtime: 1675753418" \
     -H "Authorization: YOUR_ACCESS_KEY_ID:RvcWRm6W2Q1T5wnglzu0n4p4F10="
```
