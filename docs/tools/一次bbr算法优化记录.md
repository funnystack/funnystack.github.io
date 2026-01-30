## 云端性能测试

首先需要确认你的 VPS 访问 Google 的基础能力。

### 1. 基础连通性与延迟测试 (Ping & Curl)

在 CentOS 终端执行以下命令，观察 VPS 访问 Google 的响应时间。

```
# 测试延迟（回显时间）
ping -c 4 www.google.com

# 测试网页握手时间（更接近真实访问）
curl -o /dev/null -s -w "Time Connect: %{time_connect}\nTime TTFB: %{time_starttransfer}\nTotal Time: %{time_total}\n" https://www.google.com
```

- **评估标准**：访问 Google 的 `Total Time` 通常应在 **0.05s - 0.1s** 之间。如果超过 0.5s，说明 VPS 本身的国际出口存在拥塞。

![image-20260130092716436](https://funny-1305062402.cos.ap-beijing.myqcloud.com//normalimage-20260130092716436.png)

### 2. 命令行下载速度测试 (Speedtest-CLI)

测试 VPS 本身的公网带宽上限。

```
# 下载 speedtest 脚本
wget -O speedtest-cli https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py

chmod +x speedtest-cli

# 运行测试
./speedtest-cli
```

- **评估标准**：通常提供 1Gbps 端口。下载/上传速度应达到你所购套餐的上限（通常在 **300Mbps - 800Mbps**）。

![image-20260130092732279](https://funny-1305062402.cos.ap-beijing.myqcloud.com//normalimage-20260130092732279.png)

### 3. 查看是否开启了BBR 加速

```
# 查看当前内核是否支持并开启了 BBR
sysctl net.ipv4.tcp_congestion_control
# 如果输出不是 bbr，建议升级内核并开启
```

显示结果`net.ipv4.tcp_congestion_control = cubic`

## 代理链路速度测试 

仅仅 VPS 快是不够的，你本地设备通过代理隧道加密后的速度才是最终感受。

访问 [Fast.com](https://fast.com)，该网站主要测试与 Netflix 服务器的连接，其 CDN 节点分布与 Google 类似，非常具有参考价值。在调整之前fast显示的速度是110kbps。

## 诊断结论与方案

`cubic` 是传统的 TCP 拥塞控制算法。在丢包率较高的长距离跨境链路（中国 <-> 国外）中，Cubic 一旦检测到轻微丢包就会“断崖式”减速，导致速度锁死在 110kbps。

调优方案是**开启 BBR 算法**，**这是提升速度最有效的方法。** BBR (Bottleneck Bandwidth and Round-trip propagation time) 是 Google 开发的算法，能显著提升高延迟、高丢包环境下的吞吐量。

### 强制安装新内核

```
# 1. 跳过证书检查下载
wget --no-check-certificate https://mirrors.coreix.net/elrepo-archive-archive/kernel/el7/x86_64/RPMS/kernel-lt-5.4.225-1.el7.elrepo.x86_64.rpm

# 2. 安装内核包
yum localinstall -y kernel-lt-5.4.225-1.el7.elrepo.x86_64.rpm

# 3. 自动将最新安装的内核设为默认并更新引导
grub2-set-default 0 && grub2-mkconfig -o /boot/grub2/grub.cfg
```

执行完后，请直接输入 `reboot` 重启 VPS。

### 开启 BBR（解决拥塞控制）

重启成功并看到 `uname -r` 变成 `5.4.225` 后，请务必执行以下组合拳：

```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
```

完成上述操作后，请再次访问 [Fast.com](https://fast.com) 或 YouTube 4K：

- **合格线**：速度达到 **2Mbps - 5Mbps**。虽然看 4K 还是吃力，但 1080P 应该能跑动了。
- **理想线**：速度突破 **20Mbps**。说明 BBR 成功跨越了大西洋和太平洋的物理延迟。

![image-20260130093137720](https://funny-1305062402.cos.ap-beijing.myqcloud.com//normalimage-20260130093137720.png)