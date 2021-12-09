# smartdns

# 关闭 prefetch-domain 和 serve-expired
prefetch-domain no
serve-expired no
# 有网友说，开起这两个预加载 或 过期缓存，重新获取IP时会无视分组。如果你遇到污染，可以尝试关闭。

# 关闭 IPv6 解析 和 双栈优选
force-AAAA-SOA yes
dualstack-ip-selection no
# IPv6 由于优先级较高，可能导致不可知的问题
# 如果你需要国内的 IPv6，这里自行 # 注释掉
# 或者考虑使用 address 参数，强制某地址返回 IPv4 或者 IPv6

# 国内 DNS
bind-tcp :6153 -group cn speed-check-mode ping,tcp:80
bind :6153 -group cn speed-check-mode ping,tcp:80

server 223.6.6.6 -group cn -group bootstrap
server 114.114.114.114 -group cn -group bootstrap
server 119.29.29.29 -group cn -group bootstrap

# 国外DNS
bind-tcp :5335 -group overseas -no-speed-check
bind :5335 -group overseas -no-speed-check

#DOH
server-https https://8.8.4.4/dns-query -group overseas -exclude-default-group
server-https https://1.1.1.1/dns-query -group overseas -exclude-default-group

nameserver /cloudflare-dns.com/bootstrap
server-https https://cloudflare-dns.com/dns-query -group overseas -exclude-default-group
