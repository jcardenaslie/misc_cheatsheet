gcloud dns record-sets export $FILENAME --zone $YOUR_ZONE --project project-a

gcloud dns record-sets export dns_almacenguru_cl.txt --zone almacen-guru --project almacen-guru

gcloud dns managed-zones create almacenguru-com --dns-name=almacenguru.com. --description "DNS almacenguru.com" --project almacen-guru-ti
gcloud dns managed-zones create almacenguru-cl --dns-name=almacenguru.cl. --description "DNS almacenguru.cl" --project almacen-guru-ti

gcloud dns record-sets import dns.txt --zone almacenguru-com --project almacen-guru-ti --delete-all-existing
gcloud dns record-sets import dns_almacenguru_cl.txt --zone almacenguru-cl --project almacen-guru-ti --delete-all-existing