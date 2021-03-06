# hiveutil

The `hiveutil` CLI offers several commands to help manage clusters with Hive.

To build the `hiveutil` binary, run `make build`.

### Create Cluster

The `create-cluster` command generates a `ClusterDeployment` and submits it to the Hive cluster using your current kubeconfig.

To view what `create-cluster` generates, *without* submitting it to the API server, add `-o yaml` to `create-cluster`. If you need to make any changes not supported by `create-cluster` options, the output can be saved, edited, and then submitted with `oc apply`. This is also a useful way to generate sample yaml.

`--release-image` can be specified to control which OpenShift release image to use.

#### Create Cluster on AWS

Credentials will be read from your AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY environment variables. If the environment variables are missing or empty, then `create-cluster` will look for creds at `~/.aws/credentials`. Alternatively you can specify an AWS credentials file with `--creds-file`.

```bash
bin/hiveutil create-cluster --base-domain=mydomain.example.com --cloud=aws mycluster
```

#### Create Cluster on Azure

Credentials will be read from either `~/.azure/osServicePrincipal.json`, the contents of the `AZURE_AUTH_LOCATION` environment variable, or the value provided with the `--creds-file` parameter (in increasing order of preference). The format for the credentials used for installation/uninstallation follows the same format used by the OpenShift installer:

```
{
  "subscriptionId": "azure-subscription-uuid-here",
  "clientId": "client-id-for-service-principal",
  "clientSecret": "client-secret-for-service-principal",
  "tenantId": "tenant-uuid-here"
}
```

```bash
bin/hiveutil create-cluster --base-domain=mydomain.example.com --cloud=azure --azure-base-domain-resource-group-name=myresourcegroup mycluster
```

NOTE: For deprovisioning a cluster `hiveutil` will use creds from `~/.azure/osServiceAccount.json` or the `AZURE_AUTH_LOCATION` environment variable (with the environment variable prefered).

#### Create Cluster on GCP

Credentials will be read from either `~/.gcp/osServiceAccount.json`, the contents of the `GOOGLE_CREDENTIALS` environment variable, or the value provided with the `--creds-file` parameter (in increasing order of preference). GCP credentials can be created by:

 1. Login to GCP console at https://console.cloud.google.com/
 1. Create a service account with the owner role.
 1. Create a key for the service account.
 1. Select JSON for the key type.
 1. Download resulting JSON file and save to `~/.gcp/osServiceAccount.json`.

```bash
bin/hiveutil create-cluster --base-domain=mydomain.example.com --cloud=gcp mycluster
```

NOTE: For deprovisioning a cluster, `hiveutil` will use creds from `~/.gcp/osServiceAccount.json` or the `GOOGLE_CREDENTIALS` environment variable (with the environment variable prefered).

### Other Commands

To see other commands offered by `hiveutil`, run `hiveutil --help`.
