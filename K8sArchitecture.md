What happens behind the scenes when you run a ```kubectl run web --image=nginx --replicas=3```  command

<img width="608" alt="Screen Shot 2021-01-26 at 10 11 30 PM" src="https://user-images.githubusercontent.com/5303780/105941754-bdd66b80-6023-11eb-8eef-a5bbb6d7b34b.png">


When you run the command, you tell the api server to create a resource of type "deployment", 
- so it creates an entry in the **_etcd database_** and returns the result "created" on the terminal, no container is created at this point. Only an entry in the **_etcd database_**
- The **_controller manager_** at the same time is polling the **_API server_**  to check if there are any resources in the database for it. It finds that there is a deployment resource for it.
- The controller manager then reads the spec of the deployment it has to create and checks what is required. So it goes ahead and creates a replica set entry in the **_etcd database_**.
- The **_controller manager_** again on it's next poll, reads the replica set entry and creates 3 new entries for pods with the status **"POD Pending"**
- At the same time, the **_scheduler_** also is polling the API Server to check if there are any pod entries in the **_database_** for it to schedule.
- The only job of the **_scheduler_** is to look at the available nodes and check which one of those are the most suitable to run those pods.
- Once the **_scheduler_** assigns pods to nodes, it's job is done. Then it’s the job of the **_kubelet_** on the worker nodes to check if there are any pod entries in the **_database_** for it to run.
- After it finds pods to run, it will create a **"POD Creating"** entry in the **_database_**, and contact the runtime (Docker or Containerd or anything else) to create a container.
- Once the application is started, it will update the **_etcd database_** with the entry **"POD Running"**.

