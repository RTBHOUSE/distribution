---
description: Explains how to use the Google Cloud Storage drivers
keywords: registry, service, driver, images, storage,  gcs, google, cloud
title: Google Cloud Storage driver
---

An implementation of the `storagedriver.StorageDriver` interface which uses Google Cloud for object storage.

## Parameters

| Parameter     | Required | Description |
|:--------------|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `bucket`  | yes | The name of your Google Cloud Storage bucket where you wish to store objects (needs to already be created prior to driver initialization). |
| `keyfile`  | no | A private service account key file in JSON format used for [Service Account Authentication](https://cloud.google.com/storage/docs/authentication#service_accounts). |
| `defaultcredentials` | no | Additional configuration that applies when [Google Application Default Credentials](https://developers.google.com/identity/protocols/application-default-credentials) are used, `redirect` enables serving blobs with redirects, `email` allows to override email that is used for signing redirects URLs. |
| `rootdirectory`  | no | The root directory tree in which all registry files are stored. Defaults to the empty string (bucket root). If a prefix is used, the path `bucketname/<prefix>` has to be pre-created before starting the registry. The prefix is applied to all Google Cloud Storage keys to allow you to segment data in your bucket if necessary.|
| `chunksize`  | no (default 5242880) | This is the chunk size used for uploading large blobs, must be a multiple of 256*1024. |

{{< hint type=note >}}
Instead of a key file you can use [Google Application Default Credentials](https://developers.google.com/identity/protocols/application-default-credentials).

To use redirects with this mode you have to set `defaultcredentials.redirect` to `true` and optionally set `defaultcredentials.email` if the service account email is not detected by GCP client library properly, or is not the one you want to use. When using default credentials assigned to a virtual machine you have to enable "IAM Service Account Credentials API" and grant `iam.serviceAccounts.signBlob` permission on the service account used for signing - under the hood GCP client will use [signBlob](https://cloud.google.com/iam/docs/reference/credentials/rest/v1/projects.serviceAccounts/signBlob) IAM API, you may also want to set `defaultcredentials.email` to avoid fetching the email from the metadata server for every signature.
{{< /hint >}}
