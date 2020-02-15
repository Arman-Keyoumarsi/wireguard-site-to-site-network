WireGuard Site to Site Network
=========

This role enables you to create a mesh network of fully connected WireGuard servers and add any additional client to all the servers. This gives you the ability to create a geo-location diverse vpn infrastructure on demand. 

<p align="center">
  <img width="460" height="300" src="">
</p>


Requirements
------------

 * You need to make sure the firewall ports are open for the external connections
 * You need to have access to all the defined nodes

Role Variables
--------------

There are Additional variables that can be setup for the following:

 * wireguard_path: change the default path for config and key files.
 * wireguard_network_name: Change the default Wireguard interface name
 * wireguard_clinet_peers: Enable additional client configuration for mobile clients
    * The shall include sub vars as follow:
      * '- client_name: Client name' 
      * ip: Wireguard ip of the client
      * key: Client public key 

To make sure we have full flexibility for configuring each node independently there are number of required variables that need to be defined in hostvars. 

 * local_ip: This is the local ip of the server and it is required to find the NIC for NAT rules.
 * public_ip: This is the external ip of the server where it can be reached from internet (endpoint)
 * wg_ip: The internal ip you want to use for WireGuard network
 * ListenPort: VPN port for each server
 * allowed_ips: This is the range of networks you want to allow other servers have access to. This variable is for Site to Site configuration
 * wireguard_post_up: To enable internet NAT rules for external client access to internet in each server.
 * wireguard_post_down: To disable internet NAT rules for external client access to internet in each server.

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------
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
