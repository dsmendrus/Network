# Tailscale – notatki

## Co to jest Tailscale?

Tailscale to narzędzie VPN, które pozwala łączyć urządzenia w jedną prywatną sieć, nawet jeśli znajdują się w różnych miejscach.

---

## Po co go używam?

- dostęp do maszyn spoza domu
- brak potrzeby otwierania portów na routerze
- bezpieczne połączenie między urządzeniami
- dostęp do labu z każdego miejsca

---

## Jak to działa 

- każde urządzenie dostaje adres IP z zakresu 100.x.x.x
- urządzenia komunikują się bezpośrednio (peer-to-peer)
- połączenie jest szyfrowane
- działa niezależnie od mojej sieci domowej

---

## Moja konfiguracja

### Urządzenia podłączone:

- laptop
- PC
- maszyny wirtualne: ubuntu serwer, kali

---

## Co już zrobiłem

- zainstalowałem Tailscale na:
  - pc
  - lapku
  - Linux
- zalogowałem urządzenia do jednej sieci
- sprawdziłem połączenie (ping / ssh)
- na stronie tailscale nazałem sobie prościej maszyny
---


## Problemy / błędy

czasami nie chciało sięza pierwszym razem wskoczyć, więc musiałem się dwa razy logować, i na stronie tailscale admin też miałem urządzenia nieaktywne mimio że włączyłem zainstalowałem, włączyłem i podłączyłem go


### Rozwiązanie:
wyłącz włącz xd

---

## Komendy (do zapamiętania)

```bash
tailscale up
tailscale status
tailscale ip