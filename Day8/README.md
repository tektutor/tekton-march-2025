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
</pre>

## Info 
