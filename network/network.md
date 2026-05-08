# Przegląd sieci

Ten dokument to moja obecna struktura sieci, składa się z trzech głównych części:
- sieć domowa (router od dostawcy)
- sieć laboratoryjna (maszyny wirtualne)
- sieć VPN (Tailscale)

---

## 1. Sieć domowa (router ISP)

Jest to podstawowa sieć dostarczona przez operatora internetu.

- działa jako brama do internetu
- ma ograniczone możliwości konfiguracji (zarządzana przez dostawcę)
- wszystkie fizyczne urządzenia łączą się przez tę sieć

Rola:
- dostęp do internetu
- baza dla pozostałych środowisk

---

## 2. Sieć laboratoryjna (maszyny wirtualne)

To środowisko służy do nauki, testów i eksperymentów.

### Maszyny:
- Ubuntu
- Ubuntu Server
- Kali Linux
- (planowane) pfSense

### Cel:
- nauka sieci
- administracja systemami
- przygotowanie pod przyszły SOC
---

## 3. Sieć VPN (Tailscale)

Sieć oparta na tailscale umożliwiająca bezpieczny dostęp do urządzeń.

### Funkcje:
- dostęp do maszyn spoza domu
- szyfrowana komunikacja
- brak potrzeby wystawiania portów na świat

### Zastosowanie:
- zdalny dostęp (SSH)
- zarządzanie maszynami
- dostęp do labu z dowolnego miejsca

---

## Plany na przyszłość

- dodanie pfSense (firewall, segmentacja sieci)
- budowa podstawowego SOC (monitoring, logi)
- poprawa bezpieczeństwa
- lepsza dokumentacja przepływu ruchu

---

## Notatki

Maszyny się widzą, jest między nimi komunikacja zarówno dzięki tailscale oraz z routera lokalnego.
Ssh działa poprawnie między maszynami.