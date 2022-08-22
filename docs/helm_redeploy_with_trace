# Delete the DS and Pod from the HelmChart
HELM="microk8s.helm3"
KUBECTL="microk8s.kubectl"

echo "Uninstalling the helm chart.."
$HELM uninstall falcon-helm crowdstrike/falcon-sensor -n ${FALCON_DS_NAMESPACE}
echo \$> $KUBECTL kubectl get pods -n falcon-system
echo \$> $KUBECTL kubectl get ds -n falcon-system

echo Re-install using a different flagg for "falcon.trace=debug"
$HELM upgrade --install falcon-helm crowdstrike/falcon-sensor -n ${FALCON_DS_NAMESPACE} \
--set falcon.cid=${FALCON_CID_CHKSUM} --set falcon.trace=debug --set node.image.repository="${FALCON_CR_URL}" \
--set node.image.pullSecrets="${FALCON_IMAGE_SECRET}" --set node.image.tag=${FALCON_IMAGE_TAG}

echo # Run the following to get the logs of the DS pod 
echo \$> $KUBECTL get pods -n falcon-system
echo \$> $HELM logs -f falcon-helm-falcon-sensor-nct95 -n falcon-system

echo Observe the node mount not building out the C-files
echo \$> ls -la /opt/CrowdStrike/
