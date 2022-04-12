# cat



```shell
# 将内容写入文件
cat << HELM > my-app/templates/cm.yaml
apiVersion: v1
data:
  nginx.conf: |
    events {
      worker_connections  1024;
    }
    http {
      server {
        listen 80;
        location / {
          return 200 "===============================\n\n   This is your helm deploy!   \n\n===============================\n";
        }
      }
    }
kind: ConfigMap
metadata:
  name: nginx-config
HELM
```

