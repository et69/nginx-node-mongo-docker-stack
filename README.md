# Nginx, NodeJS and MongoDB with docker swarm

NodeJS Auth API app that connects to MongoDB that new users can register via Nginx web UI and login to get the jwt token. Three components: Frontend(nginx), Backend(node) and DB(mongo).

### Building and running your application

>Before starting the app, make sure change the **db.env.example** file to **db.env** and **.env.example** file in app/ dir to **.env**, and change the password values inside.

When you're ready, start your application by running:

`$ docker stack deploy -c compose.yaml multi-tier`

Your application will be available at http://localhost.

---

**docker stack uses compose file version 3. I am sure there is no "condition" attribute under depends_on in compose file version 3. Although "depends_on" is available in compose file version 3, I am not sure "depends_on" is ignored or it just ignore "the service is ready or not".**

1. For Frontend:
    >location directive for nginx still needs to improve.
    >Frontend(nginx) container firstly exit with "Failed" error message and produce **"nginx: [emerg] host not found in upstream "backend"** in /etc/nginx/conf.d/default.conf:23" error. It is because **backend** is still not ready.

2. For backend:
    >In backend Dockerfile, add `sleep 20` before `node app.js` in `CMD` because **condition** attribute is not supported in compose file version 3 and backend(node app) starts before db is still not ready and then it will get error.
    
    >Can be rewrite code with **retry** funcation but since I am not a developer, why bother?

3. For MongoDB:
    >db.env file store env variables for username and password, creating new mongodb admin user. Healthcheck can be done ```mongosh -u $${MONGO_INITDB_ROOT_USERNAME} -p $${MONGO_INITDB_ROOT_PASSWORD} --eval "db.stats().ok"```. Env variables need to be called by double dollar sign(\$\$). 


>`docker stack deploy` uses **compose file version 3** and there is no **"condition"** attribute under **depends_on**. So starting the applications in stack need to ordered somehow. (Maybe I still don't know)


