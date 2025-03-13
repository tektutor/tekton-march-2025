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

Running a pipeline
```
tkn pipeline start first-pipeline --showlog
```

Expected output
![image](https://github.com/user-attachments/assets/786faaff-cbe5-4ebd-893e-281ebdce0ee0)

### Note
<pre>
- Task1 
  - for each Task, one Pod will be created
  - Creates one Pod with 2 containers 
  - as Task1 has 2 Steps, for each step one container will be created
- Task2 
  - for each Task, one Pod will be created
  - Creates one Pod with 3 containers 
  - as Task2 has 3 steps, for each step one conrtainer will be created
- hence, first-pipline will create 2 Pods with total 5 containers
</pre>

## Lab - Running our second pipline where Task is defined inline within the Pipeline
```
cd ~/tekton-march-2025
git pull
cd Day9/tekton
cat second-pipeline.yml
oc apply -f second-pipeline.yml
tkn pipeline list
tkn pipeline start second-pipeline --showlog
```

Expected output
![image](https://github.com/user-attachments/assets/db4ebcd9-4f4c-445d-b43f-3319254c3d60)

## Lab - Running third pipeline with sequential and parallel tasks
```
cd ~/tekton-march-2025
git pull
cd Day9/tekton
cat third-pipeline.yml
oc apply -f third-pipeline.yml
tkn pipeline list
tkn pipeline start third-pipeline --showlog
```
Expected output
![image](https://github.com/user-attachments/assets/893fea32-9774-4130-a9aa-0bbf59b25a32)
![image](https://github.com/user-attachments/assets/797fab58-1705-42ee-bcfa-e7e676e872df)
![image](https://github.com/user-attachments/assets/88b44f21-99ad-494e-8e28-7efb5ab15367)
![image](https://github.com/user-attachments/assets/05df0a67-a71a-4852-a60e-67d5f910c265)

## Lab - Tekton Task with ConfigMap
```
cd ~/tekton-march-2025
git pull
cd Day9/tekton
cat task-with-configmap.yml
oc apply -f task-with-configmap.yml
tkn task list
tkn task start task-with-configmap --showlog
```
Expected output
![image](https://github.com/user-attachments/assets/cbd7e1d1-840a-43dc-932e-d560520fbd4c)
![image](https://github.com/user-attachments/assets/e7737c42-16eb-4e46-a711-ed665466716e)
