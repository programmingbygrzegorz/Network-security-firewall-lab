# Wdrożenie FortiGate-VM w VMware

1. Pobierz obraz `FGT_VM64-v7.6.x...FORTINET.out.ovf.zip` z portalu Fortinet Support.
2. Rozpakuj archiwum i zaimportuj plik `.ovf` w VMware (Workstation lub ESXi).
3. Przypisz karty sieciowe: port1 → WAN, port2 → MGMT, kolejne porty → segmenty labu.
4. Uruchom VM, zaloguj się na konsoli (`admin`, puste hasło, wymuszona zmiana).
5. Skonfiguruj adres interfejsu zarządzania i dostęp HTTPS/SSH.
6. Aktywuj licencję ewaluacyjną (konto FortiCare). Wersja ewaluacyjna ma ograniczenia zasobów i funkcji.
7. Zrób kopię konfiguracji i zanonimizuj ją przed dodaniem do `configs/`.

Szczegóły: do uzupełnienia w trakcie budowy labu.
