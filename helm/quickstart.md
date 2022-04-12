# Quickstart



```shell
#################### Add a Helm chart repository ####################
# https://docs.mirantis.com/msr/2.9/ops/use-helm-charts/add-helm-chart-repository.html

# To add the new MSR repository as a Helm repository:
# helm repo add <reponame> https://<msrhost>/charts/<namespace>/<reponame> --username <username> --password <password> --ca-file ca.crt
# "<reponame>" has been added to your repositories

helm repo add nexeed-charts https://rb-dtr.de.bosch.com/charts/radium_ias/nexeed-charts --username zhs2si --password 50f4ac6a-75b2-411a-a508-c12fc0301d77

# To verify that the new MSR Helm repository has been added:
helm repo list



#################### Package a Helm chart ####################
helm package .




#################### Upload a Helm chart ####################
# To push a Helm chart using the Helm CLI, 
# first install the helm push plugin from chartmuseum/helm-push. 
# https://github.com/chartmuseum/helm-push
# It is not possible to push a provenance file using the Helm CLI.

helm push ./ansible-operator-0.1.0.tgz nexeed-charts



helm dependency update
helm upgrade --install <release-name> . -n <namespace>


```

