To see what your default region and zone settings are, run the following commands:

gcloud config get-value compute/zone
gcloud config get-value compute/region

gcloud compute project-info describe --project <your_project_ID>

export PROJECT_ID=<your_project_ID>

export ZONE=<your_zone>

echo $PROJECT_ID
echo $ZONE