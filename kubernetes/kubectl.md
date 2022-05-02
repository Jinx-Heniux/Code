# Quickstart

## kubectl



### api-resources

```bash
# To see which Kubernetes resources are and aren't in a namespace
# In a namespace
kubectl api-resources --namespaced=true
# Not in a namespace
kubectl api-resources --namespaced=false
```



### apply

```bash
kubectl diff -f configs/
kubectl apply -f configs/
# Process all object configuration files in the configs directory, 
# and create or patch the live objects.

kubectl diff -R -f configs/
kubectl apply -R -f configs/
# Recursively process directories
```



### create

```bash
kubectl create deployment java-demo --image=lizhenliang/java-demo --dry-run -o yaml > java-demo-deployment.yaml
# 创建deployment

kubectl create secret docker-registry dtrcred --docker-server=https://dtr.example.com --docker-username=xxx --docker-password=yyy
# 用上面的命令生成secret资源，其中保存了下载容器镜像的用户名及密码。

kubectl create deployment nginx --image nginx
# Run an instance of the nginx container by creating a Deployment object

kubectl create -f nginx.yaml
# Create the objects defined in a configuration file
```



### delete

```bash
kubectl delete -f nginx.yaml -f redis.yaml
# Delete the objects defined in two configuration files

kubectl delete pods --all
# 删除所有Pod
```



### get

```shell
kubectl get deployment 
# 列出当前的deployment资源对象





kubectl get events -A --field-selector=reason=OwnerRefInvalidNamespace
# a warning Event with a reason of OwnerRefInvalidNamespace and 
# an involvedObject of the invalid dependent





kubectl get namespace
# list the current namespaces in a cluster






kubectl get nodes --show-labels 
# 列出节点以及节点标签






kubectl get pods -o wide 
# 列出当前的pods资源对象 -o wide 列出详细信息

kubectl get pods -n kube-system -o wide
# 查看属于名称空间kube-system下的当前运行的pods。查看系统级别的pods

kubectl get pods --show-labels 
# 列出运行的pods以及标签

# kubectl get pods -w
-w 监控状态

# https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
# using labels
# using equality-based
kubectl get pods -l environment=production,tier=frontend

# using set-based requirements
kubectl get pods -l 'environment in (production),tier in (frontend)'
# OR operator on values
kubectl get pods -l 'environment in (production, qa)'
# restricting negative matching via exists operator
kubectl get pods -l 'environment,environment notin (frontend)'


# selects all Pods for which the value of the status.phase field is Running
kubectl get pods --field-selector status.phase=Running
# 以下两个等价 equivalent
kubectl get pods
kubectl get pods --field-selector "" 

# selects all Pods for which the status.phase does not equal Running and the spec.restartPolicy field equals Always
kubectl get pods --field-selector=status.phase!=Running,spec.restartPolicy=Always








kubectl get svc
# 列出当前运行的所有服务service对象

kubectl get svc -n kube-system
# 查看属于名称空间kube-system下的当前运行的services。查看系统级别service

export SERVICE_IP=$(kubectl get svc --namespace default happy-panda-wordpress --template "{{ range (index .status.loadBalancer.ingress 0) }}{{.}}{{ end }}")
echo "WordPress URL: http://$SERVICE_IP/"
echo "WordPress Admin URL: http://$SERVICE_IP/admin"


# selects all Kubernetes Services that aren't in the default namespace
kubectl get services --all-namespaces --field-selector metadata.namespace!=default


# selects all Statefulsets and Services that are not in the default namespace
kubectl get statefulsets,services --all-namespaces --field-selector metadata.namespace!=default
```



### label

```bash
# Check that the labels are applied in the newly created pod:
kubectl get pod my-pod --show-labels

# Add a label to a pod using Kubectl:
kubectl label pod my-pod versionID=ver0.9

# Remove a label from a pod using Kubectl:
kubectl label pod my-pod versionID-

# Update a label for a pod using Kubectl:
kubectl label --overwrite pods my-pod team=ops





# If the dev pods have a “dev” label, 
# you can run the following kubectl command to get their status
kubectl get pods -l 'environment in (dev)’

# if you delete all your dev/staging environments at night 
# to save on compute costs, you can automate the following command
kubectl delete deployment,services,statefulsets -l environment in (dev,sit)





# 1. List All Linux Nodes:
kubectl get nodes -l 'kubernetes.io/os=linux'
# 2. List all nodes with instance type m3.medium:
kubectl get nodes -l 'node.kubernetes.io/instance-type=m3.medium'
# 3. List all nodes in a specific region:
kubectl get nodes -l 'topology.kubernetes.io/region=us-east-1'
# 4. List all nodes in specific regions:
kubectl get nodes -l 'topology.kubernetes.io/region in (us-east-1, us-west-1)'


kubectl get pods -l 'environment in (production), release in (canary)'
kubectl get pods -l 'environment in (production, qa)'
kubectl get pods -l 'environment notin (qa)'


# Example: Find Pods by Labels to Get their Pod Logs
# https://www.magalix.com/blog/why-it-is-important-to-use-labels-in-your-workloads-specs
ns='qa' ; label='release=canary' ; kubectl get pods -n $ns -l $label -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}{end}' | xargs -I {} kubectl -n $ns logs {}
```



### replace

```bash
kubectl replace -f nginx.yaml
# Update the objects defined in a configuration file 
# by overwriting the live configuration
```

