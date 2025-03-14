# Day 10

## Reference
You may check my medium blog about CI/CD with Tekton on Openshift
<pre>
https://medium.com/tektutor/openshift-ci-cd-with-tekton-faa88ba45656  
</pre>

## Lab - Creating a declarative CI/CD Pipeline using TekTon knative Framework in Red Hat Openshift

Java CI/CD Pipeline
<pre>
apiVersion: v1
kind: PersistentVolume
metadata:
  name: tekton-pv-jegan # Replace 'jegan' with your name
  labels:
    name: jegan # Replace 'jegan' with your name
spec:
  capacity:
    storage: 100Mi
  volumeMode: Filesystem
  accessModes:
  - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  nfs:
    server: 192.168.1.108  #Replace this IP with your remote desktop linux server IP
    path: /var/share/jegan/tekton # Replace this path with your /var/nfs/user[xy]/share5
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: tekton-pvc-jegan # Replace 'jegan' with your name
spec:
  selector:
    matchLabels:
      name: jegan # Replace 'jegan' with your name
  accessModes:
  - ReadWriteMany
  resources:
    requests:
      storage: 100Mi
---
apiVersion: tekton.dev/v1
kind: Pipeline
metadata:
  name: java-tekton-cicd-pipeline
spec:
  workspaces:
  - name: source-code
  - name: maven-repo
  tasks:
  - name: clone-git-repo
    params:
    - name: url
      value: 'https://github.com/tektutor/hello-spring-boot.git'
    - name: revision
      value: main
    taskRef:
      name: git-clone
    workspaces:
    - name: output
      workspace: source-code

  - name: compile
    params:
    - name: GOALS
      value:
      - -Dmaven.repo.local=$(workspaces.maven-settings.path) 
      - clean
      - compile
    runAfter:
    - clone-git-repo
    taskRef:
      name: maven
    workspaces:
    - name: source
      workspace: source-code
    - name: maven-settings
      workspace: maven-repo

  - name: unit-test 
    params:
    - name: GOALS
      value:
      - -Dmaven.repo.local=$(workspaces.maven-settings.path)
      - test
    runAfter:
    - compile
    taskRef:
      name: maven
    workspaces:
    - name: source
      workspace: source-code
    - name: maven-settings
      workspace: maven-repo

  - name: package 
    params:
    - name: GOALS
      value:
      - -Dmaven.repo.local=$(workspaces.maven-settings.path)
      - package 
    runAfter:
    - unit-test 
    taskRef:
      name: maven
    workspaces:
    - name: source
      workspace: source-code
    - name: maven-settings
      workspace: maven-repo  
</pre>

Java CI/CD PipelineRun
<pre>
apiVersion: tekton.dev/v1
kind: PipelineRun
metadata:
  generateName: java-tekton-cicd-pipeline-run-
spec:
  pipelineRef:
    name: java-tekton-cicd-pipeline
  workspaces:
  - name: source-code
    persistentVolumeClaim:
      claimName: tekton-pvc-jegan #Replace 'jegan' with your name
    subPath: source
  - name: maven-repo
    persistentVolumeClaim:
      claimName: tekton-pvc-jegan #Replace 'jegan' with your name
    subPath: settings 
</pre>

## Lab - Triggering Tekton Pipeline using GitHub polling
<pre>
- We need to install tekton-polling operator
- this helps triggering pipeline on local openshift setup
- In case of local openshift setup, GitHub won't be able to invoke the Openshift public route url, hence the only way to trigger pipeline is using the polling operator
</pre>

Let's install the polling operator
<pre>
oc apply -f https://github.com/bigkevmcd/tekton-polling-operator/releases/download/v0.4.0/release-v0.4.0.yaml  
</pre>

## Lab - TekTon Trigger (GitHub webhook similulation with curl)
```
cd ~/tekton-march-2025
git pull
cd Day10/tekton-trigger-github-webhook
oc apply -f first-pipeline.yml
oc apply -f tekton-trigger.yml
```

Check if the event listener pod is running
```
oc get pod --field-selector=status.phase==Running
```
Find the service name
```
oc get el tektutor-trigger-listener -o=jsonpath='{.status.configuration.generatedName}'
```

List the service now
```
oc get service $(oc get el tektutor-trigger-listener -o=jsonpath'{.status.configuration.generatedName}')
```

Load the service name into an environment variable
```
SVC_NAME=$(oc get el tektutor-trigger-listener -o=jsonpath='{.status.configuration.generatedName}')
```

Let's create a route
```
oc create route edge ${SVC_NAME}-route --service=${SVC_NAME}
```

See if the route is created
```
oc get route ${SVC_NAME}-route
```

Let's us invoke the Trigger now ( this is how github webhook will notifiy for Tekton Pipeline )
```
HOOK_URL=https://$(oc get route ${SVC_NAME}-route -o=jsonpath='{.spec.host}')
curl --insecure --location --request POST ${HOOK_URL} --header 'Content-Type: application/json' --data-raw '{"name": "run-my-app", "run-it": "yes-please}'
```

Expected output
