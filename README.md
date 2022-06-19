This is an ONPREM Kubernetes cluster. Orchetrated using KUBEADM.
VM automation used is VAGRANT.

Vagrant: automate three VM box "ubuntu" on VIRTUALBOX

Kubeadm: define scripts is attached which contain both the kubeadm master node "master.sh" and "worker.sh.
- master.sh: needs to be run on the master node 
- worker.sh: needs to be run on each on the worker node.

The above setup and configuration require to have both virtualbox and vagrant install on your local pc/machine.
After a successful installation of both vagrant and virtualbox,

To provision the cluster, execute the following commands.

git clone https://github.com/donydonald1/vagrant-kubeadm.git
cd vagrant-kubeadm
vagrant up

vagrant will provision 3 worker nodes and 1 master. You can customize number of worker nodes in the Vagrantfile;
NUM_WORKER_NODE = 3

After master and worker nodes have been successfully provision, you will need to ssh into each node and orchestrate/provision kubernetes using kubeadm. The folder "scripts" have two scripts to use.

the command ssh into each node, you must be in the folder vagrant-kubeadm;
cd vagrant-kubeadm
vagrant ssh "nodeofnode" 
e.g 
   vagrant ssh kubemaster 
   vagrant ssh kubenode01

- on your virtualbox dashboard, you should see the name of these nodes which is the name of the virtual machine created.

copy https://github.com/donydonald1/vagrant-kubeadm/blob/main/scripts/install_master.sh into any path in kubemaster node and run the script as administrator "sudo".
e.g 
   if you saved file as install_master.sh, run;
   sudo sh install_master.sh

copy https://github.com/donydonald1/vagrant-kubeadm/blob/main/scripts/install_worker.sh into any path in each kubenode node and run the script as administrator "sudo".
e.g 
   if you saved file as install_worker.sh on each on of the worker nodes, run;
   sudo sh iinstall_worker.sh

If script "install_master.sh" successfully execueted, it sould be provide you with a key/command to run on each worker node. This command a the kubeadm preflight access for each node to join the master node. 

Note:
   at this point, you should be able to perform kubectl commands.
   e.g
      kubectl get node
      kubectl get pod -A

the key/command should look like;

### COMMAND TO ADD A WORKER NODE ###
kubeadm join 192.168.56.2:6443 --token fwmobl.mf7l94z2ntyjtvrz --discovery-token-ca-cert-hash sha256:5f590a346851c7ff1dfb52b19727a088f97bc3790349807a4a70cd4e4880d23d 
