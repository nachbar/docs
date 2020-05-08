The Amplify Storage category provides an interface for managing user content for your app in public, protected, or private storage buckets. The Storage category comes with default built-in support for Amazon Simple Storage Service (S3). The Amplify CLI helps you to create and configure the storage buckets for your app. The Amplify AWS S3 Storage plugin leverages [Amazon S3](https://aws.amazon.com/s3).

## Goal
To setup and configure your application with Amplify Storage and go through a simple upload file example

## Prerequisites

<inline-fragment platform="ios" src="~/lib/storage/fragments/ios/hello-storage/10_preReq.md"></inline-fragment>
<!-- TODO Android -->
<!--<inline-fragment platform="android" src="~/lib/storage/fragments/android/hello-storage/20_preReq.md"></inline-fragment>-->

## Provision Backend Storage Services

To start provisioning storage resources in the backend, go to your project directory and **execute the command**:

```bash
amplify add storage
```

Enter the following when prompted:
```console
? Please select from one of the below mentioned services:
    `Content (Images, audio, video, etc.)`
? You need to add auth (Amazon Cognito) to your project in order to add storage for user files. Do you want to add auth now?
    `Yes`
? Do you want to use the default authentication and security configuration?
    `Default configuration`
? How do you want users to be able to sign in?
    `Username`
? Do you want to configure advanced settings?
    `No, I am done.`
? Please provide a friendly name for your resource that will be used to label this category in the project:
    `S3friendlyName`
? Please provide bucket name:
    `storagebucketName`
? Who should have access:
    `Auth and guest users`
? What kind of access do you want for Authenticated users?
    `create/update, read, delete`
? What kind of access do you want for Guest users?
    `create/update, read, delete`
? Do you want to add a Lambda Trigger for your S3 Bucket?
    `No`
```

To push your changes to the cloud, **execute the command**:

```bash
amplify push
```

Upon completion, `awsconfiguration.json` and `amplifyconfiguration.json` should be updated to reference provisioned backend storage resources.  Note that these files should already be a part of your project if you followed the [Getting Started with Amplify](~/lib/storage/getting-started-amplify.md) example.

## Install Amplify Libraries
<inline-fragment platform="ios" src="~/lib/storage/fragments/ios/hello-storage/20_installLib.md"></inline-fragment>
<!-- TODO Android -->
<!--<inline-fragment platform="android" src="~/lib/storage/fragments/android/hello-storage/20_installLib.md"></inline-fragment>-->

## Initialize Amplify Storage
<inline-fragment platform="ios" src="~/lib/storage/fragments/ios/hello-storage/30_initStorage.md"></inline-fragment>
<!-- TODO Android -->
<!--<inline-fragment platform="android" src="~/lib/storage/fragments/android/hello-storage/30_initStorage.md"></inline-fragment>-->

## Uploading data to your bucket

To upload to S3 from a data object, specify the key and the data object to be uploaded.

<inline-fragment platform="ios" src="~/lib/storage/fragments/ios/hello-storage/40_upload.md"></inline-fragment>
<!-- TODO Android -->
<!--<inline-fragment platform="android" src="~/lib/storage/fragments/android/hello-storage/40_upload.md"></inline-fragment>-->

Upon successfully executing this code, you should see an file called `myKey` in a folder called `public` with the data `My Data` in your S3 bucket.

## Next Steps
Congratulations! You've uploaded a file to an s3 bucket.  Check out the following links to see other Amplify Storage use cases:

* [Configure file access levels](~/lib/storage/configureaccess.md)
* [Download files](~/lib/storage/download.md)
* [List Files](~/lib/storage/list.md)
* [Remove files](~/lib/storage/remove.md)
* [Using Lambda Triggers](~/lib/storage/triggers.md)
* [Escape Hatch](~/lib/storage/escapehatch.md)