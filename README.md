# CWP-Tools

These tools are designed to help work with CrowdStrikes CWP products and CR registries.

## Secure usage of ENV variables 

`$HOME/.falcon_common` should ***not*** include your sensitive falcon variables such as `FALCON_API_CLIENT_SECRET`

Recommendation is to use a secure vault for the secrets, or some other approved encrypted manner where secrets are not stored at rest unencrypted.

* [secrets](https://github.com/shadowbq/matrix.secrets) - Matrix GPG/RSA secrets method of storing the bash credentials.
* [vault](https://www.vaultproject.io/) - Secure, store and tightly control access to tokens, passwords, certificates, encryption keys for protecting secrets and other sensitive data using a UI, CLI, or HTTP API.


## `falcon_activate`

Sourcing `. bin/falcon_activate` into your shell will create two new aliases 

* `falcon_load_env` - load `$HOME/.falcon_common` into your ENV
* `falcon_unload_env` - unload all `FALCON_*` env variables from your shell

### Falcon Shell Environmental VARs

Sensitive Data sourced via (secrets|vault)

```
env | grep -i FALCON_
FALCON_CID="aaaaaaaaaaaaaaaaa"
FALCON_CID_CHKSUM="${FALCON_CID}-aa"
FALCON_CLIENT_ID=bbbbbbbbbbbbbb
FALCON_CLIENT_SECRET=cccccccccccccccccc
```

`~/.falcon_common` - Safe variables that can be store in a cleartxt environment

```
# Common Falcon ENV
export FALCON_CLOUD_COMMON_NAME="govcloud"
export FALCON_CLOUD_PROPER_NAME="US-GOV-1"
export FALCON_CR="registry.laggar.gcw.crowdstrike.com"
export FALCON_BASE_URL="api.laggar.gcw.crowdstrike.com"

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





Example the example files for how exported variables with the correct names are created in the example files.

![Screenshot](docs/Fetching%20Tags.png?raw=true "Screenshot") <!-- .element height="50%" width="50%" -->
