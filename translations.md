### 1. Creating and deployment a simple app to a Compute Engine VM Instance

**Task a: Creating a Compute Engine Virtual Machine Instance**
```
export IMAGE_FAMILY="tf-latest-cu92"
export ZONE="us-central-a"
export INSTANCE_NAME="my-new-instance"
export INSTANCE_TYPE="n1-standard-8"
gcloud compute instances create $INSTANCE_NAME \
        --zone=$ZONE \
        --image-family=$IMAGE_FAMILY \
        --image-project=deeplearning-platform-release \
        --maintenance-policy=TERMINATE \
        --accelerator="type=nvidia-tesla-v100,count=8" \
        --machine-type=$INSTANCE_TYPE \
        --boot-disk-size=120GB \
        --metadata="install-nvidia-driver=True"
    
gcloud compute instances list
gcloud compute ssh $INSTANCE_NAME
```
**Task b: Install software on the VM instance**
```
sudo apt-get update
sudo apyt-get install git

curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -


sudo apt install nodejs
```
**Task c: Configure the VM to Run Application Software**
```
## clone the training repository
git clone https://github.com/GoogleCloudPlatform/training-data-analyst

## cd into the directory
cd ~/training-data-analyst/courses/developingapps/nodejs/datastore/start

## run a simple web server
node server/app.js
```
<hr/>
####2.  Deploying an app to Gcloud App engine

```
## create an app engine instance
Gcloud app create --project=$DEVSHELL_PROJECT_ID --region uscentral

## clone the app from github into cloud shell
git clone https://github.com/GoogleCloudPlatform/python-docs-samples
cd python-docs-samples/appengine/standard_python3/hello_world
sudo apt-get update
sudo apt-get install virtualenv
virtualenv -p python3 venv
source venv/bin/activate
pip install  -r requirements.txt

python main.py

## deploy to app engine
gcloud app deploy
## launch your app on live web address.
gcloud app browse
```
<hr/>
###3. RUN and deploy a kubernetes container

``` 
export MY_ZONE=uscentral1
gcloud container services create webFrontend --num-nodes 2
kubectl version
kubectl get pods

## create a deployment
kubectl create deploy nginx --image=nginx:1.17.10

##expose the pod to the internet
kubectl expose deployment nginx --port 80 --type LoadBalancer
kubectl get services

##Confirm that your external IP address has not changed:
 kubectl get services

## to scale up your deployment
kubectl scale deployment nginx --replicas 3
```
