# 压缩及归档

## tar

```bash
tar -C /usr/local -xvf go1.18.1.linux-amd64.tar.gz
tar -zxvf go1.16.6.linux-amd64.tar.gz -C /usr/local

# -C, --directory=DIR
# Change to DIR before performing any operations.

# -f, --file=ARCHIVE
# Use archive file or device ARCHIVE.

# -z, --gzip, --gunzip, --ungzip
# Filter the archive through gzip(1).

# -v, --verbose
# Verbosely list files processed.

# -x, --extract, --get
# Extract files from an archive.
```

