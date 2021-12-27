```
gcloud beta compute --project=qwiklabs-gcp-03-f40b644a8090 \
  instances create lamp-1-vm \
  --zone=us-central1-a \
  --machine-type=n1-standard-2 \
  --subnet=default \
  --network-tier=PREMIUM \
  --maintenance-policy=MIGRATE \
  --tags=http-server \
  --image=debian-10-buster-v20210916 \
  --image-project=debian-cloud \
  --boot-disk-size=10GB \
  --boot-disk-type=pd-balanced \
  --boot-disk-device-name=lamp-1-vm \
  --no-shielded-secure-boot \
  --shielded-vtpm \
  --shielded-integrity-monitoring \
  --reservation-affinity=any
```

```
gcloud compute --project=qwiklabs-gcp-03-f40b644a8090 \
  firewall-rules create default-allow-http \
  --direction=INGRESS \
  --priority=1000 \
  --network=default \
  --action=ALLOW \
  --rules=tcp:80 \
  --source-ranges=0.0.0.0/0 \
  --target-tags=http-server
```

Inside VM

```
sudo apt-get update
sudo apt-get install apache2 php7.0
sudo service apache2 restart

curl -sSO https://dl.google.com/cloudagents/add-monitoring-agent-repo.sh
sudo bash add-monitoring-agent-repo.sh
sudo apt-get update
sudo apt-get install stackdriver-agent

curl -sSO https://dl.google.com/cloudagents/add-logging-agent-repo.sh
sudo bash add-logging-agent-repo.sh
sudo apt-get update
sudo apt-get install google-fluentd
```