# shared_mem_demo
A demo(proof) which communicate between userspace and kernel(map kernel memory to userspace)


# output
```
root@eBPF:~/shared_mem_demo# make
make -C /lib/modules/4.15.0-20-generic/build M=/root/shared_mem_demo modules
make[1]: Entering directory '/usr/src/linux-headers-4.15.0-20-generic'
  CC [M]  /root/shared_mem_demo/kernel_module.o
/root/shared_mem_demo/kernel_module.o: warning: objtool: mmap_drv_mmap()+0x72: sibling call from callable instruction with modified stack frame
  Building modules, stage 2.
  MODPOST 1 modules
  CC      /root/shared_mem_demo/kernel_module.mod.o
  LD [M]  /root/shared_mem_demo/kernel_module.ko
make[1]: Leaving directory '/usr/src/linux-headers-4.15.0-20-generic'
root@eBPF:~/shared_mem_demo#
root@eBPF:~/shared_mem_demo#
root@eBPF:~/shared_mem_demo# insmod kernel_module.ko
root@eBPF:~/shared_mem_demo#
root@eBPF:~/shared_mem_demo# cat /proc/devices  | grep evt_map
243 evt_map
root@eBPF:~/shared_mem_demo#
root@eBPF:~/shared_mem_demo# mknod /dev/evt_map c 243 0
root@eBPF:~/shared_mem_demo#
root@eBPF:~/shared_mem_demo# gcc userspace.c -o userspace
root@eBPF:~/shared_mem_demo#
root@eBPF:~/shared_mem_demo# ./userspace
reverse shell event{
	'evt':'rvshell',
	'pid':'19256',
	'exe':'/bin/bash',
	'cmdline':'bash',
	'cwd':'/root/Felicia',
	'ppid':'19255',
	'pexe':'/usr/bin/socat'
}
root@eBPF:~/shared_mem_demo#
```