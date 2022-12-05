# 安装 kubespray

cd /opt
git clone https://github.com/kubernetes-sigs/kubespray.git
cd kubespray
pip3 install -r requirements.txt -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com

# 配置集群
# Copy ``inventory/sample`` as ``inventory/mycluster``
cp -rfp inventory/sample inventory/mycluster

# Update Ansible inventory file with inventory builder
declare -a IPS=(10.10.1.3 10.10.1.4 10.10.1.5)
CONFIG_FILE=inventory/mycluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}

# 编辑 inventory/mycluster/hosts.yaml 文件

# 使用自定义配置
rm -rf /opt/kubespray/inventory/mycluster/group_vars
mkdir -p /opt/kubespray/inventory/mycluster/group_vars/all
curl https://gist.githubusercontent.com/yankay/a863cf2e300bff6f9040ab1c6c58fbae/raw/ab96867375325c6f43dc1e00ddd4f3d60e472487/customize.yml > /opt/kubespray/inventory/mycluster/group_vars/all/customize.yml
mkdir -p /opt/kubespray/inventory/my/group_vars/k8s_cluster
curl https://gist.githubusercontent.com/yankay/a863cf2e300bff6f9040ab1c6c58fbae/raw/ab96867375325c6f43dc1e00ddd4f3d60e472487/k8s-customize.yml > /opt/kubespray/inventory/mycluster/group_vars/all/k8s-customize.yml

# 运行
cd /opt/kubespray/
export ANSIBLE_CONFIG=/opt/kubespray/ansible.cfg
ansible-playbook -i inventory/mycluster/hosts.yaml  --become --become-user=root cluster.yml