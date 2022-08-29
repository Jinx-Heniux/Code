# Quickstart

## 安装Java

<pre class="language-bash"><code class="lang-bash"># Install Java on Ubuntu 20.04

# 通过synaptic安装Java
sudo apt install synaptic

# 通过cli安装Java
apt-cache search openjdk
sudo apt install openjdk-17-jre openjdk-17-jdk

# 设置默认版本
sudo update-alternatives --config java

# JAVA_HOME 环境变量
sudo vim /etc/environment
<strong># 添加下面的行：
</strong>JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
# 让修改在当前 shell 生效
source /etc/environment
# 验证 JAVA_HOME 环境变量被正确设置：
echo $JAVA_HOME

# 卸载 Java
sudo apt remove openjdk-11-jdk
</code></pre>



## 安装IDE

### 配置 VS Code



### 安装IntelliJ IDEA



### 安装Jetbrains Toolbox

