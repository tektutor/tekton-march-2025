# Day 9

## Info - Pipeline
<pre>
- Pipeline is a collection of many Tasks
- Pipeline invokes one to many in sequence one after the other Task
- Pipeline can also invokes certain independent tasks in parallel
</pre>

## Lab - Let's create our first Tekton Pipeline in Openshift
```
cd ~/tekton-march-2025
git pull
cd Day9/tekton
cat first-pipeline.yml
oc apply -f first-pipeline.yml
```

Expected output
![image](https://github.com/user-attachments/assets/ed0d7c8b-f99a-408a-8ec8-67443ac420b7)
![image](https://github.com/user-attachments/assets/5622d2c8-c472-4557-9786-4d451582daaf)
![image](https://github.com/user-attachments/assets/14c26e9c-4872-4dc2-a75f-26d394ad63b0)

Listing the task, taskruns, pipeline and pipelineruns
```
tkn task list
tkn taskrun list
tkn pipeline list
tkn pipelinerun list
```

Expected output
![image](https://github.com/user-attachments/assets/19ef6caf-c94a-4721-ba4e-84612b4e27e7)
![image](https://github.com/user-attachments/assets/c34a513c-747c-46c2-abe8-bd92a9f610e5)

