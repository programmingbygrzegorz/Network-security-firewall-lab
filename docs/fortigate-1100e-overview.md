# FortiGate 1100E: przegląd platformy

> Źródło: publiczny datasheet Fortinet „FortiGate 1100E Series”. Bez danych z mojego miejsca pracy.

Firewall NGFW klasy enterprise / data center, 2U, oparty o układy ASIC Fortinetu:
- **NP6**: procesor sieciowy, przyspiesza firewall, NAT, IPsec i multicast
- **CP9**: procesor treści, odciąża inspekcję SSL/TLS, IPS i antywirus

Model ma status legacy. Następcą jest seria 900G z NP7.

## Wydajność (datasheet)

| Parametr | Wartość |
|---|---|
| Firewall IPv4 (1518 / 512 / 64 B UDP) | 80 / 80 / 45 Gbps |
| Opóźnienie firewalla (64 B UDP) | 3 µs |
| Pakiety na sekundę | 67,5 Mpps |
| Równoczesne sesje TCP | 8 mln |
| Nowe sesje TCP/s | 500 000 |
| Polityki firewalla | 100 000 |
| IPsec VPN (512 B) | 48 Gbps |
| Tunele IPsec gateway-to-gateway | 20 000 |
| Tunele IPsec client-to-gateway | 100 000 |
| SSL inspection | 10 Gbps |
| Application control | 26 Gbps |
| IPS (enterprise mix) | 12,5 Gbps |
| NGFW | 9,8 Gbps |
| Threat Protection | 7,1 Gbps |

## Interfejsy

- 2× 40 GE QSFP+
- 4× 25 GE SFP28 / 10 GE SFP+ / GE SFP
- 4× 10 GE SFP+ / GE SFP
- 8× GE SFP
- 16× GE RJ45
- 2× GE RJ45 MGMT/HA, port konsoli, 2× USB
- Redundantne zasilacze hot-swap

## Funkcje FortiOS, z którymi pracuję

- Polityki NGFW: kontrola aplikacji, filtrowanie WWW, polityki oparte o użytkowników
- NAT, obiekty i grupy adresów, VIP
- IPS, antywirus, inspekcja SSL/TLS
- VPN IPsec: site-to-site i dostęp zdalny
- HA (FGCP), aktualizacje FortiOS
- Analiza logów i diagnostyka (`diagnose debug flow`, `diagnose sniffer packet`)
- VDOM, SD-WAN, routing dynamiczny, Security Fabric

## Doświadczenie (ogólnie)

Administracja klastrem FortiGate 1100E w dużej instytucji sektora publicznego: zarządzanie politykami, VPN, aktualizacjami i rozwiązywanie problemów z łącznością.
