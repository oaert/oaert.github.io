# 安装升级脚本
```shell
apt install wget
wget https://raw.githubusercontent.com/pimlie/ubuntu-mainline-kernel.sh/master/ubuntu-mainline-kernel.sh
chmod +x ubuntu-mainline-kernel.sh
sudo mv ubuntu-mainline-kernel.sh /usr/local/bin/
```

# 安装指定内核
```shell
sudo ubuntu-mainline-kernel.sh -i v6.1.0
# 安装指定版本：sudo ubuntu-mainline-kernel.sh -i 内核版本号
```

# 检查内核版本
``` shell
reboot

uname -a

```