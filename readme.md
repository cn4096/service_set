## 为什么要写这个脚本

`systemctl` 本身没有"创建服务"的功能，它只是一个**控制和管理**工具。

创建服务必须手动完成两步：

1. **写 `.service` 文件** → 放到 `/etc/systemd/system/`
2. **用 `systemctl` 加载并管理它**：
   ```bash
   systemctl daemon-reload   # 让 systemd 读取新文件
   systemctl enable xxx      # 设置开机自启
   systemctl start xxx       # 启动
   ```

`systemctl` 能做的操作：`start / stop / restart / enable / disable / status / reload` 等，针对**已存在**的服务。

所以 `service.set` 脚本做的事情是自动生成 `.service` 文件，再调用 `systemctl` 完成注册。

## 脚本安装

 下载`service.set`到系统，并设置执行权限

wget下载：

```
wget -O /usr/local/bin/service.set https://raw.githubusercontent.com/cn4096/service_set/main/service.set && chmod +x /usr/local/bin/service.set && echo "✅ 安装成功" || echo "❌ 安装失败"
```

curl下载：
```
curl -o /usr/local/bin/service.set https://raw.githubusercontent.com/cn4096/service_set/main/service.set && chmod +x /usr/local/bin/service.set && echo "✅ 安装成功" || echo "❌ 安装失败"
```


## 脚本使用

三个命令行选项，优先级高于脚本顶部的配置项：

| 选项 | 作用 |
|------|------|
| `-i` / `--install` | 临时启用安装（复制到 `INSTALL_DIR`） |
| `-I` / `--no-install` | 临时禁用安装（保留源目录） |
| `-d <path>` / `--dir <path>` | 临时指定安装目录 |

**用法示例：**

```bash
# 默认：保留源目录（INSTALL_APP=false）
sudo service.set myapp

# 临时覆盖：复制到 /usr/local/bin
sudo service.set -i myapp

# 临时覆盖：复制到自定义目录
sudo service.set -i -d /opt/apps myapp

# 查看帮助
service.set -h
```
