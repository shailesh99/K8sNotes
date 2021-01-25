What happens behind the scenes when you run a ```kubectl run web --image=nginx --replicas=3```  command

When you run the command, you tell the api server to create a resource of type "deployment", 
- so it creates an entry in the **_etcd database_** and returns the result "created" on the terminal, no container is created at this point. Only an entry in the **_etcd database_**

