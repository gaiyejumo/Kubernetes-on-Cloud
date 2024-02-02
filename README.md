# Kubernetes-on-Cloud
Hands-on with Kubernetes on Cloud

Steps
Download and upload the tcb-vote.zip file to Cloud Shell:
https://www.dropbox.com/s/oxsnuzydxubwyhb/tcb-vote-en.zip?dl=0
Unzip tcb-vote.zip
mkdir kube
mv tcb-vote*.zip kube
cd kube
unzip tcb-vote*.zip
Set project
gcloud config set project <PROJECT_ID>

​
Build and push the image to Container Registry
gcloud services enable cloudbuild.googleapis.com --project <PROJECT_ID>
gcloud builds submit --tag gcr.io/<PROJECT_ID>/tcb-vote-front

​
Create and Connect to GKE
Deploy the app to GKE
kubectl apply -f tcb-vote-plus-redis-v2.yaml

​
Enable autoscaling - horizontal pod autoscaling
kubectl autoscale deployment tcb-vote-front --cpu-percent=50 --min=1 --max=10
kubectl get hpa
kubectl get pods
kubectl get deployment tcb-vote-front

​
Simulate user access (increase the load)
kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- <http://tcb-vote-front>; done"

​
Watch autoscale
kubectl get hpa tcb-vote-front --watch
kubectl get deployment tcb-vote-front

​
Stop load
CTRL +C on cloud shell tab running the load inscrease
Watch scale down
kubectl get hpa tcb-vote-front --watch
kubectl get deployment tcb-vote-front

