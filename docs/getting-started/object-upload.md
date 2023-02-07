---
layout: default
title: Object Upload
parent: Getting Started
nav_order: 2
---

# Object Upload

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

If you want to play with the P3 service. The [curl](https://curl.se/) command can give you a quick start. Objects can be uploaded to P3 using this command line tool. Here is an example:

```bash
curl -X PUT http://p3.photon.storage:13000/gateway/v1/[OBJECT_KEY] \
     -H "x-p3-unixtime: [UNIX_TIMESTAMP]" \
     -H "x-p3-bucket: photon-archive-test" \
     -H "x-p3-content-md5: 0e7104bd7848d22bbc8adb6bed03f3a4" \
     -H "x-p3-content-type: application/octet-stream" \
     -H "Content-Type: application/octet-stream" \
     -H "Authorization: [YOUR_ACCESS_KEY_ID]:[SIGNATURE]" \
     --data-binary "@[LOCAL_FILE_PATH]"
```

You need to fill in desired target key `OBJECT_KEY`, current unix timestamp `UNIX_TIMESTAMP`, authentication token `YOUR_ACCESS_KEY_ID` and `SIGNATURE`, and local path to file being uploaded `LOCAL_FILE_PATH`.

It is a bit hassle to calculate data MD5 and the authentication signature. Here is a [tool](https://github.com/photon-storage/p3-sdk-go/blob/main/cmd/curl/main.go) that can generate a curl command line for given parameters.

## Go

If you are developing with P3, there is a Go SDK available [here](https://github.com/photon-storage/p3-sdk-go/blob/main/p3/client.go). Here is an example to upload an object using the SDK

```go
import "github.com/photon-storage/p3-sdk-go/p3"

func upload() error {
    cli := p3.New()
    if _, err := cli.PutObject(
        "your-p3-bucket",
        "your-object-key",
        []byte("hello-world!"),
    ); err != nil {
        return err
    }

    fmt.Printf("object uploaded successfully.\n")
    return nil
}
```
