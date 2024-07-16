你可以使用 `watch` 命令结合 `top` 或 `htop` 监控 CPU 使用情况，并使用 `nvidia-smi` 监控 GPU 温度。以下是具体操作步骤：

1. 安装必要工具

确保你已经安装了 `lm-sensors` 和 NVIDIA 驱动程序及工具：

```sh
sudo apt-get install lm-sensors
sudo sensors-detect
sudo apt-get install nvidia-driver-555-open  # 或者安装你需要的版本
sudo apt-get install nvidia-cuda-toolkit
```

2. 配置 `lm-sensors`

运行 `sensors-detect` 来检测系统传感器并回答所有问题：

```sh
sudo sensors-detect
```

按照提示回答所有问题（通常都选择 `yes`）。

3. 使用 `watch` 查看 CPU 使用情况

你可以使用 `top` 或 `htop` 来实时监控 CPU 使用情况。使用 `watch` 命令定时刷新 `top` 或 `htop` 的输出。

使用 `top`：

```sh
watch -n 1 top -b -n 1 | head -n 20
```

以上命令每秒刷新一次 `top` 输出并显示前 20 行。由于 `top` 本身是一个交互式工具，这里使用 `-b` 选项以批处理模式运行，并使用 `-n 1` 来仅显示一次输出。

使用 `htop`：

`htop` 更加直观，但不太适合与 `watch` 结合使用。你可以直接运行 `htop` 进行实时监控：

```sh
htop
```

4. 使用 `watch` 查看 GPU 温度

使用 `nvidia-smi` 查看 GPU 温度，并用 `watch` 命令定时刷新输出：

```sh
watch -n 1 nvidia-smi
```

这个命令每秒刷新一次 `nvidia-smi` 的输出，显示 GPU 使用情况和温度。

5. 使用 `watch` 查看 CPU 温度

使用 `sensors` 命令查看 CPU 温度，并用 `watch` 命令定时刷新输出：

```sh
watch -n 1 sensors
```

这个命令每秒刷新一次 `sensors` 的输出，显示 CPU 温度和其他传感器信息。

综合监控

如果你想同时监控 CPU 使用情况和 GPU 温度，可以在一个终端窗口中运行 `watch` 命令监控 GPU 温度，在另一个终端窗口中运行 `htop` 或 `top` 监控 CPU 使用情况。

以下是一个示例脚本，用于在一个终端窗口中同时监控 CPU 和 GPU 状态：

```sh
#!/bin/bash
# 打开两个终端窗口
gnome-terminal -- bash -c "watch -n 1 sensors; exec bash"
gnome-terminal -- bash -c "watch -n 1 nvidia-smi; exec bash"
gnome-terminal -- bash -c "htop; exec bash"
```

这个脚本会打开三个终端窗口，分别监控 CPU 温度、GPU 温度和 CPU 使用情况。

运行脚本

将上述脚本保存为 `monitor.sh`，然后运行：

```sh
chmod +x monitor.sh
./monitor.sh
```

这样，你可以同时在不同终端窗口中实时监控 CPU 和 GPU 的状态。
