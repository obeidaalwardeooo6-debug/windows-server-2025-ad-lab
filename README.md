# Windows Server 2025 – Active Directory Lab

## Om prosjektet

I dette prosjektet satte jeg opp et lite Windows Server 2025-miljø med Active Directory Domain Services (AD DS), DNS og en Windows 11 Pro-klient.

Målet var å lære hvordan en Windows Server kan brukes som Domain Controller, hvordan brukere og datamaskiner administreres sentralt, og hvordan en klientmaskin kobles til et Active Directory-domene.

---

## Miljø

### Server

- Operativsystem: Windows Server 2025
- Servernavn: DC01
- Domene: `firma.local`
- IPv4-adresse: `192.168.0.10`
- Subnet mask: `255.255.255.0`
- Default gateway: `192.168.0.1`
- DNS-server: `192.168.0.10`

### Klient

- Operativsystem: Windows 11 Pro
- Maskinnavn: CLIENT-PC
- Plattform: VMware Workstation
- IPv4: DHCP
- DNS-server: `192.168.0.10`
- Domene: `firma.local`

---

## Nettverksstruktur

```text
Router
192.168.0.1
    |
    +---- DC01
    |     192.168.0.10
    |     Active Directory
    |     DNS
    |
    +---- CLIENT-PC
          192.168.0.x
          Windows 11 Pro
```

---

## Gjennomføring

### 1. Installasjon og grunnoppsett av server

Windows Server 2025 ble installert på servermaskinen.

Serveren fikk navnet:

`DC01`

Deretter ble nettverket konfigurert med en fast IPv4-adresse:

- IP-adresse: `192.168.0.10`
- Subnet mask: `255.255.255.0`
- Default gateway: `192.168.0.1`

En fast IP-adresse ble brukt fordi serveren må kunne finnes på samme adresse hele tiden.

---

### 2. Active Directory Domain Services

Rollen Active Directory Domain Services (AD DS) ble installert gjennom:

`Server Manager → Manage → Add Roles and Features`

Etter installasjonen ble serveren promotert til Domain Controller.

Det ble opprettet et domene med navnet:

`firma.local`

Active Directory brukes til sentral administrasjon av brukere, datamaskiner, grupper og rettigheter.

---

### 3. DNS

DNS ble konfigurert på DC01.

DC01 bruker:

`192.168.0.10`

som DNS-adresse.

DNS er viktig i Active Directory fordi klientmaskiner må kunne finne domenet og Domain Controller ved hjelp av navn.

---

### 4. Opprettelse av domenebruker

Verktøyet:

`Server Manager → Tools → Active Directory Users and Computers`

ble brukt for å administrere Active Directory.

Det ble opprettet en domenebruker:

`ola.nordmann`

Brukeren ble senere brukt for å teste pålogging på klientmaskinen.

---

### 5. Oppsett av CLIENT-PC

For å teste domenet ble det opprettet en virtuell klientmaskin i VMware Workstation.

Maskinen fikk navnet:

`CLIENT-PC`

Windows 11 Pro ble installert fordi denne versjonen støtter tilkobling til et Active Directory-domene.

Nettverksadapteren i VMware ble konfigurert som:

`Bridged`

Dette gjorde at CLIENT-PC kunne kommunisere på samme lokale nettverk som DC01.

---

### 6. Kontroll av klientens IP-adresse

På CLIENT-PC ble følgende kommando brukt:

`ipconfig`

Klienten fikk blant annet:

- IPv4-adresse: `192.168.0.101`
- Subnet mask: `255.255.255.0`
- Default gateway: `192.168.0.1`

DC01 hadde adressen:

`192.168.0.10`

Dette viste at serveren og klienten var på samme lokale nettverk.

---

### 7. Test av kommunikasjon

Fra CLIENT-PC ble forbindelsen til DC01 testet med:

`ping 192.168.0.10`

Testen var vellykket.

Resultatet viste:

`Sent = 4, Received = 4, Lost = 0`

Dette bekreftet at CLIENT-PC kunne kommunisere med serveren over nettverket.

---

### 8. DNS-konfigurasjon på CLIENT-PC

CLIENT-PC fikk IP-adressen automatisk via DHCP.

DNS-serveren ble deretter endret manuelt til:

`192.168.0.10`

Dette betyr at CLIENT-PC bruker DNS-tjenesten på DC01 når den skal finne domenet og Active Directory-tjenester.

---

### 9. Test av DNS

DNS ble testet fra CLIENT-PC med:

`nslookup firma.local`

Resultatet viste:

`firma.local → 192.168.0.10`

Dette bekreftet at CLIENT-PC kunne finne domenet ved hjelp av DNS på DC01.

Forskjellen mellom testene var:

`ping 192.168.0.10`

testet om klienten kunne nå serveren gjennom nettverket.

`nslookup firma.local`

testet om DNS kunne finne domenet ved hjelp av navnet.

