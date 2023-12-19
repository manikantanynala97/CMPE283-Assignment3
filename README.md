# CMPE 283 Assignment 3

#### **Perfromed the step to generate the rsa key-pair**

![assignment2-1](https://github.com/manikantanynala97/CMPE283-Assignment3/assets/90610801/d7a112a1-90de-4c52-a5ea-219bb4b09e4b)


## Question 1:



Successfully generated the kernel and installed crucial kernel modules within the VM. Conducted thorough research on CPUID instructions and CPU leaf nodes, pinpointing specific modifications needed for assignment completion. Implemented the necessary changes in the cpuid.c and vmx.c files to align with project requirements. Updated the README.md with comprehensive details and crafted documentation for future reference.

Achieved the creation of the kernel and proceeded to install vital kernel modules within the VM. Developed a solid understanding of the assignment's code, followed by the installation of the nested VM and comprehensive testing through CPUID.

  The output is in [this repository](https://github.com/manikantanynala97/linux.git)
  <br />

## Question 2: (Steps used to complete the assignment)

Run this code in the VM

```
git clone https://github.com/manikantanynala97/linux.git
```

Get the correct Linux version

```
ls /boot/
```

Copy the config file to root directory

```
cp /boot/config-5.11.0-1029-gcp .config
```

```
sudo apt install make
```

```
make oldconfig
```

Install all the required libraries

```
sudo apt install gcc
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install bison
sudo apt-get install qemu-kvm qemu virt-manager virt-viewer libvirt-daemon
```

Make the modules parallely using 8 cores in our machine
Update the cpuid.c and vmx.c files and make the modules

```
cd arch
cd x86
cd kvm
vi cpuid.c
vi vmx/vmx.c
cd ..
make modules
sudo make -j 8 modules
sudo make install
```

Create a user-data file and save the nested VM login config

```
#cloud-config
chpasswd: { expire: False }
ssh_pwauth: True
```

Download and Install the nested VM

```
wget https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64.img
sudo virt-customize -a bionic-server-cloudimg-amd64.img --root-password password:newpass
sudo virt-customize -a bionic-server-cloudimg-amd64.img --uninstall cloud-init
cloud-localds user-data.img user-data
sudo qemu-system-x86_64 -enable-kvm -hda bionic-server-cloudimg-amd64.img -drive "file=user-data.img,format=raw" -m 512 -curses -nographic
```

- username: root
- password: newpass

  Inside the Nested VM Install cpuid to test

  ```
  sudo apt-get update
  sudo apt-get install cpuid
  ```

  Test using CPUID

  ```
  cpuid -l 0x4FFFFFFD
  cpuid -l 0x4FFFFFFC
  ```

## Question 3:

Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there
more exits performed during certain VM operations? Approximately how many exits does a full VM
boot entail?

The number of exits increases at a stable rates as these exits are called fairly regularly

## Question 4:

Of the exit types defined in the SDM, which are the most frequent? Least ?

Most frequent exit is
Exit #30 - IO_INSTRUCTION

Least frequest exit is
Exit #0 - EXCEPTION_NMI

![assignment3-1](https://github.com/manikantanynala97/CMPE283-Assignment3/assets/90610801/2397f0be-3c8e-4de4-b034-556ca9b9fb3c)

![assignment3-2](https://github.com/manikantanynala97/CMPE283-Assignment3/assets/90610801/83eaa418-22a4-4dad-b044-ffcab3170d25)


