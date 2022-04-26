# kubectl

## apply

```bash
kubectl diff -f configs/
kubectl apply -f configs/
# Process all object configuration files in the configs directory, 
# and create or patch the live objects.

kubectl diff -R -f configs/
kubectl apply -R -f configs/
# Recursively process directories
```



## create

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



## delete

```bash
kubectl delete -f nginx.yaml -f redis.yaml
# Delete the objects defined in two configuration files

kubectl delete pods --all
# 删除所有Pod
```



## get

```shell
kubectl get pods -o wide 
# 列出当前的pods资源对象 -o wide 列出详细信息

kubectl get pods -n kube-system -o wide
# 查看属于名称空间kube-system下的当前运行的pods。查看系统级别的pods

kubectl get pods --show-labels 
# 列出运行的pods以及标签

# kubectl get pods -w
-w 监控状态




kubectl get nodes --show-labels 
# 列出节点以及节点标签





kubectl get deployment 
# 列出当前的deployment资源对象




kubectl get svc
# 列出当前运行的所有服务service对象

kubectl get svc -n kube-system
# 查看属于名称空间kube-system下的当前运行的services。查看系统级别service

export SERVICE_IP=$(kubectl get svc --namespace default happy-panda-wordpress --template "{{ range (index .status.loadBalancer.ingress 0) }}{{.}}{{ end }}")
echo "WordPress URL: http://$SERVICE_IP/"
echo "WordPress Admin URL: http://$SERVICE_IP/admin"
```



## replace

```bash
kubectl replace -f nginx.yaml
# Update the objects defined in a configuration file 
# by overwriting the live configuration
```

