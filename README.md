# ansible-playbook-change-hostname
A ansible playbook to help fix /etc/hosts

# Overview
This was created from some snippets I found online and put it all together in a single place. This is really just here for my own reference. 

Here are the links that I owe credit to:
* https://support.ansible.com/hc/en-us/articles/201958037-Reboot-a-server-and-wait-for-it-to-come-back

# Howto use this playbook

## Define an inventory file in your current directory
```
[all]
foo-app-01.example.com
foo-app-02.example.com
foo-app-03.example.com
foo-app-03.example.com
```

So, in order for this to work `foo-app*.example.com` must be resolvable by DNS and these are the values you want your hostnames to become.  If your hosts are not resolvable in DNS you can create a node alias in your inventory file as follows.
```
[all]
foo-app-01.example.com  ansible_ssh_host=1.2.3.4
foo-app-02.example.com  ansible_ssh_host=1.2.3.5
foo-app-03.example.com  ansible_ssh_host=1.2.3.6
foo-app-03.example.com  ansible_ssh_host=1.2.3.7
```

After your hostnames are updated, you can also pass a parameter on the command line to reboot.  Here's how to do that.
```
ansible-playbook -e 'reboot=True' -i inventory change_hostname.yaml
```

Finally, if all goes well the last step is a vaildation routine that makes sure your remote clients hostname matches your `inventory_hostname` you defined in your inventory.  You might need to blow away the `tmp/ansible-facts` cache dir for this to succeed, however.  
