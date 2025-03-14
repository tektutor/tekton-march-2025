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

## Info - Custom Resource Definitions (CRD)
<pre>
- Using the Kubernetes CRD, Tekton Team has added many custom resources
</pre>  
![image](https://github.com/user-attachments/assets/a24f265d-22d2-4037-bd0b-0873ada4e06a)


## Tekton 
<pre>
- is a serverless CI/CD Framework
- runs within Kubernetes/Openshift
- it is knative serverless CI/CD Framework
- knative - means Kubernetes native framework, hence only works in Kubernetes and Openshift
- this is an alternate to Jenkins or any server based CI/CD Build Servers
- Tekton team have developed the knative serverless CI/CD Framework as an Openshift/Kubernetes Operator
- Tekton Operators comes with
  - many Tekton custom resources
  - many Tekton custom controllers
</pre>

## Demo - Installing Red Hat Openshift Pipelines Operator to enable Tekton in Openshift
![image](https://github.com/user-attachments/assets/9bf5c19e-649f-49f2-8a7f-e1fc3ecdb0b6)
Select "Red Hat Openshift Pipelines"
![image](https://github.com/user-attachments/assets/1a783b67-cb05-41be-bf29-395c72d705fc)
Click "Install"
![image](https://github.com/user-attachments/assets/09f8c289-36bc-4722-a3b0-5f8dd16f5448)
![image](https://github.com/user-attachments/assets/d6287322-adab-4ca8-82cf-cd7c76fcebe4)
Click "Install"
![image](https://github.com/user-attachments/assets/46609edc-6aea-4365-92a7-749619900dd7)
![image](https://github.com/user-attachments/assets/87a8b9c9-46a5-43ac-9099-b418b594acf2)

Once the "Red Hat Openshift Pipeline" Operator is installed, it will add the Pipeline Dashboard within Openshift webconsole.

