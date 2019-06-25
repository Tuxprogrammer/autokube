# AutoKube
## Automated Kubernetes Deployment

These set of scripts assist in provisioning a kubernetes cluster on bare-metal VMware ESXi host. The default ocnfiguration creates the following nodes:

- Kubernetes Master:
  - `earth`
- Kubernetes Workers:
  - `jupiter`
  - `mars`
  - `mercury`
  - `neptune`

It uses `generic/debian9` as the box for each system. The playbooks rely on a debian-based system since the repos are hardcoded for Debian release `stretch`. It would be trivial to adapt the scripts to other Debian-based systems and not much more difficult than that to adapt the script to RHEL systems.

# Usage:

`vagrant plugin install vagrant-vmware-esxi`
`vagrant up --provider=vmware_esxi`

`ansible-playbook -i inventory gitlab-kubernetes.yml`

## Optional:
ansible-playbook -i inventory gitlab-kubernetes.yml

# Todo:

- automate install of gluster-kubernetes with gk-deploy
- write ansible teardown playbook so that you don't have to vagrant destroy to reset the cluster
- ...
- profit?