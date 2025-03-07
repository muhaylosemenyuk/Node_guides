# Update

```bash
sudo systemctl stop subspaced
```

```bash
# If your version v0.6.0 - Press `subspace wipe`
# If your version v0.6.2+ - Skip `subspace wipe`
subspace wipe
```

```bash
# Do you want to wipe farmer (delete farm)? [y/n]: y
# Do you want to wipe node? [y/n]: n
# Do you want to wipe summary? [y/n]: y
# Do you want to wipe config? [y/n]: y
```

Next step...

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

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Create your [polkadot.js](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Feu-0.gemini-3c.subspace.network%2Fws#/accounts) wallet or [subwallet](https://www.subwallet.app/) or use the wallet from previous episodes

```bash
subspace init
```

Follow the terminal instructions

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

```bash
sudo systemctl restart subspaced
```

```bash
# Check the logs
sudo journalctl -fu subspaced --no-hostname -o cat
```
