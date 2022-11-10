# OS installation

We are going through a standard ONIE procedure.

Please refer to https://netbergtw.com/top-support/articles/onie-recovery-on-x86-enabled-netberg-aurora-switches/ if ONIE is missing.

You can [make your own ONIE image](https://github.com/opencomputeproject/onie/blob/master/contrib/debian-iso/README.md) with the preseed file or get a ready to use Ubuntu 20.04 [image](http://www.netbergtw.com/wp-content/uploads/Files/ubuntu-focal-amd64-mini-ONIE.bin) and [preseed](http://www.netbergtw.com/wp-content/uploads/Files/debian-preseed.txt):

Install OS with the `onie-nos-install` command from a remote or local source (don’t forget to put debian-preseed.txt near the image).
```bash
 onie-nos-install http://192.168.0.4:8000/ubuntu-focal-amd64-mini-ONIE.bin
```

## During installation

Select the proper network device: e.g. enp8s0: Intel Corporation I210 Gigabit Network Connection.
Agree to a weak password (onie/onie is the default user/password).

# Post-intallation steps:

## Update repos and install the latest kernel:
```bash
 apt update
 apt upgrade
 apt install --install-recommends linux-generic-hwe-20.04
```
## Install additional components:
```bash
 apt install openssh-server build-essential git i2c-tools
```
## Build optoe driver
```bash
 git clone https://github.com/opencomputeproject/oom.git
 cd oom/optoe
 echo "ccflags-y := -DLATEST_KERNEL" > Makefile
 echo "obj-m := optoe.o" >> Makefile
 make -C /lib/modules/$(uname -r)/build M=$(pwd) modules
 make -C /lib/modules/$(uname -r)/build M=$(pwd) modules_install
 depmod
```

## Model dependent steps:

See the appropriate folder for instructions for specific models.
