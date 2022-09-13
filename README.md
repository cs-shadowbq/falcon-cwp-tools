# CWP-Tools

These tools are designed to help work with CrowdStrikes CWP products and CR registries.

## Warning (Proof of Concept)

This is *not a secure method* of storing secets in an unencrypted bash shell. This is meant as a demostration purpose only on how one might load environmental variables into the shell for processing. 

Recommendation is to use a secure vault for the secrets, or some other approved encrypted manner where secrets are not stored at rest unencrypted.

* [Secrets](https://github.com/shadowbq/matrix.secrets) - Matrix GPG/RSA secrets method of storing the bash credentials.
* [vault](https://www.vaultproject.io/) - Secure, store and tightly control access to tokens, passwords, certificates, encryption keys for protecting secrets and other sensitive data using a UI, CLI, or HTTP API.


## `falcon_activate`

Sourcing `. bin/falcon_activate` into your shell will create two new aliases that allow for loading and unloading of the environmental bash variables stored in the following files.

```
~/.falcon_common
~/.falcon_secrets 
```

### Falcon Secrets

`~/.falcon_common`

```
# Common Falcon ENV
export FALCON_CLOUD_COMMON_NAME="govcloud"
export FALCON_CLOUD_PROPER_NAME="US-GOV-1"
export FALCON_CR="registry.laggar.gcw.crowdstrike.com"
export FALCON_API_URL="api.laggar.gcw.crowdstrike.com"

# K8s Settings
export FALCON_DS_NAMESPACE="falcon-system"
export FALCON_IMAGE_SECRET="cs-registry-secret"

# Falcon Sensor
export FALCON_SENSOR_VERSION="6.44.0-2701"
# Helm chart uses falcon.image.tag "latest" by default
export FALCON_IMAGE_TAG="${FALCON_SENSOR_VERSION}.falcon-linux.x86_64.Release.${FALCON_CLOUD_PROPER_NAME}"


# USE CrowdStrike falcon-sensor or container-sensor
export FALCON_SENSOR_TYPE="falcon-sensor"
```

`~/.falcon_secrets` 

```
# Falcon SECRET Information
export FALCON_CID="aaaaaaaaaaaaaaaaa"
export FALCON_CID_CHKSUM="${FALCON_CID}-aa"
export FALCON_API_CLIENT_ID=bbbbbbbbbbbbbb
export FALCON_API_CLIENT_SECRET=cccccccccccccccccc
```



Example the example files for how exported variables with the correct names are created in the example files.

![Screenshot](docs/Fetching%20Tags.png?raw=true "Screenshot") <!-- .element height="50%" width="50%" -->
