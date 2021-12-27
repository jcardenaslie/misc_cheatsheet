# 1

Initialize your App Engine app with your project and choose its region:
gcloud app create --project=$DEVSHELL_PROJECT_ID

Clone the source code repository for a sample application in the hello_world directory:
git clone https://github.com/GoogleCloudPlatform/python-docs-samples

Navigate to the source directory:
cd python-docs-samples/appengine/standard_python3/hello_world

# 2
sudo apt-get update

sudo apt-get install virtualenv

virtualenv -p python3 venv

source venv/bin/activate

pip install  -r requirements.txt

python main.py

cd ~/python-docs-samples/appengine/standard_python3/hello_world

gcloud app deploy