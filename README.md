# kubectl_kvm2_minikube
step by step

originally tried to use QEMU, but faced an odd error:

curl -LO https://dl.k8s.io/release/`curl -LS https://dl.k8s.io/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client

apt-get install qemu-system

curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && chmod +x minikube
  sudo mkdir -p /usr/local/bin/
  
  On this step, I faced an error which is related to OVMF_CODE.fd file location. Solved via symlink:
  sudo ln -s /usr/share/OVMF/OVMF_CODE_4M.fd /usr/share/OVMF/OVMF_CODE.fd
  
  minikube start --driver=qemu --force    
  
  failed with connection refused and apiserver stopped.
_______________
switch to kvm2:
grep -E -q 'vmx|svm' /proc/cpuinfo && echo yes || echo no
apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils

virsh list --all

virsh net-list --all
___________
minikube start --driver=kvm2 --force

success
