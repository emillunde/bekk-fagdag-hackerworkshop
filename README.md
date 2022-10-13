# Hackerworkshop Bekk Fagdag 21.10.2022

## Engasjementsregler

- Det er bare tillatt å angripe wifi-nettverket kalt HackMe og enheter tilknyttet ruterens lokale nettverk på `192.168.38.0/24`.

- Tjenestenektangrep mot ruteren eller andre enheter på nettverket er ikke tillatt da det vil ødelegge for andre.

- Siden alle er koblet på samme nettverk er det lurt å unngå permanente endringer av filer, rettigheter og lignende dersom du skaffer deg tilgang til en server.

## Oppgaver

### Bryt deg inn på wifi-nettverket
Utnytt svakhetene i WPS vha. [OneShot](https://github.com/drygdryg/OneShot) til å skaffe deg passordet til nettverket.

<details><summary>Løsningsforslag</summary>
  
```
sudo python oneshot.py -i wlan0 --pixie-dust
```
Navnet på det trådløse grensesnittet finner du via iwconfig-kommandoen. OneShot vil liste ut nettverkene den finner med WPS aktivert. Velg nettverket kalt HackMe.
</details>

### Kartlegg hva som fins på det lokale nettverket
Finn ut hvilke enheter og porter som er tilgjengelige.

<details><summary>Løsningsforslag</summary>
  
```
sudo nmap -A 192.168.38.0-100
```

Nmap kan brukes til å scanne et nettverk etter tilgjengelige enheter og åpne porter.
</details>

### Finn ut så mye som mulig om serveren(e)
Etter at man har kartlagt hva som er tilgjengelig i nettverket er neste steg å finne ut enda mer om de mest interessante delene. Typisk er dette servere man kan kommunisere med via HTTP, SSH eller andre kjente protokoller. 

Denne oppgaven er ganske åpen og kan løses på mange måter. Ofte er det lurt å gå bredt ut til å begynne med og snevre inn hva man undersøker etterhvert som man skaffer seg mer informasjon. Noen typiske spørsmål man kan prøve å besvare er:
- Hva er det sannsynlig at serveren brukes til?
- Inneholder den noe interessant?
- Hva slags teknologi er i bruk?
- Hvilke versjoner av teknologien brukes?
- Har disse versjonene noen kjente sårbarheter?

<details><summary>Hint 1</summary>

- `gobuster` kan brukes til å kartlegge hva en webserver inneholder ved å gi den en liste med vanlige mappe- og filnavn. For å ikke overbelaste serveren er det fint om du bruker en relativt kort ordliste, f.eks. [denne](https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/common.txt). Et stort utvalg av diverse ordlister finner man [her](https://github.com/danielmiessler/SecLists).
  
  ```gobuster dir -u http://192.168.38.101:8000 -w common.txt```
- Hvilke filer som lastes og hvilke headere som returneres kan gi mye informasjon om hvilken teknologi som er i bruk. Wappalyzer er også en nyttig addon/extension man kan installere i nettleseren som lister opp den underliggende teknologien.

</details>

<details><summary>Hint 2</summary>

Kali har et verktøy som heter wpscan som kan gi deg mye informasjon om en Wordpress-server. Ev. kan man bruke en scanner i Metasploit. Tips til bruk av Metasploit finner du i neste oppgave.

</details>

### Skaff deg tilgang til serveren

### Finn et flagg på serveren

### Skaff deg tilgang til databasen

### Finn et flagg i databasen
