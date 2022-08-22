# tar

<pre class="language-shell"><code class="lang-shell">
# 参数前- 可省略

<strong># -c, --create
</strong><strong># Create a new archive.  
</strong># Arguments supply the names of the files to be archived.  
# Directories are archived recursively, unless the --no-recursion option is given.

# -C, --directory=DIR
# Change to DIR before performing any operations.

# -f, --file=ARCHIVE
# Use archive file or device ARCHIVE.

# -j, --bzip2
# Filter the archive through bzip2(1).

# -v, --verbose
# Verbosely list files processed.

# -x, --extract, --get
# Extract files from an archive.

# -z, --gzip, --gunzip, --ungzip
# Filter the archive through gzip(1).

tar -C /usr/local -xvf go1.18.1.linux-amd64.tar.gz
# 解压到/usr/local

tar -zxvf go1.16.6.linux-amd64.tar.gz -C /usr/local
# -z 明确说明用gzip解压

tar jcf /tmp/users.tar.bz2 inittab issue
# -j 明确说明用bzip2压缩
# -c 创建归档 即打包
# 目的地：/tmp/users.tar.bz2
# 目标文件：inittab issue

tar xf /tmp/users.tar.bz2 -C ./
# 将/tmp/users.tar.bz2文件解压到当前目录下</code></pre>

