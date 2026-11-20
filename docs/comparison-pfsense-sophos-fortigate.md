# FortiGate vs pfSense vs Sophos Firewall

| Obszar | FortiGate (FortiOS) | pfSense | Sophos Firewall (SFOS) |
|---|---|---|---|
| Model | Komercyjny NGFW, appliance lub VM, subskrypcje FortiGuard | Open source (FreeBSD), CE i Plus (Netgate) | Komercyjny NGFW, appliance XGS lub VM |
| Wydajność | Sprzętowe ASIC (NP/CP) | Zależna od sprzętu x86 | Architektura Xstream, FastPath |
| Zarządzanie | GUI + rozbudowane CLI, FortiManager | GUI WWW, pakiety (Suricata, HAProxy) | GUI WWW, Sophos Central |
| Segmentacja | VLAN, strefy, VDOM | VLAN, interfejsy, aliasy | VLAN, strefy |
| VPN | IPsec, FortiClient | IPsec, OpenVPN, WireGuard | IPsec, SSL VPN, Sophos Connect |
| Ekosystem | Security Fabric | Niezależny | Synchronized Security (Heartbeat) |
| Zastosowanie | Duże organizacje, data center | MŚP, homelab | MŚP, średnie firmy |

## Wnioski z praktyki

Do uzupełnienia: obserwacje z pracy z każdą platformą (np. diagnostyka, aktualizacje, łatwość tworzenia polityk).
