WireGuard Site to Site Network
=========

This role enables you to create a mesh network of fully connected WireGuard servers and add any additional client to all the servers. This gives you the ability to create a geo-location diverse vpn infrastructure on demand. 

[![Ansible Role](https://img.shields.io/ansible/role/d/46588)](https://galaxy.ansible.com/arman_keyoumarsi/wireguard_site_to_site_network) [![Ansible Quality](https://img.shields.io/ansible/quality/46588)](https://galaxy.ansible.com/arman_keyoumarsi/wireguard_site_to_site_network)

<p align="center">
  <img src="/files/WireGuard.png">
</p>


Requirements
------------

 * You need to make sure the firewall ports are open for the external connections
 * You need to have access to all the defined nodes
 * Role should be installed using Ansibel Galaxy
 
 ```bash
 ansible-galaxy install arman_keyoumarsi.wireguard_site_to_site_network
 ```

Role Variables
--------------

There are Additional variables that can be setup for the following:

 * _wireguard_path_: change the default path for config and key files.
 * _wireguard_network_name_: Change the default Wireguard interface name
 * _wireguard_clinet_peers_: Enable additional client configuration for mobile clients
    * The shall include sub vars as follow:
      * '- client_name: Client name' 
      * ip: Wireguard ip of the client
      * key: Client public key 

To make sure we have full flexibility for configuring each node independently there are number of required variables that need to be defined in hostvars. 

 * _local_ip_: This is the local ip of the server and it is required to find the NIC for NAT rules.
 * _public_ip_: This is the external ip of the server where it can be reached from internet (endpoint)
 * _wg_ip_: The internal ip you want to use for WireGuard network
 * _ListenPort_: VPN port for each server
 * _allowed_ips_: This is the range of networks you want to allow other servers have access to. This variable is for Site to Site configuration
 * _wireguard_post_up_: To enable internet NAT rules for external client access to internet in each server.
 * _wireguard_post_down_: To disable internet NAT rules for external client access to internet in each server.

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

## Inventory
```yml
wg:
 hosts:
   192.168.0.70:
     local_ip: 192.168.0.70
     public_ip: 192.168.0.70
     wg_ip: 10.13.113.1/24
     ListenPort: 5566
     allowed_ips: 10.10.0.0/16, 10.13.113.1/24
     wireguard_post_up: true
     wireguard_post_down: true
   192.168.0.71:
     local_ip: 192.168.0.71
     public_ip: 192.168.0.71
     wg_ip: 10.13.113.2/24
     ListenPort: 5566
     allowed_ips: 192.168.4.0/24, 10.13.113.2/24
     wireguard_post_up: true
     wireguard_post_down: true
```


## Playbook
```yml
---
- hosts: wg
  gather_facts: yes
  become: yes
  become_method: sudo
  roles:
    - wireguard-site-to-site-network
  vars:
      wireguard_client_peers:
          - client_name: mobile
            ip: 10.13.113.10/32
            key: if4Za/1xhb1HgpRZqA3AN5n8d4At8FWYZYsqNo0XYi8=
          - client_name: Laptop
            ip: 10.13.113.11/32
            key: if4Za/1xhb1HgpRZqA3AN5n8d4At8FWYZYsqNo0XYi8=
```
License
-------

[MIT](LICENSE)

Author Information
------------------

Originally created by [Arman Keyoumarsi](https://github.com/Arman-Keyoumarsi)
 * Inspired from [wireguard-private-networking](https://github.com/mawalu/wireguard-private-networking)
