---
title: Alternate installation settings
linkTitle: Alternate installation settings
weight: 10
date: 2021-11-30
description: Learn how to customize your installation according to your needs.
---

## Helm parameters management

There are different ways to manage your custom [Helm parameters](/docs/install/helm-parameters/). The best way depends on your use case.

For example, you could:

* Use `--set key=value` when running the `helm install` or `helm upgrade` command.
    * Example: `helm install <name> <chart> --set key=value`.
* Edit `values.yaml` or `values-ha.yaml` files and change any values you need.
* (recommanded) Create a custom values file (for example, `my-values.yaml`) where you can overwrite parameters and pass on the file to `helm install` or `helm upgrade` command.
    * Example: `helm install -f values.yaml -f values-ha.yaml -f my-values.yaml <name> <chart>`. (The last `values` file in this command overwrites any conflicting parameter.)

After you choose one of the options, we recommend you always use it to avoid issues when you [upgrade the helm chart](/docs/install/upgrade/).

## Use a custom Docker registry

The following code is an example of how to create a Kubernetes registry:

```sh
export NAMESPACE="my-namespace"
export REGISTRY_SECRET_NAME="my-registry-secret-name"
export REGISTRY_SERVER="my-registry-server"
export REGISTRY_USERNAME="my-registry-username"
export REGISTRY_PASSWORD="my-registry-password"

kubectl create secret docker-registry "${REGISTRY_SECRET_NAME}" --docker-server="${REGISTRY_SERVER}"  --docker-username="${REGISTRY_USERNAME}" --docker-password="${REGISTRY_PASSWORD}" -n "${NAMESPACE}"
```

To use your Kubernetes Secret in the registry, set the Secret's name in the `imagePullSecrets` array. For example, add `--set imagePullSecrets[0].name="${REGISTRY_SECRET_NAME}"` to the Helm chart installation command.

You must also set `images.repository` accordingly to your custom registry. For more information, see [Streams parameters](/docs/install/helm-parameters#streams-parameters).

## Custom embedded Mariadb security settings

By default, both TLS and TDE are enabled for the embedded mariadb, however you may want to disable one or the other or both. In order to do so, please follow the one of the procedures bellow :

* For enabling TLS only:

To **only** enable TLS, you need to create a secret containing the [TLS](#tls) certificates, for example:

```sh
export NAMESPACE="my-namespace"
kubectl create secret generic streams-database-secret --from-file=CA_PEM=ca.pem --from-file=SERVER_CERT_PEM=server-cert.pem --from-file=SERVER_KEY_PEM=server-key.pem -n ${NAMESPACE}
```

You must also set the [Helm parameters](/docs/install/helm-parameters/) `embeddedMariadb.encryption.enabled` to `false`.

* For enabling TDE only:

To **only** enable TDE, you need to create a secret containing the [TDE](#transparent-data-encryption-tde) keyfile, for example:

```sh
export NAMESPACE="my-namespace"
kubectl create secret generic streams-database-secret --from-file=KEYFILE=keyfile -n ${NAMESPACE}
```

You must also set the [Helm parameters](/docs/install/helm-parameters/) `embeddedMariadb.tls.enabled` to `false`.

* For disabling all security features

To disable MariaDB encryption **and** TLS, you must set the following [Helm parameters](/docs/install/helm-parameters/):

* `embeddedMariadb.tls.enabled` and `embeddedMariadb.encryption.enabled` to `false`
* `embeddedMariadb.master.extraEnvVarsSecret` and `embeddedMariadb.slave.extraEnvVarsSecret` to `null`

{{< alert title="Note" >}}
Disabling security is not recommended for production environments.
{{< /alert >}}

## Custom embedded kafka security settings

Currently, Streams works only with SASL/SCRAM authentication (using the SHA-512 hash functions) **and** TLS enabled or neither of the two.

To disable all security features provided by kafka, you must set the following [Helm parameters](/docs/install/helm-parameters/):

* `embeddedKafka.auth.clientProtocol` to `plaintext`
* `embeddedKafka.auth.interBrokerProtocol` to `plaintext`
* `embeddedKafka.auth.sasl.mechanisms` to `plain`
* `embeddedKafka.auth.sasl.interBrokerMechanism` to `plain`
* `embeddedKafka.auth.sasl.jaas.clientPasswordSecret` to `null`
* `embeddedKafka.extraEnvVars` to `null`
* `embeddedKafka.extraVolumes` to `null`
* `embeddedKafka.extraVolumeMounts` to `null`

{{< alert title="Note" >}}
Disabling security is not recommended for production environments.
{{< /alert >}}