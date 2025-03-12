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
- Reusable Task and Pipeline Resources from a website
</pre>

## Info - Tekton Dashboard
<pre>
- Web based Dashboard or webconsole for Tekton
- Openshift integrated the Tekton Dashboard within Openshift webconsole
- in case of Kubernetes, Tekton provides an independent Tekton Dashboard that can be accessed outside the Kubernetes cluster
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
