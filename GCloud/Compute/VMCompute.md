https://cloud.google.com/compute/docs/

gcloud compute zones list | grep us-central1

gcloud config set compute/zone us-central1-b

gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image-family "debian-10" \
--subnet "default"

sudo apt-get install nginx-light -y

sudo nano /var/www/html/index.nginx-debian.html

---

gcloud compute instances create --help

gcloud compute instances list

---

gcloud compute instances create www1 \
  --image-family debian-9 \
  --image-project debian-cloud \
  --zone us-central1-a \
  --tags network-lb-tag \
  --metadata startup-script="#! /bin/bash
    sudo apt-get update
    sudo apt-get install apache2 -y
    sudo service apache2 restart
    echo '<!doctype html><html><body><h1>www1</h1></body></html>' | tee /var/www/html/index.html"

---

# Firewall
gcloud compute firewall-rules create www-firewall-network-lb \
    --target-tags network-lb-tag --allow tcp:80

---

### Static Ip

gcloud compute addresses create network-lb-ip-1 \
 --region us-central1

### Health check
gcloud compute http-health-checks create basic-check

### Target Pool


gcloud compute target-pools create www-pool \
    --region us-central1 --http-health-check basic-check

### Add instances to pool

gcloud compute target-pools add-instances www-pool \
    --instances www1,www2,www3


### Add forwarding rules

gcloud compute forwarding-rules create www-rule \
    --region us-central1 \
    --ports 80 \
    --address network-lb-ip-1 \
    --target-pool www-pool

### Describe forwarding rule

gcloud compute forwarding-rules describe www-rule --region us-central1

### Send traffic to forwarding rool

while true; do curl -m1 IP_ADDRESS; done

---

First, create the load balancer template:

gcloud compute instance-templates create lb-backend-template \
   --region=us-central1 \
   --network=default \
   --subnet=default \
   --tags=allow-health-check \
   --image-family=debian-9 \
   --image-project=debian-cloud \
   --metadata=startup-script='#! /bin/bash
     apt-get update
     apt-get install apache2 -y
     a2ensite default-ssl
     a2enmod ssl
     vm_hostname="$(curl -H "Metadata-Flavor:Google" \
     http://169.254.169.254/computeMetadata/v1/instance/name)"
     echo "Page served from: $vm_hostname" | \
     tee /var/www/html/index.html
     systemctl restart apache2'

Create a managed instance group based on the template:

gcloud compute instance-groups managed create lb-backend-group \
   --template=lb-backend-template --size=2 --zone=us-central1-a

Create the fw-allow-health-check firewall rule. This is an ingress rule that allows traffic from the Google Cloud health checking systems (130.211.0.0/22 and 35.191.0.0/16). This lab uses the target tag allow-health-check to identify the VMs.

gcloud compute firewall-rules create fw-allow-health-check \
    --network=default \
    --action=allow \
    --direction=ingress \
    --source-ranges=130.211.0.0/22,35.191.0.0/16 \
    --target-tags=allow-health-check \
    --rules=tcp:80

Now that the instances are up and running, set up a global static external IP address that your customers use to reach your load balancer.

gcloud compute addresses create lb-ipv4-1 \
    --ip-version=IPV4 \
    --global
Note the IPv4 address that was reserved:

gcloud compute addresses describe lb-ipv4-1 \
    --format="get(address)" \
    --global

Create a healthcheck for the load balancer:

gcloud compute health-checks create http http-basic-check \
    --port 80

Create a backend service:

gcloud compute backend-services create web-backend-service \
    --protocol=HTTP \
    --port-name=http \
    --health-checks=http-basic-check \
    --global

Add your instance group as the backend to the backend service:

gcloud compute backend-services add-backend web-backend-service \
    --instance-group=lb-backend-group \
    --instance-group-zone=us-central1-a \
    --global

Create a URL map to route the incoming requests to the default backend service:

gcloud compute url-maps create web-map-http \
    --default-service web-backend-service

Create a target HTTP proxy to route requests to your URL map:

gcloud compute target-http-proxies create http-lb-proxy \
    --url-map web-map-http

Create a global forwarding rule to route incoming requests to the proxy:

gcloud compute forwarding-rules create http-content-rule \
    --address=lb-ipv4-1\
    --global \
    --target-http-proxy=http-lb-proxy \
    --ports=80

---

```sh
gcloud beta compute --project=qwiklabs-gcp-01-ab2a429bbff9 instances create mynet-eu-vm --zone=europe-central2-c --machine-type=n1-standard-1 --subnet=mynetwork --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=187668853582-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --image=debian-10-buster-v20210316 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-balanced --boot-disk-device-name=mynet-eu-vm --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --reservation-affinity=any
```

```sh
gcloud compute instances create privatenet-us-vm --zone=us-central1-c --machine-type=f1-micro --subnet=privatesubnet-us --image-family=debian-10 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=privatenet-us-vm
```

```sh
gcloud compute instances list --sort-by=ZONE
```

---

 cat << EOF > startup.sh
    #! /bin/bash
    apt-get update
    apt-get install -y nginx
    service nginx start
    sed -i -- 's/nginx/Google Cloud Platform -'"\$HOSTNAME"'/' 
    /var/www/html/index.nginx-debian.html
    EOF

gcloud compute instance-templates create nginx-template \
--metadata-from-file startup-script=startup.sh

gcloud compute target-pools create nginx-pool

gcloud compute instance-groups managed create nginx-group \
--base-instance-name nginx \
--size 2 \
--template nginx-template \
--target-pool nginx-pool
