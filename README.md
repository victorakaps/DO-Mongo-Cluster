## DO-Mongo-Cluster

### MongoDB Cluster Kubernetes Manifests
All the Kubernetes Mongodb YAML manifests used in this guide are hosted on Github. Clone the repository for reference and implementation.

```git clone https://github.com/victorakaps/DO-Mongo-Cluster.git```

![manifest](https://imgur.com/QkkYOIj.png)

Take a moment and go through all the manifests while to understand their role and working.

### Create MongoDB Secrets
substitute *username* and *password* field in mongodb-manifests/secrets.yaml file
with one's you want 
_Note: make sure secrets are base64 encoded._

with that done, lets start our kubernetes cluster creation and deployment
### Step 1: Create a Kubernetes cluster
- Head to [DigitalOcean](https://www.digitalocean.com/) and click on the **Kubernetes** button in **Create** dropdown button on the top right.
- Choose and fill in the following fields (side note: you can leave the fields unchanged for the challenge):
    - **Region**: Choose the region where you want to create your cluster.
    - **Name**: Choose a name for your cluster.
    - **Version**: Choose the version of Kubernetes you want to use.
    - **Node Pool**: Choose the node pool you want to use.
    side note: you can leave the fields as they are.
- Click on **Create** button.
- Wait until the cluster is created.

### Step 2: Connect to the cluster
- In the **Overview** tab, **Getting Started** section will have the required information.
- Go to second tab **Connecting to kubernetes** and configure the cluster access certifcate using doctl.
 
### Step 3: Deploy MongoDB Cluster
navigate to the manifest folder which is in the repo you just cloned and execute

```kubectl apply -f mongodb-manifests/.```

This command will deploy all manifests in the mongodb-manifests folder

With this done you have successfully deployed mongodb cluster. you can verify same
by running following command

```kubectl get all```

your output should look like this
![kubectl get all](https://imgur.com/B2cvE9n.png)

### Step 4: Connecting to MongoDB Cluster
- After the client gets created. Follow the below steps to access MongoDB.
- Exec into the client.
  
```kubectl exec deployment/mongo-client -it -- /bin/bash```

Login into the MongoDB shell

```mongo --host mongo-nodeport-svc --port 27017 -u user -p password```

Do following steps to test mongodb server that we just spinoff.  
Display list of DBs

```show dbs```

Get inside a particular DB.

```use db1```

Display a list of collections inside the ‘db1’ database.

```show collections```

Insert data into the db1 database.

```db.challenges.insert({name: "DOChallenge"})```

Display data from db1 database.

```db.challenges.find()```

### Step 5: Connecting to MongoDB from outside
Let’s try and access the database from outside the cluster. In order to do so, we created mongo-nodeport-svc to expose our mongodb server in kubernetes cluster to outside world.
To connect from outside the Kubernetes cluster, you must use the Kubernetes cluster’s worker node IP address or a load balancer address.
use the following command:

```mongo --host <ip> --port <port of nodeport svc> -u user -p password```

## Conclusion
we have learned how to create a MongoDB deployment instance, a client to connect with it, ran basic operations like entering data, and also explored connecting from outside the Kubernetes cluster.