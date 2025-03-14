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
    server: 192.168.1.231  #Replace this IP with your remote desktop linux server IP
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

Running the java ci/cd pipeline
```
cd ~/tekton-march-2025
git pull
cd Day10
oc apply -f java-cicd-pipeline.yml
oc create -f java-cicd-pipeline-run.yml
```

Expected output
![image](https://github.com/user-attachments/assets/e5173b01-663d-430c-8997-5b2904923e63)
![image](https://github.com/user-attachments/assets/4e45e004-a3e4-49f6-aaca-943f60222af8)
![image](https://github.com/user-attachments/assets/4abdb437-be7f-4078-923f-cae3ae6ce9bc)
![image](https://github.com/user-attachments/assets/1e27109a-2684-4912-b9dd-805a70d39cee)
![image](https://github.com/user-attachments/assets/09d43af2-9a53-44a6-a111-b84fc30a5f4b)
![image](https://github.com/user-attachments/assets/a85c129e-c6ee-4b0a-9517-5a8e56c9a081)


## Info - Triggering Tekton Pipeline based on code commit
<pre>
- So far, we have been running the Tekton Pipeline manually
- It is possible to configure the Pipeline to be invoked whenever there is a code commit
- Triggering pipeline can be done in 2 ways
  - Option 1 (Push) - GitHub Webhooks
    - Whenever some engineer pushes code changes to the GitHub Repository, GitHub can notify the TekTon pipline
    - preferred and recommended option
    - this only works if your Openshift webconsole is accessible from Internet
  - Option 2 (Pull) - TekTon Pipline polling
    - In case, the Openshift webconsole is not accessible from Internet for GitHub we can go for this option
    - Our lab setup, Openshift webconsole is not accessible from Internet, hence GitHub will not be able to notify our Tekton Pipeline when there is code commit
    - Hence, we will be using this option to find code changes
    - We need to install Tekton Trigger Polling Plugin to achieve this
</pre>  
Installing TekTon Trigger Polling Plugin
![image](https://github.com/user-attachments/assets/3b6c9b7b-b143-410f-b6e8-9e4e7fe4c6df)

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

```
cd ~/tekton-march-2025
git pull
cd Day10/tekton-trigger-github-polling
tkn pipeline list
cat github-trigger.yml
oc apply -f github-trigger.yml
```

Expected output
![image](https://github.com/user-attachments/assets/1f5447ee-822d-45b5-be13-6be6b6ebcf6c)
![image](https://github.com/user-attachments/assets/2eec3e41-bb6a-4e04-b8b5-5a7e7b6fe3fd)
![image](https://github.com/user-attachments/assets/2e6f1ce0-93aa-41f7-a6a3-0b8840a7d587)
![image](https://github.com/user-attachments/assets/d63ed7b2-80e5-46fe-a157-5ac54c6f3222)
![image](https://github.com/user-attachments/assets/78ea59ab-089a-4a19-b232-b1f3e55a6152)
![image](https://github.com/user-attachments/assets/3dddde07-b770-42f5-b004-50f5b06b185a)
![image](https://github.com/user-attachments/assets/f4c65c51-72ef-4db6-a13c-1c5e1a596d7e)
![image](https://github.com/user-attachments/assets/eb991550-2168-430d-b599-fe0618da6b4e)
![image](https://github.com/user-attachments/assets/2729ab6f-d34e-44d8-83d1-1ecf1df7f18f)


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
