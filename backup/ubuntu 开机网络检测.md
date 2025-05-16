## 卡在 A start job is running for hait for Network to be Configured (1min 40s / no） 这里

![LOGO](https://img2023.cnblogs.com/blog/2908207/202304/2908207-20230414174711771-2116171721.png)

### 解决方法
```shell
vim /etc/systemd/system/network-online.target.wants/systemd-networkd-wait-online.service

#在service添加配置
TimeoutStartSec=2sec

```

如图
![LOGO](https://img.picui.cn/free/2025/05/16/6827094fb6459.png)


## 失败时候可以试下下面的方法

修改网卡配置 /etc/netplan/00-installer-config.yaml ，也有可能不是这个名字，一般这个文件夹下面只有一个yaml配置文件，用来配置网卡的信息。

在网卡上添加 optional: true 选项 然后重启系统，再看是否还卡同样的问题

![LOGO](https://img2023.cnblogs.com/blog/2908207/202308/2908207-20230804095204414-1994174192.png)

这个配置表示，该网卡的配置是可选的，而不是强制性的。

这意味着系统可以在没有这个网卡配置的情况下正常运行，或者在必要时自动选择适当的默认配置。