# Installation

_**Automatic Installation**_

```bash
source <(curl -s https://raw.githubusercontent.com/NodersUA/Scripts/main/subspace)
```

_**Manual Installation**_

```bash
# Update the repositories
apt update && apt upgrade -y
```

```bash
# Install developer packages (One command)
sudo apt-get install wget jq ocl-icd-opencl-dev \
libopencl-clang-dev libgomp1 ocl-icd-libopencl1 -y
```

```bash
# Check your cpu version
echo 'BEGIN {
    while (!/flags/) if (getline < "/proc/cpuinfo" != 1) exit 1
    if (/lm/&&/cmov/&&/cx8/&&/fpu/&&/fxsr/&&/mmx/&&/syscall/&&/sse2/) level = 1
    if (level == 1 && /cx16/&&/lahf/&&/popcnt/&&/sse4_1/&&/sse4_2/&&/ssse3/) level = 2
    if (level == 2 && /avx/&&/avx2/&&/bmi1/&&/bmi2/&&/f16c/&&/fma/&&/abm/&&/movbe/&&/xsave/) level = 3
    if (level == 3 && /avx512f/&&/avx512bw/&&/avx512cd/&&/avx512dq/&&/avx512vl/) level = 4
    if (level > 0) { print "CPU supports x86-64-v" level; exit level + 1 }
    exit 1
}' | awk -f -
```

![image](https://user-images.githubusercontent.com/79005788/228566773-da28066a-469c-4f3c-967e-ded42a19cc43.png)

If your version **CPU supports x86-64-v2**

```bash
VER=$(wget -qO- https://api.github.com/repos/subspace/subspace-cli/releases | jq '.[] | select(.prerelease==false) | select(.draft==false) | .html_url' | grep -Eo "v[0-9]+\.[0-9]+\.[0-9]+.*$" | sed 's/.$//' | head -n 1) && \
cd /usr/local/bin/ && wget https://github.com/subspace/pulsar/releases/download/${VER}/pulsar-ubuntu-x86_64-v2-${VER} -qO subspace
```

If your version **CPU supports x86-64-v3**

```bash
VER=$(wget -qO- https://api.github.com/repos/subspace/subspace-cli/releases | jq '.[] | select(.prerelease==false) | select(.draft==false) | .html_url' | grep -Eo "v[0-9]+\.[0-9]+\.[0-9]+.*$" | sed 's/.$//' | head -n 1) && \
cd /usr/local/bin/ && wget https://github.com/subspace/pulsar/releases/download/${VER}/pulsar-ubuntu-x86_64-skylake-${VER} -qO subspace
```

```bash
# Next step..
sudo chmod +x subspace && cd
echo -e "\n\nrelease  >> ${VER}." && echo -e "instaled >> $(subspace -V)\n\n"
```

Create your [polkadot.js](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Feu-0.gemini-3c.subspace.network%2Fws#/accounts) wallet or [subwallet](https://www.subwallet.app/) or use the wallet from previous episodes

```bash
subspace init
```

Follow the terminal instructions

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

```bash
# Fix journal (One command)
sudo tee <<EOF >/dev/null /etc/systemd/journald.conf
Storage=persistent
EOF
```

```bash
# Restart journal
sudo systemctl restart systemd-journald
```

```bash
# Create service file (One command)
sudo tee <<EOF >/dev/null /etc/systemd/system/subspaced.service
[Unit]
Description=Subspace Node
After=network.target
[Service]
Type=simple
User=$USER
ExecStart=$(which subspace) farm -v
StandardOutput=syslog
StandardError=syslog
Restart=always
RestartSec=3
LimitNOFILE=1024000
[Install]
WantedBy=multi-user.target
EOF
```

```bash
# Start the node
sudo systemctl daemon-reload && \
sudo systemctl enable subspaced && \
sudo systemctl restart subspaced
```

```bash
# Check the logs
sudo journalctl -fu subspaced --no-hostname -o cat
```

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Check your node in [telemetry](https://telemetry.subspace.network/#list/0x92e91e657747c41eeabed5129ff51689d2e935b9f6abfbd5dfcb2e1d0d035095)

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
