# CrowdStrike Falcon Cloud Workload Protection DevSecOp Tools

These tools are designed to help work with CrowdStrikes CWP products, the multiple helm charts and Container Registries.

For my general crowdstrike python tools: https://github.com/cs-shadowbq/falconpy-tools

CR Scanning CLI tool - https://github.com/CrowdStrike/container-image-scan/blob/main/README.md

## Reference Repositories

* CrowdStrike Helm Charts 
  + Falcon Sensor  - A Falcon Sensor deployment via a container for DEVSECOPS into Kubernetes.
     https://github.com/CrowdStrike/falcon-helm/tree/main/helm-charts/falcon-sensor  
     https://crowdstrike.github.io/falcon-helm  
     The falcon sensor public helm chart is a public helm chart that references the helm.tar.gz file in the deployment section of the public gitrepo. You can mirror it to your own helm chart repo.
       
    - Kernel Mode Sensor Deployment (aka Linux Sensor) - DaemonSet deployment (default) `--set container.enabled=false` that is deployed to all Kubernetes Worker nodes with special operator priviledges.
    - Container Sensor (aka UserMode Sensor) - SideCar deployment `--set container.enabled=true` that is deployed to monitor specific NAMESPACEs with stub sidecars on every pod and a Sensor pod for each Namespace.
     
  + Kubernetes Protection Agent - A Kubernetes API level agent information gathering container deployed once into a cluster for workload visibility.
     The kubernetes protection agent is a private helm.tar.gz file in the private helmchart of the Crowdstrike private API. You can mirror it to your own helm chart repo. 
     
     Setup - https://falcon.crowdstrike.com/cloud-security/registration
     
     This is a Dockerv1 API so you can provide a dockerAPIToken for authentication
     
     - https://registry.crowdstrike.com/kpagent-helm
     - https://registry.crowdstrike.com/kubernetes_protection/kpagent 
     
* CrowdStrike Operators
  + Falcon Sensor -  **An alternative to the Falcon Sensor helm charts**,  - Sensor deployment via a container for DEVSECOPS. This operator which deploys a controller into Kubernetes assists in getting the bearer tokens from the two APIs that must be done manually if used with the helm chart.
  
       https://github.com/CrowdStrike/falcon-operator
       https://github.com/CrowdStrike/falcon-operator/blob/main/deploy/falcon-operator.yaml

    - Kernel Mode Sensor Deployment (aka Linux Sensor) - DaemonSet deployment `kind: FalconNodeSensor`
    - Container Sensor (aka UserMode Sensor) - SideCar deployment `kind: FalconContainer`
    
* Falcon Sensor Download
  + The user installable - located in the Falcon Platform UI(https://falcon.crowdstrike.com) and [REST API](https://api.crowdstrike.com) of CrowdStrike Falcon. You can fetch this with the psfalconSDK or the falconpy SDK. This is NOT deployable via containers or kubernetes.
  
  + The DEVSECOP container deployment - is NOT located in the UI . You must retrieve it from the container registry associated with cloud that the customer CID environment is associated. In order to talk to the registry, you must get a bearer token from the REST API and then also from the REGISTRY itself. 
    - https://registry.crowdstrike.com/falcon-sensor
    - https://registry.crowdstrike.com/falcon-container

### Notes on alternative clouds

CrowdStrike Gov Clouds `us-gov-1` require specific urls liek the addition of `laggar.gcw.`, other clouds such as us-2 are often auto discoverable but *may require* specific domain changes. 

* Private Helm - https://registry.laggar.gcw.crowdstrike.com/kpagent-helm 
* Private CR - https://registry.laggar.gcw.crowdstrike.com/falcon-container
* REST API -  https://api.laggar.gcw.crowdstrike.com 

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


Example retrieving the list of available sensor in the container repositories.

    - https://registry.crowdstrike.com/falcon-sensor
    - https://registry.crowdstrike.com/falcon-container

![Screenshot](docs/Fetching%20Tags.png?raw=true "Screenshot") <!-- .element height="50%" width="50%" -->
