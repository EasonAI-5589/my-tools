# Clash Verge 配置备份

Clash Verge 关键配置文件备份。

## 文件说明

| 文件 | 说明 | 原始路径 |
|------|------|----------|
| `rules.yaml` | 直连规则配置 | `~/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/profiles/rm8rjUmDn772.yaml` |
| `verge.yaml` | Verge 主配置 | `~/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/verge.yaml` |

## 恢复方法

```bash
# 恢复规则配置
cp rules.yaml ~/Library/Application\ Support/io.github.clash-verge-rev.clash-verge-rev/profiles/rm8rjUmDn772.yaml

# 恢复主配置
cp verge.yaml ~/Library/Application\ Support/io.github.clash-verge-rev.clash-verge-rev/verge.yaml

# 重启 Clash Verge
pkill -f "Clash Verge" && sleep 2 && open -a "Clash Verge"
```

## 当前规则概览

```yaml
# Tailscale - 远程连接家里主机
- IP-CIDR,100.64.0.0/10,DIRECT

# OpenVPN - 亦庄显卡集群
- IP-CIDR,10.51.0.0/16,DIRECT
- DOMAIN-SUFFIX,x-humanoid-cloud.com,DIRECT

# 飞连 - 字节显卡集群
- DOMAIN-SUFFIX,bytedance.net,DIRECT
- DOMAIN-SUFFIX,byted.org,DIRECT

# 奇安信零信任 - 上庄显卡集群
- DOMAIN-SUFFIX,qianxin.com,DIRECT
- DOMAIN-KEYWORD,qianxin,DIRECT
```

## 更新备份

修改 Clash 配置后，运行以下命令更新备份：

```bash
cp ~/Library/Application\ Support/io.github.clash-verge-rev.clash-verge-rev/profiles/rm8rjUmDn772.yaml ~/my-tools/proxy/clash-config/rules.yaml
cp ~/Library/Application\ Support/io.github.clash-verge-rev.clash-verge-rev/verge.yaml ~/my-tools/proxy/clash-config/verge.yaml
cd ~/my-tools && git add . && git commit -m "更新 Clash 配置备份" && git push
```
