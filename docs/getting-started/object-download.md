---
layout: default
title: Object Download
parent: Getting Started
nav_order: 3
---

# Object Download

Objects can be downloaded from P3 through its REST endpoint using `GET` method. The endpoint URL has the form `http://p3.photon.storage/gateway/v1/KEY`, where `KEY` is the P3 storage path for the object to download.

Object is treated as byte array and received through the HTTP response body. The following table shows a list of additional headers to define for a request.

| Name              | Required | Usage                           |
|:------------------|:---------|:--------------------------------|
| x-p3-bucket       |  Yes     | Target bucket to upload object. |
| x-p3-unixtime     |  Yes     | Unix timestamp at the time of the request. Fallback to `Date` if missingr. |
| Authorization     |  Yes     | See [Authenication](/docs/getting-started/auth.html) |
| Date              |  No      | Fallback for `x-p3-unixtime` |

## CLI

If you want to play with the P3 service. The [curl](https://curl.se/) command can give you a quick start. Objects can be downloaded from P3 using this command line tool. Here is an example:

```bash
curl -X GET http://p3.photon.storage:13000/gateway/[OBJECT_KEY] \
     -H "x-p3-bucket: [OBJECT_BUCKET]" \
     -H "x-p3-unixtime: [UNIX_TIMESTAMP]" \
     -H "Authorization: [YOUR_ACCESS_KEY_ID]:[SIGNATURE]" \
     --output [LOCAL_FILE_PATH]
```

You need to fill in desired target bucket `OBJECT_BUCKET`, key `OBJECT_KEY`, current unix timestamp `UNIX_TIMESTAMP`, authentication token `YOUR_ACCESS_KEY_ID` and `SIGNATURE`, and local path to save downloaded data `LOCAL_FILE_PATH`.

It is a bit hassle to calculate data MD5 and the authentication signature. Here is a [tool](https://github.com/photon-storage/p3-sdk-go/blob/main/cmd/curl/main.go) that can generate a curl command line for given parameters.

## Go

If you are developing with P3, there is a Go SDK available [here](https://github.com/photon-storage/p3-sdk-go/blob/main/p3/client.go). Here is an example to download an object using the SDK

```go
import "github.com/photon-storage/p3-sdk-go/p3"

func download() error {
    cli := p3.New()
    data, err := cli.GetObject(
        "your-p3-bucket",
        "your-object-key",
    ); err != nil {
        return err
    }

    // Downloaded content is returned by data as []byte
    fmt.Printf("object downloaded successfully.\n")

    return nil
}
```
