# iptables

```yml
---

ip6tables: True
iptables:
  ssh_port: 2234
  iptables_chain:
    - name: INPUT
      policy: ACCEPT
      rules:
        tcp_dport_accept:
          - 80
          - 443
          - 25 #smtp
          - 587 #smtps
          - 143 #imap
          - 993 # imaps
        udp_sport_dport_accept:
          - sport: 5353
            dport: 5353
      pipes_to:
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
