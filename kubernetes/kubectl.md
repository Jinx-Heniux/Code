# kubectl



## kubectl get

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

