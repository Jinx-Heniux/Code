# 磁盘管理





## [10 01 Raid及mdadm命令之一 - YouTube](https://www.youtube.com/watch?v=Oe4WQA-SGoU)

```bash
#!/bin/bash
#
cat << EOF
d|D) show disk usages.
m|M) show memory usages.
s|S) show swap usages.
*) quit.
EOF
echo -n -e "\033[1;31;41mYour choice: \033[0m"
read CHOICE
# read -p "Your choice: " CHOICE

while [ $CHOICE != 'quit' ];do
  case $CHOICE in
  d|D)
    echo "Disk usage: "
    df -Ph ;;
  m|M)
    echo "Memory usage: "
    free -m | grep "Mem" ;;
  s|S)
    echo "Swap usage: "
    free -m | grep "Swap" ;;
  *)
    echo "Unknown.." ;;
  esac
  
  read -p "Again, your choice: " CHOICE
done

```









