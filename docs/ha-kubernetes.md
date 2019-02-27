
## Mô hình 

![ha_topo](../images/kubernetes_ha.png)

## Các thông số cấu hình  

| VM | IP address | Chức năng |
|----|------------|-----------|
| HA1 | 172.27.100.160 | LoadBalance cho các máy Master |
| HA2 | 172.27.100.161 | Backup cho HA1 |
| MASTER1 | 172.27.100.176 | Master K8S |
| MASTER2 | 172.27.100.177 | Master K8S |
| MASTER3 | 172.27.100.178 | Master K8S |
| NODE1 | 172.27.100.180 | Node |
| NODE1 | 172.27.100.181 | Node |
| NODE1 | 172.27.100.182 | Node |
| NODE1 | 172.27.100.183 | Node |

VIP: 172.27.100.175
Mô hình sử dụng HA-proxy để loadbalance các request cho các Master và Keepalived để đảm bảo không có SPOF tại node LB  

## Cài đặt 

### Keepalived 
Các lệnh thực hiện trên `HA1` và `HA2`

Để Keepalived forward các gói tin đến máy đích, các máy cái Keepalived phải được bật chế độ IP forwarding trong kernel. 

```bash
[root@ha1 ~]# cat >> /etc/sysctl.conf << EOF
net.ipv4.ip_forward = 1
EOF

[root@ha1 ~]# sysctl -p
```

#### Cài đặt: 

```bash
[root@ha1 ~]# yum install -y keepalived
```

#### Cấu hình:

Edit file `/etc/keepalived/keepalived.conf` theo cấu hình sau

(../conf.d/keepalived.conf)

**Lưu ý các tham số:**
 
- state : MASTER  với HA1 và BACKUP với HA2
- interface 
- priority : 100 với HA1 và 99 với HA2. số lớn sẽ được chọn làm master (phạm vi 0-250)  
- virtual_ipaddress

#### Restart service 
```bash
[root@ha1 ~]# systemctl restart keepalived
[root@ha1 ~]# systemctl enable keepalived
```

### HAproxy 

```bash
root@ha1# cat >> /etc/sysctl.conf << EOF
net.ipv4.ip_nonlocal_bind = 1
EOF
```

```bash
root@ha1# sysctl -p
net.ipv4.ip_forward = 1
net.ipv4.ip_nonlocal_bind = 1
```