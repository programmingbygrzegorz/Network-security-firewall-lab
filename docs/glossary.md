# Słowniczek pojęć

Wyjaśnienia terminów używanych w opisie FortiGate 1100E i w labie.

## Interfejsy i sprzęt

**GE (Gigabit Ethernet)**: Port o przepustowości 1 Gbit/s. Zapis „10 GE”, „25 GE” czy „40 GE” oznacza odpowiednio 10, 25 i 40 Gbit/s.

**RJ45**: Zwykłe gniazdo na kabel miedziany (skrętkę), takie jak w laptopie czy przełączniku biurowym. Zasięg do ok. 100 m.

**SFP**: Gniazdo na wymienny moduł (transceiver) 1 Gbit/s. Moduł wybiera się do medium: światłowód wielomodowy, jednomodowy albo miedź. Dzięki temu jeden port obsłuży różne kable i odległości.

**SFP+**: Wersja SFP dla 10 Gbit/s. Ten sam rozmiar gniazda, zwykle przyjmuje też moduły SFP 1G.

**SFP28**: Wersja dla 25 Gbit/s. Zwykle obsługuje też moduły SFP+ (10G) i SFP (1G), stąd zapis „SFP28 / SFP+ / SFP”.

**QSFP+**: Większe gniazdo (Quad SFP) łączące cztery tory po 10 Gbit/s, czyli 40 Gbit/s. Używane do szybkich połączeń z rdzeniem sieci lub data center.

**MGMT / HA**: MGMT to port wydzielony do zarządzania urządzeniem (GUI, SSH), oddzielony od ruchu użytkowników. HA to port heartbeat, którym dwa firewalle w klastrze wymieniają stan i synchronizują konfigurację.

**PSU hot-swap**: PSU to zasilacz. Dwa zasilacze dają redundancję: urządzenie działa po awarii jednego z nich. Hot-swap oznacza wymianę bez wyłączania firewalla.

**NP6**: Network Processor, układ ASIC Fortinetu. Przejmuje od procesora głównego przetwarzanie pakietów: firewall, NAT, IPsec. Stąd tak duże przepustowości przy niskim opóźnieniu.

**CP9**: Content Processor, drugi układ ASIC. Przyspiesza operacje na treści: szyfrowanie i deszyfrowanie SSL/TLS, dopasowywanie sygnatur IPS i antywirusa.


## System i polityki

**FortiOS**: System operacyjny wszystkich FortiGate'ów, sprzętowych i wirtualnych. Te same polecenia CLI i ten sam GUI działają na 1100E i na FortiGate-VM w labie.

**NGFW (Next-Generation Firewall)**: Firewall „nowej generacji”. Klasyczny firewall decyduje na podstawie adresów IP i portów. NGFW rozpoznaje dodatkowo aplikacje, użytkowników i zawartość ruchu, a do tego ma wbudowane IPS, antywirusa i filtrowanie WWW.

**Polityka (reguła) firewalla**: Reguła mówiąca: ruch z tej strefy/adresu do tamtej, dla tych usług, w tym czasie: zezwól albo zablokuj, z opcjonalnym NAT i profilami bezpieczeństwa. Reguły sprawdzane są od góry, a na końcu jest domyślne „deny”.

**Kontrola aplikacji**: Rozpoznawanie aplikacji po charakterystyce ruchu, a nie po porcie. Pozwala np. zablokować BitTorrent albo ograniczyć pasmo dla YouTube, nawet jeśli działają na porcie 443.

**Filtrowanie WWW**: Blokowanie lub dopuszczanie stron według kategorii (np. hazard, malware, media społecznościowe) z bazy FortiGuard albo według własnych list.

**Polityki oparte o użytkowników**: Reguły przypisane do użytkownika lub grupy (np. z Active Directory lub Entra ID), a nie tylko do adresu IP. Księgowość ma dostęp do systemu finansowego niezależnie od tego, przy którym komputerze siedzi.


## Ochrona przed zagrożeniami

**IPS (Intrusion Prevention System)**: System zapobiegania włamaniom. Porównuje ruch z sygnaturami znanych ataków i exploitów (np. próby wykorzystania podatności w serwerze WWW) i blokuje je w locie.

**AV (antywirus)**: Skanowanie plików przesyłanych przez firewall (HTTP, poczta, FTP) w poszukiwaniu złośliwego oprogramowania, zanim dotrą do użytkownika.

