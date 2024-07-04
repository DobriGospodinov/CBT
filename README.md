# C-Bank Task Breakdown

#### For task description:

Check the task-description file

# Steps to fulfill the task

#### Download the data:
git clone https://github.com/DobriGospodinov/CBT.git

#### Change working directory to CBT:
cd CBT

#### Create pod:
kubectl create -f logopod.yml 

#### Ensure containers have started correctly:
kubectl get pods

#### Test container #1:
kubectl logs logopod -c the-creator

#### Test container #2:
kubectl logs logopod -c the-scholar

#### Test container #3:

Step 1: Modified logger script to create a new log file every minute:
date=$(date +"%Y-%m-%d-%H-%M-00")

Step 2: Modified annihilator script to delete logs older than 2 minutes and to run every 61 seconds, so that it runs right after the log ages

while true; do
  find /home/docata/example -name 'dummy*.log' -mmin +2 -delete
  sleep 5
done


Step 3: Observe log deletion in real time:
watch kubectl exec -it logopod -c the-annihilator -- ls /var/log/example

#### Delete config:
kubectl delete pod logopod

# --- SOURCES ---

#### Got info on how to run a script directly from yaml:
https://stackoverflow.com/questions/62782554/how-to-run-a-script-as-command-in-kubernetes-yaml-file

#### How to share data between containers:
https://stackoverflow.com/questions/53887893/can-we-use-single-volume-mount-in-pod-for-more-than-a-single-container

#### Was getting a uuid error when running the logger script, so had to add uuid-runtime installation step to "the-creator" container:
:~$ what-provides uuidgen 

uuid-runtime: /usr/bin/uuidgen

#### How to configure filebeat with command line arguments (and how to disable the default elasticsearch output and write to stdout instead):
https://www.elastic.co/guide/en/beats/libbeat/8.14/config-file-format-cli.html

#### How to configure the input parameters for filebeat:
https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-input-log.html#filebeat-input-log-options

#### Deleting files every X hours:
https://stackoverflow.com/questions/249578/how-to-delete-files-older-than-x-hours
