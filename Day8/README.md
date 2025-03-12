# Day 8

## Info - Tekton Jargons
<pre>
- Red Hat Pipeline operator installs Tekton Knative Serverless Pipeline framework into Openshift
- the operators adds many Custom Resource like
  - Task
  - TaskRun
  - Pipeline
  - PipelineRun
  - PipelineResources
  - Catalog
  - Tekton Trigger
</pre>


## Info - What is a Task?
<pre>
- is one of the  Custom Resources added by Red Hat openshift operator using CRD
- Task can be executed independently outside a Tekton Pipeline
- Task supports two scopes
  - Project scope
  - Cluster scope
- Each task contains one or more steps
- Task is the smallest independent unit that can be deployed in Tekton under Openshift/Kubernetes
- Examples
  - A Task can be used to clone source code from GitHub, BitBucket, GitLab, any version control
  - A Task can be used to build a container image, etc.,
  - A Task can be used to build application binary from source code, etc.,
</pre>

## Info - What is a Step in Tekton Task?
<pre>
- Step is a Custom Resource added by Tekton in our openshift cluster using Custom Resource Definition(CRD)
- Each Step runs in a separate Container
- Steps can' be executed independently
- Steps always runs within a Tekton Task
</pre>

## Info - Tekton TaskRun
<pre>
- is a custom Resource added by Tekton
- Task can be thought of like a Class, while TaskRun is running instance of a Task
- TaskRun helps us supply Task arguments/parameters that are required for a Task to run
- Tekton TaskRun controller will spin off a Pod for each instance of TaskRun
</pre>

## Info - Pipeline
<pre>
- is a Custom Resource added by Tekton
- it is a collection of many Tasks executed in sequence or parallel
- results in an output based on your inputs given to CI/CD pipeline
- Examples
  - a First Task could clone your source code from your GitHub Repository
  - a Second Task could compile your application from the First Task clones the source code
  - a Third Task could run Unit test cases against your compiled application binary if the Second Task succeeds
  - a Fourth Task could package the application binaries if the Third Task succeeds
  - a Fifth Task could deploy the binaries to JFrog Artifactory Server or Sonatype Nexus Server if the Fourth Task succeeds
  - a Sixth Task could run in parallel to Fifth Task to deploy the microservice(application) to Openshift if Fourth Task succeeds
</pre>

## Info - PipelineRun
<pre>
- is a Custom Resource added by Tekton
- Pipeline is the blueprint/specification of PipelineRun
- PipelineRun is the running instance of the Pipeline
- PipelineRun helps us provide arguments to Pipeline
</pre>

## Info - Tekton Trigger
<pre>
- is a component that allows Tekton Pipeline to detect events from variety of sources
- it also helps us detect code changes from GitHub, or any version control
- Tekton Triggers can invoke TaskRun and/or PipelineRun with the parameters retrieved from events
- Tekton Triggers can invoke TekTon Pipeline
- Example
  - helps in triggering a Tekton CI/CD pipeline based on code commit to GitHub repo or similar version controls
</pre>