---

### 10. Domain Join

Etter at nettverk og DNS var kontrollert, ble CLIENT-PC koblet til domenet:

`firma.local`

Windows ba om brukernavn og passord til en konto som hadde tillatelse til å legge datamaskinen inn i domenet.

Domain Join var vellykket, og Windows ba deretter om omstart.

---

### 11. Pålogging med domenebruker

Etter omstart ble:

`Annen bruker`

valgt på innloggingsskjermen.

CLIENT-PC viste at maskinen kunne logge på domenet:

`FIRMA`

Det ble deretter logget inn med domenebrukeren:

`FIRMA\ola.nordmann`

Påloggingen var vellykket.

Dette bekreftet at CLIENT-PC kunne kommunisere med Domain Controller og bruke en konto som var opprettet i Active Directory.

---

### 12. Kontroll i Active Directory

Til slutt ble:

`Server Manager → Tools → Active Directory Users and Computers`

åpnet på DC01.

Under:

`firma.local → Computers`

var:

`CLIENT-PC`

registrert.

Dette bekreftet at Active Directory kjente klientmaskinen som medlem av domenet.

---

## Resultat

Prosjektet resulterte i et fungerende Windows Server 2025-miljø med Active Directory og DNS.

Følgende ble gjennomført:

- Windows Server 2025 ble installert.
- Serveren fikk navnet `DC01`.
- DC01 fikk statisk IP-adresse `192.168.0.10`.
- Active Directory Domain Services ble installert.
- DC01 ble Domain Controller.
- Domenet `firma.local` ble opprettet.
- DNS ble konfigurert.
- En domenebruker ble opprettet.
- Windows 11 Pro ble installert på CLIENT-PC.
- CLIENT-PC kunne kommunisere med DC01.
- CLIENT-PC brukte DC01 som DNS-server.
- DNS-oppslag mot `firma.local` fungerte.
- CLIENT-PC ble medlem av domenet.
- Domenebrukeren kunne logge inn på CLIENT-PC.
- CLIENT-PC ble registrert i Active Directory.

---

## Hva jeg lærte

Gjennom prosjektet lærte jeg hvordan flere deler av et Windows Server-miljø arbeider sammen.

### IP-adresse

En IP-adresse identifiserer en enhet på nettverket.

I prosjektet:

`DC01 = 192.168.0.10`

`CLIENT-PC = 192.168.0.101`

`Router = 192.168.0.1`

### Statisk IP

DC01 fikk en statisk IP-adresse fordi adressen til serveren ikke bør endres.

### DHCP

CLIENT-PC fikk IP-adressen automatisk.

Dette gjøres ved hjelp av DHCP.

### Default Gateway

Default Gateway er veien fra det lokale nettverket til andre nettverk.

I prosjektet var routeren:

`192.168.0.1`

### DNS

DNS brukes til å finne maskiner og tjenester ved hjelp av navn.

CLIENT-PC brukte:

`192.168.0.10`

som DNS-server.

### Active Directory

Active Directory brukes til sentral administrasjon av brukere, datamaskiner, grupper og rettigheter.

### Domain

Et domene samler brukere og datamaskiner i et sentralt administrert miljø.

Domenet i prosjektet var:

`firma.local`

### Domain Controller

En Domain Controller administrerer domenet og kontrollerer domenebrukere og domenemaskiner.

I prosjektet var:

`DC01`

Domain Controller.

### Lokal bruker og domenebruker

En lokal bruker finnes bare på den lokale datamaskinen.

Eksempel:

`LocalAdmin`

En domenebruker finnes i Active Directory.

Eksempel:

`FIRMA\ola.nordmann`

### Workgroup og Domain

En maskin i en Workgroup administreres hovedsakelig lokalt.

En maskin som er medlem av et Domain kan administreres sentralt gjennom Active Directory.

---

## Kommandoer brukt i prosjektet

Kontroll av nettverksinformasjon:

`ipconfig`

Test av forbindelse mellom CLIENT-PC og DC01:

`ping 192.168.0.10`

Test av DNS:

`nslookup firma.local`

---

## Dokumentasjon

Repositoryet skal også inneholde skjermbilder fra viktige deler av prosjektet, blant annet:

- Server Manager
- Active Directory Users and Computers
- DNS
- nettverkskonfigurasjon
- `ipconfig`
- `ping 192.168.0.10`
- `nslookup firma.local`
- Domain Join
- domenepålogging
- CLIENT-PC registrert under Computers i Active Directory

---

## Konklusjon

Gjennom prosjektet fikk jeg praktisk erfaring med Windows Server 2025, nettverk, DNS og Active Directory.

Jeg lærte hvordan en Domain Controller settes opp, hvordan DNS brukes for å finne domenet, hvordan brukere opprettes og administreres sentralt, og hvordan en Windows 11 Pro-klient kobles til og logger inn i et Active Directory-domene.
