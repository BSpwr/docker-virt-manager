[![](https://github.com/m-bers/docker-virt-manager/workflows/docker%20build/badge.svg)](https://github.com/m-bers/docker-virt-manager/actions/workflows/deploy.yml)[![](https://img.shields.io/docker/pulls/mber5/virt-manager)](https://hub.docker.com/r/mber5/virt-manager)
# Docker virt-manager
## broadway webui for libvirt/kvm
![Docker virt-manager](docker-virt-manager.png)

### What is it? 
virt-manager: https://virt-manager.org/  
broadway: https://developer.gnome.org/gtk3/stable/gtk-broadway.html


### Features:
Uses GTK3 Broadway (HTML5) backend--no vnc, xrdp, etc needed!

New: Password/SSH passphrase support via ttyd (thanks to [@obazda20](https://github.com/obazda20/docker-virt-manager) for the idea!)
Just click the terminal icon at the bottom left to get to the password prompt after adding an ssh connection. 

<img width="114" alt="Screen Shot 2021-10-25 at 12 01 02 AM" src="https://user-images.githubusercontent.com/4750774/138649110-73c097cc-b054-424c-8fa0-d0c23540b499.png">


### Requirements:
git, docker, docker-compose, at least one libvirt/kvm host

### Usage

#### docker-compose

If docker and libvirt are on the same host
```yaml
services: 
  virt-manager:
    image: mber5/virt-manager:latest
    restart: always
    ports:
      - 8185:80
    environment:
      HOSTS: "['qemu:///system']"
    # If on an Ubuntu host (or any host with the libvirt AppArmor policy,
    # you will need to use an ssh connection to localhost
    # or use qemu:///system and uncomment the below line

    # privileged: true

    volumes:
      - "/var/run/libvirt/libvirt-sock:/var/run/libvirt/libvirt-sock"
      - "/var/lib/libvirt/images:/var/lib/libvirt/images"
    devices:
      - "/dev/kvm:/dev/kvm"
```
If docker and libvirt are on different hosts
```yaml
services: 
  virt-manager:
    image: mber5/virt-manager:latest
    restart: always
    ports:
      - 8185:80
    environment:
      # Substitute comma separated qemu connect strings, e.g.: 
      # HOSTS: "['qemu+ssh://user@host1/system', 'qemu+ssh://user@host2/system']"
      HOSTS: "[]"
    # volumes:
      # If not using password auth, substitute location of ssh private key, e.g.:
      # - /home/user/.ssh/id_rsa:/root/.ssh/id_rsa:ro
```
#### Building from Dockerfile
```bash
    git clone https://github.com/m-bers/docker-virt-manager.git
    cd docker-virt-manager
    docker build -t docker-virt-manager . && docker-compose up -d
```
Go to http://localhost:8185 in your browser