**Inspekcja SSL/TLS (deep inspection)**: Większość ruchu jest szyfrowana, więc IPS i AV „nie widzą” zawartości. Firewall odszyfrowuje ruch, sprawdza go i szyfruje ponownie. Wymaga zaufanego certyfikatu firewalla na komputerach i mocno obciąża CPU, stąd sprzętowe wsparcie CP9.

**FortiGuard**: Usługi subskrypcyjne Fortinetu dostarczające aktualne sygnatury IPS, bazy antywirusa, kategorie stron i reputację adresów IP.


## VPN

**VPN (Virtual Private Network)**: Zaszyfrowany tunel przez internet, dzięki któremu odległe sieci lub użytkownicy działają tak, jakby byli w sieci lokalnej.

**IPsec**: Standardowy zestaw protokołów do budowy tuneli VPN. Klucze uzgadnia protokół IKE (dziś zwykle IKEv2), dane szyfruje ESP. Standard, więc działa między różnymi producentami, np. FortiGate ↔ Sophos.

**Site-to-site**: Tunel łączący dwie całe sieci, np. centralę z oddziałem. Zestawiany między dwoma firewallami, użytkownicy nic nie konfigurują.

**Dostęp zdalny (remote access)**: VPN dla pojedynczego użytkownika, np. pracownika z laptopem w domu. Łączy się klientem VPN z firewallem w firmie.

**FortiClient**: Aplikacja Fortinetu instalowana na komputerze użytkownika, służąca m.in. jako klient VPN do FortiGate.


## Architektura i wysoka dostępność

**VDOM (Virtual Domain)**: Podział jednego fizycznego FortiGate'a na kilka niezależnych, wirtualnych firewalli. Każdy ma własne interfejsy, polityki, routing i administratorów. Przydatne np. do oddzielenia sieci różnych jednostek lub klientów.

**HA (High Availability)**: Wysoka dostępność: dwa (lub więcej) firewalle pracują jako klaster. Gdy jeden padnie, drugi przejmuje ruch bez przerwy w pracy sieci.

**FGCP (FortiGate Clustering Protocol)**: Protokół Fortinetu do budowy klastra HA. Wybiera jednostkę główną (primary), synchronizuje konfigurację i monitoruje stan urządzeń przez porty HA.

**Active-passive**: Tryb HA, w którym ruch obsługuje tylko jednostka główna, a zapasowa czeka w gotowości i przejmuje pracę przy awarii.

**Active-active**: Tryb HA, w którym obie jednostki przetwarzają ruch. Jednostka główna rozdziela część sesji (głównie inspekcję) na drugą, co zwiększa wydajność.

**Synchronizacja sesji**: Kopiowanie tabeli aktywnych połączeń między jednostkami klastra. Po przełączeniu otwarte połączenia (np. sesja RDP) nie zrywają się.


## Routing i WAN

**SD-WAN**: Software-Defined WAN: inteligentne wykorzystanie kilku łączy internetowych naraz. Firewall mierzy jakość łączy (opóźnienie, utrata pakietów) i kieruje ruch, np. wideorozmowy, przez najlepsze z nich.

**Routing dynamiczny**: Routery i firewalle same wymieniają informacje o sieciach i budują trasy, zamiast wpisywać je ręcznie (routing statyczny).

**OSPF**: Protokół routingu dynamicznego używany wewnątrz jednej organizacji. Szybko reaguje na awarię łącza i wybiera najkrótszą ścieżkę.

**BGP**: Protokół routingu, na którym działa internet. Wymienia trasy między niezależnymi sieciami (np. z operatorami), używany też w dużych sieciach firmowych i SD-WAN.


## Ekosystem Fortinet

**Security Fabric**: Spięcie produktów Fortinetu w jeden system: wspólny widok topologii, wymiana informacji o zagrożeniach i automatyczne reakcje (np. odcięcie zainfekowanego komputera na przełączniku).

**FortiAnalyzer**: Centralny system zbierania i analizy logów z wielu FortiGate'ów: raporty, korelacja zdarzeń, długie przechowywanie.

**FortiManager**: Centralne zarządzanie konfiguracją wielu FortiGate'ów: wspólne polityki i obiekty, aktualizacje, wdrażanie zmian na wiele urządzeń naraz.

**FortiSwitch**: Przełączniki Fortinetu zarządzane bezpośrednio z FortiGate'a (FortiLink): VLAN-y i porty konfiguruje się z panelu firewalla.

**FortiAP**: Punkty dostępowe Wi-Fi Fortinetu, którymi zarządza FortiGate pełniący rolę kontrolera sieci bezprzewodowej.

