
## soft RoCE

### RoCE 설치 
```sh
$ sudo apt install rdma-core
$ apt install ibverbs-utils
$ sudo modprobe rdma_rxe
$ sudo rdma link add rxe0 type rxe net enp0s8
$ sudo ibv_devinfo
$ sudo apt install perftest
$ sudo ib_write_bw -d rxe0  <<= server
$ sudo ib_write_bw 192.168.0.21 -d rxe0  <<=== client
```

```sh
root@vm:~# sudo  ibv_devinfo
hca_id:	rxe0
	transport:			InfiniBand (0)
	fw_ver:				0.0.0
	node_guid:			0a00:27ff:fe67:4f90
	sys_image_guid:			0a00:27ff:fe67:4f90
	vendor_id:			0xffffff
	vendor_part_id:			0
	hw_ver:				0x0
	phys_port_cnt:			1
		port:	1
			state:			PORT_ACTIVE (4)
			max_mtu:		4096 (5)
			active_mtu:		1024 (3)
			sm_lid:			0
			port_lid:		0
			port_lmc:		0x00
			link_layer:		Ethernet

$ sudo  rdma link list
link rxe0/1 state ACTIVE physical_state LINK_UP netdev enp0s8
```

### rdma_rxe 
* rdma module 
```sh
echo  "rdma_rxe" | sudo tee -a /etc/modules
cat /etc/modules
```
* rxe-setup 
```sh
$ rdma link add rxe0 type rxe net enp0s8 NETDEV

$ sudo  vi /etc/systemd/system/rxe-setup.service
[Unit]
Description=Setup RXE (RoCE v2 over Ethernet)
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=/usr/bin/rdma link add rxe0 type rxe net enp0s8
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```
* service daemon
```sh
$ sudo chmod  644 /etc/systemd/system/rxe-setup.service 
$ sudo systemctl daemon-reload
$ sudo systemctl enable rxe-setup.service
```
