# jenkins-restore-job
## Run restore job
Make sure all jobs are not running before run command.

* Remove jenkins_home directory
```shell script
# Access jenkins pod
kubectl exec -it <JENKINS_POD> sh

# Remove jenkins_home all files
# *** Ignore Can not delete casc_configs directory ***
rm -rf /var/jenkins_home/*
```

* Remove from backup bucket
```shell script
sed '
  s/<POD_NAME>/jenkins-pod/g;
  s/<CONTAINER_NAME>/jenkins-container/g;
  s/<BACKUP_PATH>/\/backup\/latest/g;
' patch.template.yaml > patch.rendered.yaml

# dry-run
kubectl kustomize .

# apply
kubectl apply -k .
```

* Restart jenkins
```shell script
# Restart jenkins - kubernetes will re-scheduled jenkins pod after deleted. 
kubectl delete pod <JENKINS_POD>
```
