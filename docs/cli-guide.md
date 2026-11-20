# Przewodnik po CLI FortiOS

`get` i `show` tylko czytają, `config` zmienia konfigurację, `execute` wykonuje akcje, `diagnose` służy do diagnostyki. Adresy pochodzą z labu. ⚠ = polecenie przerywa pracę lub usuwa dane.

## Status i sprzęt

Pierwsze polecenia po zalogowaniu

```
get system status
```
Model, wersja FortiOS, numer seryjny, licencja, tryb pracy.

```
get system performance status
```
Zużycie CPU i pamięci, liczba sesji, uptime.

```
diagnose sys top 5 20
```
Procesy najbardziej obciążające CPU (odświeżanie co 5 s, 20 pozycji). Wyjście: q.

```
get system interface physical
```
Stan fizyczny portów: link, prędkość, duplex.

```
diagnose hardware deviceinfo nic port1
```
Szczegóły karty sieciowej: liczniki błędów, MAC, prędkość.

## Konfiguracja: składnia

Każda zmiana w CLI ma tę samą strukturę: config → edit → set → next → end

```
config system interface
    edit "port3"
        set alias "K3S-MONITORING"
        set ip 10.10.20.1 255.255.255.0
        set allowaccess ping https ssh snmp
    next
end
```
Ustawienie aliasu, adresu i dozwolonego dostępu na interfejsie.

```
config firewall address
    edit "k3s-ingress"
        set subnet 10.10.20.10 255.255.255.255
    next
end
```
Obiekt adresu, używany potem w regułach.

```
config firewall policy
    edit 0
        set name "MGMT-to-K3S"
        set srcintf "port2"
        set dstintf "port3"
        set srcaddr "all"
        set dstaddr "k3s-ingress"
        set action accept
        set schedule "always"
        set service "HTTPS" "SSH"
        set logtraffic all
    next
end
```
Nowa reguła firewalla (edit 0 = następny wolny numer).

```
show firewall policy
```
Konfiguracja reguł (tylko wartości zmienione względem domyślnych).

```
show full-configuration system interface port3
```
Pełna konfiguracja obiektu razem z wartościami domyślnymi.

```
show firewall policy | grep -f K3S
```
Filtrowanie wyniku: -f pokazuje cały blok, w którym jest dopasowanie.

```
abort
```
Wyjście z bloku config bez zapisywania zmian.

## Routing

Tablica routingu i protokoły dynamiczne

```
get router info routing-table all
```
Pełna tablica routingu (trasy statyczne, podłączone, dynamiczne).

```
get router info routing-table details 10.10.30.5
```
Którą trasą pójdzie ruch do konkretnego adresu.

```
get router info ospf neighbor
```
Sąsiedzi OSPF i stan sąsiedztwa.

```
get router info bgp summary
```
Sesje BGP i liczba otrzymanych prefiksów.

```
get system arp
```
Tablica ARP: które adresy IP i MAC widać na interfejsach.

## Testy łączności

Ping i traceroute z samego firewalla

```
execute ping-options source 10.10.20.1
execute ping 10.10.20.10
```
Ping z wybranego adresu źródłowego (ważne przy testach przez tunel VPN).

```
execute traceroute 1.1.1.1
```
Ścieżka do celu.

```
execute ping-options reset
```
Przywrócenie domyślnych opcji pinga.

## Sesje

Aktywne połączenia przechodzące przez firewall

```
diagnose sys session filter clear
diagnose sys session filter dst 10.10.20.10
diagnose sys session filter dport 443
diagnose sys session list
```
Lista sesji pasujących do filtra: kto, dokąd, która reguła, NAT, liczniki.

```
diagnose sys session stat
```
Statystyki tabeli sesji.

```
diagnose sys session clear
```
⚠ Usuwa sesje pasujące do filtra (bez filtra: WSZYSTKIE). Na produkcji ostrożnie.

## Diagnostyka ruchu: debug flow

Najważniejsze narzędzie: pokazuje krok po kroku, co firewall zrobił z pakietem i którą regułą go przepuścił albo odrzucił

```
diagnose debug reset
diagnose debug flow filter clear
diagnose debug flow filter addr 10.10.20.10
diagnose debug flow filter proto 6
diagnose debug flow show function-name enable
diagnose debug console timestamp enable
diagnose debug enable
diagnose debug flow trace start 20
```
Śledzenie 20 pakietów do/z 10.10.20.10 (TCP). W wyniku szukaj: „Allowed by Policy-X” albo „Denied by forward policy check”.

```
diagnose debug disable
diagnose debug reset
```
Zawsze wyłącz debug po zakończeniu.

## Przechwytywanie pakietów: sniffer

Odpowiednik tcpdump w FortiOS

```
diagnose sniffer packet any 'host 10.10.20.10 and port 443' 4 20 l
```
Interfejs any, filtr w składni tcpdump, poziom 4 (nagłówki + nazwa interfejsu), 20 pakietów, czas lokalny. Przerwanie: Ctrl+C.

```
diagnose sniffer packet port1 'icmp' 1
```
Tylko ICMP na porcie WAN, poziom 1 (same nagłówki).

## VPN IPsec

Stan i diagnostyka tuneli

```
get vpn ipsec tunnel summary
```
Krótki stan wszystkich tuneli: up/down, liczba selektorów.

```
diagnose vpn ike gateway list
```
Szczegóły fazy 1 (IKE): adres drugiej strony, wersja IKE, czas życia.

```
diagnose vpn tunnel list
```
Szczegóły fazy 2: selektory ruchu, SPI, liczniki pakietów, błędy.

```
diagnose vpn ike log filter rem-addr4 203.0.113.20
diagnose debug application ike -1
diagnose debug enable
```
Debug negocjacji IKE z konkretnym peerem (np. Sophos). Pokazuje, na którym etapie i dlaczego tunel się nie zestawia.

## HA

Klaster wysokiej dostępności

```
get system ha status
```
Rola jednostek (primary/secondary), stan synchronizacji, monitorowane interfejsy.

```
diagnose sys ha checksum cluster
```
Sumy kontrolne konfiguracji na każdej jednostce. Różne wartości = konfiguracja nie jest zsynchronizowana.

```
execute ha manage 1 admin
```
Przełączenie konsoli na drugą jednostkę klastra.

## Logi i monitoring

Wysyłka na zewnątrz (Zabbix / Grafana w labie)

```
config log syslogd setting
    set status enable
    set server "10.10.20.30"
end
```
Wysyłanie logów do serwera syslog.

```
config system snmp sysinfo
    set status enable
end
```
Włączenie agenta SNMP (dalej: community lub użytkownik SNMPv3).

```
diagnose log test
```
Wygenerowanie testowych wpisów w logach, żeby sprawdzić, czy docierają do serwera.

## Utrzymanie

Kopie, aktualizacje, restart

```
execute backup config tftp fgt-lab.conf 10.10.10.50
```
Kopia konfiguracji na serwer TFTP (są też warianty ftp i usb).

```
diagnose autoupdate versions
```
Wersje baz FortiGuard (IPS, AV) i data ostatniej aktualizacji.

```
execute update-now
```
Wymuszenie aktualizacji baz FortiGuard.

```
execute reboot
```
⚠ Restart urządzenia.

```
execute factoryreset
```
⚠ Przywrócenie ustawień fabrycznych, usuwa całą konfigurację.
