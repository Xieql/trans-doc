cp -rfp inventory/sample inventory/mycluster  # 复制临时配置

declare -a IPS=(36.137.235.80, 36.137.236.195)
CONFIG_FILE=inventory/mycluster/hosts.yaml python3  contrib/inventory_builder/inventory.py ${IPS[@]}

cat inventory/mycluster/hosts.yaml

cat inventory/mycluster/group_vars/all/all.yml
cat inventory/mycluster/group_vars/k8s_cluster/k8s-cluster.yml

查看配置
cat inventory/mycluster/hosts.yaml
cat inventory/mycluster/group_vars/all/all.yml
cat inventory/mycluster/group_vars/k8s_cluster/k8s-cluster.yml

