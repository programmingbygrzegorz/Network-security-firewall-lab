# Przewodnik po GUI FortiOS

FortiGate-VM w labie ma ten sam interfejs co urządzenia sprzętowe (np. 1100E). Część menu włącza się w **System → Feature Visibility**.

## 1. Dashboard

`Dashboard → Status / Security / Network / Users & Devices`

Ekran startowy z widżetami: informacje o systemie (model, wersja FortiOS, licencje), zużycie CPU i pamięci, liczba sesji, ruch na interfejsach, najaktywniejsze źródła i aplikacje (FortiView). Widżety można dodawać i układać.

**W labie:** Sprawdzić wersję FortiOS, licencję ewaluacyjną i obciążenie VM.

_Zrzut ekranu z labu: wkrótce_

## 2. Security Fabric

`Security Fabric → Physical / Logical Topology, Fabric Connectors, Automation, Security Rating`

Mapa urządzeń i sieci podłączonych do FortiGate'a, integracje z innymi systemami (np. chmura, SDN, Active Directory), automatyzacja (wyzwalacz → akcja, np. e-mail po wykryciu ataku) i ocena konfiguracji według dobrych praktyk.

**W labie:** Uruchomić Security Rating i poprawić wskazane problemy, dodać automatyzację z powiadomieniem.

_Zrzut ekranu z labu: wkrótce_

## 3. Network

`Network → Interfaces, DNS, SD-WAN, Static Routes, Policy Routes, OSPF / BGP, Packet Capture, Diagnostics`

Konfiguracja interfejsów fizycznych i VLAN-ów (adres IP, rola, dozwolony dostęp administracyjny: HTTPS, SSH, PING, SNMP), DNS, routing statyczny i dynamiczny, SD-WAN oraz przechwytywanie pakietów z poziomu GUI.

**W labie:** Nadać aliasy portom (WAN, MGMT, K3S-MONITORING, OPENSTACK, DMZ), utworzyć trasy i zrobić Packet Capture ruchu do k3s.

_Zrzut ekranu z labu: wkrótce_

## 4. Policy & Objects

`Policy & Objects → Firewall Policy, Addresses, Services, Schedules, Virtual IPs, IP Pools, Traffic Shaping`

Serce firewalla. Reguły (skąd, dokąd, jaka usługa, akcja, NAT, profile bezpieczeństwa) oraz obiekty, z których są budowane: adresy i grupy adresów, usługi, harmonogramy. Virtual IPs służą do publikacji usług z wnętrza sieci (port-forward).

**W labie:** Utworzyć obiekty segmentów, reguły między strefami z domyślnym deny i VIP do Ingress w k3s.

_Zrzut ekranu z labu: wkrótce_

## 5. Security Profiles

`Security Profiles → AntiVirus, Web Filter, DNS Filter, Application Control, Intrusion Prevention, File Filter, SSL/SSH Inspection`

Profile, które podpina się do reguł firewalla. Określają, co sprawdzać w dozwolonym ruchu: wirusy, kategorie stron, zapytania DNS, aplikacje, sygnatury ataków, typy plików, oraz czy odszyfrowywać SSL/TLS.

**W labie:** Zbudować profil IPS dla DMZ i sprawdzić blokadę na teście EICAR i skanie nmap.

_Zrzut ekranu z labu: wkrótce_

## 6. VPN

`VPN → IPsec Tunnels, IPsec Wizard, VPN Location Map`

Tunele IPsec: kreator dla typowych scenariuszy (site-to-site, dostęp zdalny dla FortiClient) i tryb własny z pełną kontrolą nad fazą 1 (IKE) i fazą 2 (selektory ruchu). Lista tuneli pokazuje ich stan i przesłane dane.

**W labie:** Zestawić tunel IKEv2 FortiGate ↔ Sophos i dostęp zdalny dla FortiClient.

_Zrzut ekranu z labu: wkrótce_

## 7. User & Authentication

`User & Authentication → User Definition, User Groups, LDAP Servers, RADIUS Servers, FortiTokens, Authentication Settings`

Lokalni użytkownicy i grupy, podłączenie do zewnętrznych katalogów (LDAP / Active Directory, RADIUS), tokeny MFA. Grupy wykorzystuje się w regułach firewalla i w VPN.

**W labie:** Utworzyć lokalną grupę VPN-LAB i przypisać ją do dostępu zdalnego.

_Zrzut ekranu z labu: wkrótce_

## 8. WiFi & Switch Controller

`WiFi & Switch Controller → FortiLink Interface, Managed FortiSwitches, Managed FortiAPs, SSIDs`

Zarządzanie przełącznikami FortiSwitch i punktami dostępowymi FortiAP bezpośrednio z FortiGate'a. W labie bez fizycznego sprzętu menu służy głównie do zapoznania się z opcjami.

**W labie:** Tylko przegląd (brak sprzętu w labie).

_Zrzut ekranu z labu: wkrótce_

## 9. System

`System → Administrators, Admin Profiles, Firmware & Registration, Settings, HA, SNMP, Certificates, FortiGuard, Feature Visibility`

Ustawienia samego urządzenia: konta administratorów i ich uprawnienia, aktualizacja firmware, nazwa hosta, NTP, strefa czasowa, klaster HA, SNMP, certyfikaty, aktualizacje FortiGuard. W Feature Visibility włącza się ukryte domyślnie menu (np. routing dynamiczny, VDOM-y).

**W labie:** Utworzyć osobne konto administratora z profilem tylko do odczytu, włączyć SNMP dla Zabbix, ustawić NTP.

_Zrzut ekranu z labu: wkrótce_

## 10. Log & Report

`Log & Report → Forward Traffic, Local Traffic, System Events, Security Events, Log Settings`

Przeglądanie logów: ruch przechodzący przez firewall (Forward), ruch do samego urządzenia (Local), zdarzenia systemowe i bezpieczeństwa (IPS, AV, Web Filter). W Log Settings ustawia się wysyłkę logów na zewnątrz (syslog, FortiAnalyzer).

**W labie:** Wysłać logi przez syslog do stosu monitoringu w segmencie K3S-MONITORING.

_Zrzut ekranu z labu: wkrótce_
