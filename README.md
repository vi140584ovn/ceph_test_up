# ceph_test_up

1. Install environment
   - ansible
   - virtualbox
   - vagrant
   - vagrant-dns

2. Install vagrant-dns `vagrant plugin install vagrant-dns`

3. git clone git@github.com:vi140584ovn/ceph_test_up.git

4. cd ceph_test_up

5. VAGRANT_EXPERIMENTAL=disks vagrant up

6. vagrant dns --install

7. cd ansible

8. ansible-playbook -i hosts Install_Env.yml 
