# Day 7

## Jenkins 
<pre>
- is an opensource CI/CD Build Server
- this helps us automate the Continuous Integration
- engineers will be integrating their code several times a day into the dev branch on the GitHub or similar version control
- Jenkins will be monitoring for code changes happening in your project code repository 
- When Jenkins detects code change, it will clone the source code, it trigger build, as part of the build automated test cases will be executed to find defects
- if the code compiles without any compilation error and if all the automated test cases passes, the build will result in success otherwise the build will fail
- the Jenkins Build report is the feedback given to the team
- Jenkins is Web server which runs 24/7, 365 days looking for code changes
- Inspired by Jenkins,many similar CI/CD servers came into the market
  - Bamboo
  - Circle CD
  - Microsoft TFS
  - Cloudbees ( Enterprise Jenkins - Cloudbees organization provides support worldwide )
</pre>

## Serverless Architecture
<pre>
- it is not that there is no server
- the server will be provisioned when there is a need
- the provisioned server will trigger build, reports the build status, and then the server will be disposed
</pre>

## Tekton 
<pre>
- is a serverless CI/CD Framework
- runs with Kubernetes/Openshift
- it is knative serverless CI/CD Framework
- knative - means Kubernetes native framework, hence only works in Kubernetes and Openshift
- this is an alternate to Jenkins or any server based CI/CD Build Servers
</pre>
