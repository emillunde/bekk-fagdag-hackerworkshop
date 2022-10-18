# Hackerworkshop Bekk Fagdag 21.10.2022

I denne workshopen skal du bryte deg inn på en ruter, kartlegge nettverket, finne sårbarheter i en webserver og skaffe deg tilgang til serveren og dens database.

## Engasjementsregler

- Det er bare tillatt å angripe wifi-nettverket kalt HackMe og enheter tilknyttet ruterens lokale nettverk på `192.168.38.0/24`.

- Tjenestenektangrep mot ruteren eller andre enheter på nettverket er ikke tillatt da det vil ødelegge for andre.

- Siden alle er koblet på samme nettverk er det lurt å unngå permanente endringer av filer, rettigheter og lignende dersom du skaffer deg tilgang til en server.

## Oppgaver

### Bryt deg inn på wifi-nettverket
Utnytt svakhetene i WPS vha. [OneShot](https://github.com/drygdryg/OneShot) til å skaffe deg passordet til nettverket. Repoet er allerede klonet på maskinen og ligger på rot-nivå. 

<details>
<summary>Hint 1</summary>
Naviger til OneShot-repoet i en terminal og kjør oneshot.py med riktig options.
</details>

<details>
<summary>Hint 2</summary>
Dere må spesifisere riktig trådløst grensesnitt ved å bruke -i flagget. Trådløst grensesnitt kan du finne med kommandoen ```iwconfig```.
</details>

<details>
<summary>Hint 3</summary>
Om det tar lang tid, kan det hende dere har glemt å spesifisere hvilket angrep OneShot skal kjøre, ved å kjøre ```--pixie-dust```.
</details>

<details><summary>Løsningsforslag</summary>
  
```
sudo python oneshot.py -i wlan0 --pixie-dust
```

Velg nettverket HackMe.

</details>

### Kartlegg hva som fins på det lokale nettverket
Finn ut hvilke enheter og porter som er tilgjengelige.

<details><summary>Hint</summary>
Nmap kan brukes til å scanne et nettverk etter tilgjengelige enheter og åpne porter.
</details>

### Undersøk ut om det er noe interessant på en av webserverene

<details><summary>Hint</summary>

- `gobuster` kan brukes til å kartlegge hva en webserver inneholder ved å gi den en liste med mappe- og filnavn. For å ikke overbelaste serveren er det fint om du bruker en relativt kort ordliste, f.eks. [denne](https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/common.txt). Et stort utvalg av diverse ordlister finner man [her](https://github.com/danielmiessler/SecLists).
  
  ```gobuster dir -u http://192.168.38.101:8000 -w common.txt```

</details>

 ### Finn ut hva slags teknologi som er i bruk
  
<details><summary>Hint</summary>
  
Hvilke filer som lastes og hvilke headere som returneres kan gi mye informasjon om hvilken teknologi som er i bruk. Wappalyzer er også en nyttig addon/extension man kan installere i nettleseren som lister opp den underliggende teknologien.

</details>

### Kjente sårbarheter?

Når man vet hvilken teknologi som brukes er ofte neste steg å finne ut nøyaktig hvilke versjoner som brukes. Deretter kan man undersøke om det er kjente sårbarheter tilknyttet disse versjonene.

<details><summary>Hint</summary>

Kali har et verktøy kalt wpscan som kan gi deg mye informasjon om en Wordpress-server.

</details>

### Bryt deg inn på serveren

### Finn et flagg på serveren

### Skaff deg tilgang til databasen

### Finn et flagg i databasen
