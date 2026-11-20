# Network Security & Firewall Lab

**Strona projektu:** https://programmingbygrzegorz.github.io/Network-security-firewall-lab/

Lab bezpieczeństwa sieci oparty na doświadczeniu z trzema platformami: **FortiGate** (produkcyjnie, sektor publiczny), **pfSense** i **Sophos Firewall**.
Wirtualny FortiGate (FortiGate-VM64, VMware Workstation Pro) jest brzegiem sieci dla moich pozostałych labów: [k3s-monitoring-lab](https://github.com/programmingbygrzegorz/k3s-monitoring-lab) i [openstack-lab](https://github.com/programmingbygrzegorz/openstack-lab).
Sophos Firewall działa jako VM na Proxmoxie ([Proxmox-virtualization-lab](https://github.com/programmingbygrzegorz/Proxmox-virtualization-lab)) i tworzy drugą lokalizację, połączoną z FortiGate tunelem IPsec site-to-site.

## Zawartość

| Ścieżka | Opis |
|---|---|
| `index.html` | Strona projektu (GitHub Pages) |
| `docs/fortigate-1100e-overview.md` | Funkcje platformy FortiGate 1100E na podstawie publicznego datasheetu |
| `docs/glossary.md` | Słowniczek pojęć: interfejsy, NGFW, IPS, VPN, VDOM, HA, SD-WAN, Security Fabric |
| `docs/comparison-pfsense-sophos-fortigate.md` | Porównanie trzech platform |
| `docs/topology.md` | Topologia i adresacja labu |
| `lab-fortigate-vm/` | Wdrożenie FortiGate-VM, konfiguracje i scenariusze |

## Scenariusze

1. Wdrożenie FortiGate-VM w VMware Workstation Pro (OVF, licencja ewaluacyjna)
2. Segmentacja: VLAN-y, strefy, polityki między strefami
3. Publikacja usług z k3s i OpenStack (VIP)
4. Profile bezpieczeństwa: IPS, kontrola aplikacji, filtrowanie WWW
5. IPsec site-to-site FortiGate ↔ Sophos
6. Zdalny dostęp VPN: FortiClient i Sophos Connect
7. Monitoring obu firewalli: SNMP i syslog → Zabbix / Grafana
8. Automatyzacja: REST API FortiOS, Ansible

## Zasada publikacji

Repozytorium **nie zawiera** żadnej konfiguracji, adresacji, nazw, zrzutów ekranu ani topologii z mojego miejsca pracy.
Wszystkie konfiguracje i zrzuty ekranu pochodzą z wirtualnego FortiGate w labie. Dane sprzętowe 1100E pochodzą z publicznego datasheetu Fortinet.

---
FortiGate, FortiOS, pfSense i Sophos są znakami towarowymi ich właścicieli.
