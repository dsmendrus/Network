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
#cmd
ssh-keygen
ren "$env:USERPROFILE\.ssh\config.txt" "config"
ren = Rename-Item
#powershell
scp $env:USERPROFILE\.ssh\id_ed25519.pub kali@kali.et:/tmp/
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0 - instalowanie sshderver na windowsa
#bash
cat /tmp/id_ed25519.pub >> ~/.ssh/authorized_keys
```
## podłączanie urządzeń
na początku robiłem logowanie dzięki ssh ze statycznym adresem ip jakie dał mi tailscale. ale potem dowiedziałem się że można dzięki magicdns na tailscal'u logować się nie przez ip tylko przez wyznaczona nazwę.
teraz soie dorobiłem plik w user/.ssh/config gdzie jeszcze bardziej sobie to ułatwiłem
Host kali
    HostName kali.tail66fc55.ts.net
    User kali
cos w tym stylu i teraz tylko mam jedną komendę i wpisuje hasło
-ssh kali
więc czemu nie pójść kroku dalej i nie dodać logowania za pomocą kluczy. i wtedy nawet nie trzeba haseł Więc
Na windowsiach z których chce się połączyć do maszyn robie
ssh-keygen. czyli generuje klucze prywatne i publiczne
potem przesyłam sobie ten plik na linuxa przeważnie dzięki scp $env:USERPROFILE\.ssh\id_ed25519.pub kali@kali.nt:/tmp/
potem kopiuję sobie do konkretnego pliku dzieki temu poleceniu cat /tmp/id_ed25519.pub >> ~/.ssh/authorized_keys
i mogę się logować tylko używając ssh kali i jestem juz zalogowany

Przydatne jeszcze jest zmiana z config.txt na config rn "$env:USERPROFILE\.ssh\config.txt" "config"

