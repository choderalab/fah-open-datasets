# `fah-open-datasets`: F@H datasets hosted on AWS S3

Open datasets generated with Folding@Home by the Chodera Lab.


## Requirements

Install the [AWS CLI](https://github.com/aws/aws-cli) for your platform.
Though it is possible to use the AWS S3 REST API to download or upload data, the CLI makes this far easier to do in most cases.

In order to download data, **you must have an existing AWS account.**
The `fah-open-datasets` bucket is configured for "requestor pays" in order to be cost-effective for the Chodera Lab.
While the Chodera Lab pays for the costs of hosting the data, downloads of the data are billed to requestor accounts.

Once the AWS CLI is installed, you will need to [generate an access key](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_CreateAccessKey), then set your configuration with:

    aws configure

and follow the prompts.

## Dataset index

See the [index](INDEX.md) for a list of datasets available for use.


## Downloading data from `fah-open-datasets`

**Warning**: This bucket is configured as "requestor pays"; your AWS account will be billed for the request and data download.

Datasets within `fah-open-datasets` can be listed with:

    aws s3 ls --request-payer requester s3://fah-open-datasets

To download a dataset to your local machine, use:

    aws s3 sync --request-payer requester s3://fah-open-datasets/<dataset_name> <local_directory>

If you aren't sure how much data this will amount to, you can use the `--dryrun` flag in the above `sync` command to get an idea.


## Uploading datasets to `fah-open-datasets`

These instructions assume you are on a F@H work server hosted on the Chodera Lab AWS account.

To upload a dataset, use `aws s3 sync`:

    aws s3 sync --acl authenticated-read <local_directory> s3://fah-open-datasets/<dataset_name>

Be sure to add an entry for your dataset to the [index](INDEX.md).


## Uploading from hosts outside of Chodera Lab AWS VPC

To upload a dataset outside of a Chodera Lab AWS VPC, you must possess a user identity within the Chodera Lab AWS account.
The AWS CLI must be configured to use this user identity on your local machine, and you will need to get a temporary session token with

    aws sts get-session-token --serial-number arn:aws:iam::<aws-account-id>:mfa/<user-name> --token-code <MFA-TOKEN>

You can then set the resulting key id, secret key, and token with:

    export AWS_ACCESS_KEY_ID=example-access-key-as-in-previous-output
    export AWS_SECRET_ACCESS_KEY=example-secret-access-key-as-in-previous-output
    export AWS_SESSION_TOKEN=example-session-token-as-in-previous-output

You can then follow the instructions above for upload.
