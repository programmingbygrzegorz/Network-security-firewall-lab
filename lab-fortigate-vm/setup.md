# Wdrożenie FortiGate-VM w VMware Workstation Pro

Na czas labu FortiGate działa w VMware Workstation Pro. Hosty ESXi i vCenter z Vmware-infrastructure-lab są osobnymi maszynami w segmencie MGMT.

1. Pobierz obraz `FGT_VM64-v7.6.x...FORTINET.out.ovf.zip` z portalu Fortinet Support.
2. Rozpakuj archiwum. W Workstation wybierz **File → Open** i wskaż plik `.ovf`, nadaj nazwę VM i zaimportuj.
3. W **Edit → Virtual Network Editor** przygotuj sieci (przykład):
   - `VMnet8` (NAT) lub Bridged → port1 / WAN
   - `VMnet10` (Host-only) → port2 / MGMT
   - `VMnet20` → port3 / K3S-MONITORING
   - `VMnet30` → port4 / OPENSTACK
   - `VMnet40` → port5 / DMZ
4. W ustawieniach VM przypisz karty sieciowe do tych sieci, w kolejności portów.
5. Uruchom VM, zaloguj się na konsoli (`admin`, puste hasło, wymuszona zmiana).
6. Skonfiguruj adres interfejsu zarządzania i dostęp HTTPS/SSH.
7. Aktywuj licencję ewaluacyjną (konto FortiCare). Wersja ewaluacyjna ma ograniczenia zasobów i funkcji.
8. Zrób kopię konfiguracji i zanonimizuj ją przed dodaniem do `configs/`.

Szczegóły: do uzupełnienia w trakcie budowy labu.
