---
theme: gaia
_class: lead
paginate: true
backgroundColor: "#fff"
---
是的，您可以修改 K3s 的安装路径，将其安装到自定义目录（如 `/mnt/disc1/beng003/k3s`）。在 K3s 的安装过程中，有一些环境变量和选项可以帮助您实现这一点。

下面是如何在安装 K3s 时指定自定义安装路径的步骤：

### 1. 设置 K3S_HOME 环境变量

K3s 支持通过设置 `K3S_HOME` 环境变量来改变 K3s 的安装和数据存储路径。您可以将该路径设置为 `/mnt/disc1/beng003/k3s`。

#### 1.1 创建自定义路径

首先，确保目标路径 `/mnt/disc1/beng003/k3s` 存在，如果不存在，创建它：

```bash
sudo mkdir -p /mnt/disc1/beng003/k3s
```

#### 1.2 设置环境变量

然后，通过设置 `K3S_HOME` 环境变量，将 K3s 的工作目录指向 `/mnt/disc1/beng003/k3s`：

```bash
export K3S_HOME=/mnt/disc1/beng003/k3s
```

为了使该环境变量在每次登录时生效，您可以将上述命令添加到您的 shell 配置文件中（如 `~/.bashrc` 或 `~/.zshrc`）：

```bash
echo "export K3S_HOME=/mnt/disc1/beng003/k3s" >> ~/.bashrc
source ~/.bashrc
```

### 2. 安装 K3s 到自定义路径

安装 K3s 时，您可以通过指定 `K3S_HOME` 环境变量来让 K3s 使用该路径。运行安装脚本时，`K3S_HOME` 将指向您自定义的路径：

```bash
curl -sfL https://get.k3s.io | K3S_HOME=/mnt/disc1/beng003/k3s sh -
```

### 3. 配置 K3s 使用自定义路径

安装后，您可以验证安装路径和配置：

- K3s 的二进制文件和配置文件将存储在 `/mnt/disc1/beng003/k3s` 路径下。
- 您的 `kubectl` 配置文件通常位于 `K3S_HOME` 下的 `~/.kube/config` 文件中，可以通过以下命令修改它的路径：

```bash
sudo cp /mnt/disc1/beng003/k3s/etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $(id -u):$(id -g) ~/.kube/config
```

### 4. 启动 K3s 并检查路径

安装完毕后，K3s 会使用您指定的路径。您可以启动 K3s 并检查服务状态：

```bash
sudo systemctl start k3s
sudo systemctl status k3s
```

如果 K3s 已成功安装并运行，您应该可以看到其使用的路径信息。

### 5. 其他自定义路径设置

除了 `K3S_HOME` 外，您还可以通过其他环境变量来指定特定路径，例如：

- **`K3S_DATA_DIR`**：指定 K3s 数据存储目录。
- **`K3S_LOG_DIR`**：指定日志目录。

例如，如果您还想为 K3s 设置数据和日志目录，可以通过以下命令来设置：

```bash
export K3S_DATA_DIR=/mnt/disc1/beng003/k3s/data
export K3S_LOG_DIR=/mnt/disc1/beng003/k3s/logs
```

然后重新安装或启动 K3s 以应用这些自定义路径。

### 总结

通过设置 `K3S_HOME` 环境变量，您可以轻松地将 K3s 的安装和数据存储路径修改为 `/mnt/disc1/beng003/k3s`。同时，您还可以设置其他环境变量来自定义 K3s 的数据目录和日志目录。这样，您就可以根据需要自由地控制 K3s 的安装路径。