## Info - Tekton Catalog
<pre>
- Reusable Task and Pipeline Resources from a website ( Tekton Hub - hub.tekton.dev )
</pre>
![image](https://github.com/user-attachments/assets/525e00d0-42a4-40b2-8eb7-763b0bdf6c5b)
![image](https://github.com/user-attachments/assets/a758ff48-8d90-45e2-808e-b774b01039a8)


## Info - Tekton Dashboard
<pre>
- Web based Dashboard or webconsole for Tekton
- Openshift integrated the Tekton Dashboard within Openshift webconsole
- in case of Kubernetes, Tekton provides an independent Tekton Dashboard that can be accessed outside the Kubernetes cluster
</pre>

## Info - Tekton Hub has many reusable Tasks and Pipeline code
<pre>
https://hub.tekton.dev/
</pre>
![image](https://github.com/user-attachments/assets/2fbcebb9-539d-4bfd-9777-a33626160208) 

## Info - Tekton Workspace
<pre>
- is a folder where a Tekton Task can store the output
- the output stored by a Task can be fed as an input to other Tasks in the Tekton Pipeline
- For example
  - Task 1 - Clone source code from GitHub Repo and stores it in the output workspaces ( external NFS path )
  - Task 2 - Uses the already clone source and build the source using maven
</pre>

## Lab - Cloning TekTutor Training Repository
```
cd ~
git clone https://github.com/tektutor/tekton-march-2025.git
```

## Lab - Creating and running your first Tekton Task
```
cd ~/tekton-march-2025
git pull
cd Day8/tekton
cat first-tekton-task.yml
oc apply -f first-tekton-task
oc get taks
tkn task list
tkn task start helloworld-task
tkn taskrun logs helloworld-task-run-xnn4j -f -n jegan
```

Expected output
![image](https://github.com/user-attachments/assets/b8b95cf9-e759-4765-838b-7bf533fa43a0)
![image](https://github.com/user-attachments/assets/4ebd983d-3f55-428c-8a09-0c31113ed8b5)

## Lab - Tekton Client tool usage details
```
tkn

tkn tasks
tkn task
tkn t

tkn taskruns
tkn taskrun
tkn tr

tkn pipelines
tkn pipeline
tkn p

tkn pipelineruns
tkn pipelinerun
tkn pr
```

Expected output
![image](https://github.com/user-attachments/assets/3d3670ff-535d-4e36-86a0-ab13fc6ce58e)
![image](https://github.com/user-attachments/assets/a0fb4ed0-8db9-4fd6-aab2-e0686582f2fc)
![image](https://github.com/user-attachments/assets/05499b28-4a68-46af-9ebb-a5374c319bde)
![image](https://github.com/user-attachments/assets/7cdfc74d-219a-4071-93d5-8c42c52698fe)

## Lab - Tasks with multiple steps
```
cd ~/tekton-march-2025
git pull
cd Day8/tekton
cat tasks-with-multiple-steps.yml
oc apply -f tasks-with-multiple-steps.yml
tkn task list
tkn task start task-with-multiple-steps --show-log
tkn taskrun list
```

Expected output
![image](https://github.com/user-attachments/assets/ff4308af-795f-4abc-b4b6-47cc77e8bcff)
![image](https://github.com/user-attachments/assets/573cd1bd-86e2-44ab-becf-e5aa915e4532)
![image](https://github.com/user-attachments/assets/d6397d01-4da2-4368-9f31-bc0be5461202)
![image](https://github.com/user-attachments/assets/0f81c13d-2c97-4bba-82b3-eb1ec844ac75)

## Lab - Passing parameter to a Tekton Task
```
cd ~/tekton-march-2025
git pull
cd Day8/tekton
cat task-with-params.yml
oc apply -f task-with-params.yml
tkn task list
tkn task start hello-task-with-params --showlog
```

Expected output
![image](https://github.com/user-attachments/assets/268102d1-7497-43a2-8b99-d188f0797626)
![image](https://github.com/user-attachments/assets/d57e3e00-bf13-4b91-8ae9-a21b764f56d0)

## Lab - Installing Tasks from Tekton Hub to refer that from our TaskRun
```
tkn hub install task git-clone
```
Expected output
![image](https://github.com/user-attachments/assets/a3c71d71-facb-43d6-8db6-d8d97ac02c9a)
![image](https://github.com/user-attachments/assets/12d4da30-da3f-4f2c-96b4-2ec26a22a53f)

## Lab - Clone GitHub repo using Tekton TaskRun

Make sure you are customizing the clone.yml replace 'jegan' with your name.   Also make sure the nfs server IP and the path is updated as per the folder allocated to you before proceeding.

```
cd ~/tekton-march-2025
git pull
cd Day8/tekton
cat clone.yml
oc apply -f clone.yml
```

Expected output
![image](https://github.com/user-attachments/assets/a004d436-d489-4d18-b06d-53fd2d4b7c47)
![image](https://github.com/user-attachments/assets/861874dc-533c-4ff4-acd1-2ed6fec59eb1)
![image](https://github.com/user-attachments/assets/99d536b2-1c93-47e3-aecc-ecc1a56730f7)
![image](https://github.com/user-attachments/assets/759b3930-b147-4216-a840-71ccdcf976b4)

