# Packer-TP1
## Réponses du TP 1

Installation de git sur la VM d'origine:
```bash
[root@tp-1 ~]# dnf install git
```
* Ajout des informations de configuration de git + génération clé SSH
```bash
[root@tp-1 ~]# git config --global user.name "<USERNAME>"
[root@tp-1 ~]# git config --global user.email "<EMAIL>"
[root@tp-1 ~]# ssh-keygen -t ed25119
```
* Ajout de la clé sur git
* Clone du dépôt
```bash
[root@tp-1 ~]# git clone git@github.com:HDCarBoNe/Packer-TP1.git
[root@tp-1 ~]# cd Packer-TP1/
```

* Installation d'ansible
```bash
dnf update -y
dnf install -y epel-release
dnf install -y ansible
```
* Exécution de packer
```
[root@tp-1 ~]# packer build packer.pkr.hcl
```
* Le rendu dans la console
``` bash
[root@tp-1 Packer-TP1]# packer build packer.pkr.hcl
qemu.example: output will be in this color.

==> qemu.example: Retrieving ISO
==> qemu.example: Trying ../Rocky-8.4-x86_64-minimal.iso
==> qemu.example: Trying ../Rocky-8.4-x86_64-minimal.iso?checksum=sha256%3A0de5f12eba93e00fefc06c                                                             db0aa4389a0972a4212977362ea18bde46a1a1aa4f
==> qemu.example: ../Rocky-8.4-x86_64-minimal.iso?checksum=sha256%3A0de5f12eba93e00fefc06cdb0aa43                                                             89a0972a4212977362ea18bde46a1a1aa4f => /root/Rocky-8.4-x86_64-minimal.iso
==> qemu.example: Starting HTTP server on port 8588
==> qemu.example: Found port for communicator (SSH, WinRM, etc): 3071.
==> qemu.example: Looking for available port between 5900 and 6000 on 127.0.0.1
==> qemu.example: Starting VM, booting from CD-ROM
    qemu.example: The VM will be run headless, without a GUI. If you want to
    qemu.example: view the screen of the VM, connect via VNC without a password to
    qemu.example: vnc://127.0.0.1:5991
==> qemu.example: Waiting 10s for boot...
==> qemu.example: Connecting to VM via VNC (127.0.0.1:5991)
==> qemu.example: Typing the boot command over VNC...
    qemu.example: Not using a NetBridge -- skipping StepWaitGuestAddress
==> qemu.example: Using SSH communicator to connect: 127.0.0.1
==> qemu.example: Waiting for SSH to become available...
```

* On se conencte ensuite au vnc en utilisant moba xterm
  - Création d'une nouvel connection vnc
  - Adress Remote: 127.0.0.1
  - Port: 5991
  - Ensuite dans network >- SSH Gateway (Jump host)
    - Adresse celle de la VM d'origine 192.168.100.131 dans mon cas
    - Username: root
    - Port: 22
* Le script Ansible permet:
  - Installation des paquets Git
  - Installation de make 
  - Installation de golang 
  - Clone du projet Golang-MyIP
  - Compilation et installation de Golang-MyIP
  - Création du service golangip
  - Activation du service au démarrage
  - Désactivation de SELinux
  - Ajout des clés SSH

* Executer le build:
```
/usr/libexec/qemu-system-x86_64 -name Rocky-GolangMyIP-packer \
                    -netdev user,id=user.0,hostfwd=tcp::4141-:22 \
                    -device virtio-net,netdev=user.0 \
                    -drive file=build-rocky-8/tdhtest,if=virtio,cache=writeback,discard=ignore,format=qcow2 \
                    -machine type=pc,accel=kvm \
                    -smp cpus=2,sockets=2 \
                    -m 4096M \
```
