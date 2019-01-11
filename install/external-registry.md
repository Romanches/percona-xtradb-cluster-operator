Use custom registry
------------------------------------------------
lets illustrate on OpenShift registry example

1. lets login on OpenShift.

```bash
# oc login
Authentication required for https://192.168.1.100:8443 (openshift)
Username: admin
Password:
Login successful.
You have one project on this server: "pxc"
Using project "pxc".
```

2. first we need to get the token for our user
```bash
# oc whoami -t 
ADO8CqCDappWR4hxjfDqwijEHei31yXAvWg61Jg210s
```

3. second we need registry IP
```bash
# kubectl get services/docker-registry -n default
NAME              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
docker-registry   ClusterIP   172.30.162.173   <none>        5000/TCP   1d
```

3. use the token for the login to the registry.
```bash
# docker login -u admin -p ADO8CqCDappWR4hxjfDqwijEHei31yXAvWg61Jg210s 172.30.162.173:5000
Login Succeeded
```

4. pull image by sha
```bash
# docker pull docker.io/perconalab/percona-xtradb-cluster-operator@sha256:8895ff4647602dcbcabbf6ea5d1be1611e9d7a9769c3bb3415c3a73aba2adda0
Trying to pull repository docker.io/perconalab/percona-xtradb-cluster-operator ...
sha256:8895ff4647602dcbcabbf6ea5d1be1611e9d7a9769c3bb3415c3a73aba2adda0: Pulling from docker.io/perconalab/percona-xtradb-cluster-operator
Digest: sha256:8895ff4647602dcbcabbf6ea5d1be1611e9d7a9769c3bb3415c3a73aba2adda0
Status: Image is up to date for docker.io/perconalab/percona-xtradb-cluster-operator@sha256:8895ff4647602dcbcabbf6ea5d1be1611e9d7a9769c3bb3415c3a73aba2adda0
```

5. push image to custom registry (into pxc project in OpenShift)
```bash
# docker tag \
    docker.io/perconalab/percona-xtradb-cluster-operator@sha256:8895ff4647602dcbcabbf6ea5d1be1611e9d7a9769c3bb3415c3a73aba2adda0 \
    172.30.162.173:5000/pxc/percona-xtradb-cluster-operator:0.2.0
refusing to create a tag with a digest reference
# docker push 172.30.162.173:5000/pxc/percona-xtradb-cluster-operator:0.2.0
```

6. check image on openshift web interface.
![OpenShift Registry](./assets/images/openshift-registry.png "OpenShift Registry"){: .align-center}
copy "Docker Repo" string

7. put copied value + tag string (like `docker-registry.default.svc:5000/pxc/percona-xtradb-cluster-operator:0.2.0) into `image:` option in `deploy/operator.yaml`  config.

8. make 4-6 steps for other images, update corresponding params in `deploy/cr.yaml` file
