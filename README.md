# iptables

Role to create basic firewall rules

## Role variables

```yml
# Set to True if want the same rules to be applied on ipv6
ip6tables: False
# File where rules are saved
iptables_restore_file: /etc/iptables.up.rules
# File where rules are saved for ipv6
ip6tables_restore_file: /etc/ip6tables.up.rules
# Pre up script path
iptables_pre_up_file: /etc/network/if-pre-up.d/iptables
# Pre up script path for ipv6
ip6tables_pre_up_file: /etc/network/if-pre-up.d/ip6tables
# iptables rules
iptables:
  ssh_port: 22
  iptables_chain:
    - name: INPUT
      policy: ACCEPT
    - name: FORWARD
      policy: ACCEPT
    - name: OUTPUT
      policy: ACCEPT
```

## Example configuration

```yml
---

ip6tables: True
iptables:
  ssh_port: 2234
  iptables_chain:
    - name: INPUT
      policy: ACCEPT
      rules:
        # Accept connection to these tcp ports
        tcp_dport_accept:
          - 80
          - 443
          - 25 #smtp
          - 587 #smtps
          - 143 #imap
          - 993 # imaps
        # Accept connection to these udp ports
        udp_sport_dport_accept:
          - sport: 5353
            dport: 5353
      pipes_to:
        # Pipe packates from source to custom network
        - chain: CUSTOM_NETWORK
          proto: tcp
          source: 192.168.0.0/24

    - name: FORWARD
      policy: ACCEPT
    - name: OUTPUT
      policy: ACCEPT

    - name: CUSTOM_NETWORK
      policy: '-'
      jump: DROP

      rules:
        tcp_dport_accept:
          - 139 # samba
          - 445 # samba
        udp_dport_accept:
          - 137
          - 138

# vi:sw=2:tabstop=2
```
