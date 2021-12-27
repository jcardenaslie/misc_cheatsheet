ref: https://cloud.google.com/resource-manager/docs/creating-managing-projects#gcloud

# Get a project

gcloud projects describe PROJECT_ID

# Listing projects

gcloud projects list

cloud projects list --filter=test

# Creating a project

gcloud projects create PROJECT_ID

gcloud projects create PROJECT_ID --organization=ORGANIZATION_ID

gcloud projects create PROJECT_ID --folder=FOLDER_ID

# Updating a project

gcloud alpha projects update PROJECT_ID
--name=NAME
--update-labels=[KEY=VALUE, ...]

# Shutting down

gcloud projects delete PROJECT_ID

# Restoring a project

gcloud projects undelete PROJECT_ID