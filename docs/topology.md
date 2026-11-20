# Topologia labu

FortiGate-VM64 (FortiOS 7.6, VMware Workstation Pro) jako brzeg sieci labu.
Hosty ESXi i vCenter z [Vmware-infrastructure-lab](https://github.com/programmingbygrzegorz/Vmware-infrastructure-lab) są w segmencie MGMT.

| Segment | VLAN | Przeznaczenie |
|---|---|---|
| WAN | - | Uplink do routera domowego |
| MGMT | 10 | ESXi, vCenter, Proxmox, stacja admina |
| K3S-MONITORING | 20 | Klaster k3s, Prometheus, Grafana, Zabbix |
| OPENSTACK | 30 / 31 | Controller, compute, sieć provider |
| DMZ | 40 | Reverse proxy, publikowane usługi |

## Lokalizacja B

Sophos Firewall (VM na Proxmox) chroni drugą sieć domową. Tunel IPsec site-to-site FortiGate ↔ Sophos łączy obie lokalizacje.

Adresacja: do uzupełnienia (prywatne zakresy RFC 1918, wyłącznie labowe).

## Przepływy

- MGMT → wszystkie segmenty: zarządzanie (SSH, HTTPS)
- K3S-MONITORING → FortiGate: SNMP polling
- FortiGate → K3S-MONITORING: syslog
- WAN → DMZ: tylko opublikowane usługi (VIP)
- Domyślnie: deny między segmentami
