# Pi-Zero-OpenWrt

## 香橙派 Pi Zero (Orange Pi Zero) 支持 wifi 固件

### 配置 Samba 

配置文件修改 `map to guest = Bad User` -> `map to guest = never`

添加 Samba 用户
```bash
# 添加 用户组
addgroup samba_group
# 添加 用户
adduser -H -D -s /bin/false -G samba_group samba_user
# 添加 用户到Samba并配置密码
smbpasswd -a samba_user
```

### 修正 Aria2 启动失败

编辑 `vim /etc/init.d/aria2` 添加修复这行代码

```bash
procd_add_jail "$NAME.$section" log
procd_add_jail_mount "/usr/lib" #fix "errorCode=1 OSSL_PROVIDER_load 'legacy' failed"
procd_add_jail_mount "$ca_certificate" "$certificate" "$rpc_certificate" "$rpc_private_key"
procd_add_jail_mount_rw "$dir" "$config_dir" "$log"
procd_close_instance
```

启动 ` /etc/init.d/aria2 restart`