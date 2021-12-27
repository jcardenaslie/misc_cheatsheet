export MY_ZONE=us-central1-a

gsutil cp gs://cloud-training/gcpfcoreinfra/mydeploy.yaml mydeploy.yaml

sed -i -e "s/PROJECT_ID/$DEVSHELL_PROJECT_ID/" mydeploy.yaml

sed -i -e "s/ZONE/$MY_ZONE/" mydeploy.yaml

cat mydeploy.yaml

```yaml
resources:
- name: my-vm
  type: compute.v1.instance
  properties:
    zone: us-central1-a
    machineType: zones/us-central1-a/machineTypes/n1-standard-1
    metadata:
      items:
      - key: startup-script
        value: "apt-get update"
    disks:
    - deviceName: boot
      type: PERSISTENT
      boot: true
      autoDelete: true
      initializeParams:
        sourceImage: https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/debian-9-stretch-v20201216
    networkInterfaces:
    - network: https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-02-ed1cc52721fd/global/networks/default
      accessConfigs:
      - name: External NAT
        type: ONE_TO_ONE_NAT
```

Build a deployment from the template:

gcloud deployment-manager deployments create my-first-depl --config mydeploy.yaml

nano mydeploy.yaml

```yaml
resources:
- name: my-vm
  type: compute.v1.instance
  properties:
    zone: us-central1-a
    machineType: zones/us-central1-a/machineTypes/n1-standard-1
    metadata:
      items:
      - key: startup-script
        value: "apt-get update; apt-get install nginx-light -y"
    disks:
    - deviceName: boot
      type: PERSISTENT
      boot: true
      autoDelete: true
      initializeParams:
        sourceImage: https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/debian-9-stretch-v20201216
    networkInterfaces:
    - network: https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-02-ed1cc52721fd/global/networks/default
      accessConfigs:
      - name: External NAT
        type: ONE_TO_ONE_NAT
```

gcloud deployment-manager deployments update my-first-depl --config mydeploy.yaml


curl -sSO https://dl.google.com/cloudagents/install-monitoring-agent.sh
sudo bash install-monitoring-agent.sh

curl -sSO https://dl.google.com/cloudagents/install-logging-agent.sh
sudo bash install-logging-agent.sh