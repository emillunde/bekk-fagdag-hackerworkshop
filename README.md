# Hackerworkshop Bekk Fagdag 21.10.2022

## Engasjementsregler

- Det er bare tillatt å angripe wifi-nettverket kalt HackMe og enheter tilknyttet ruterens lokale nettverk på `192.168.38.0/24`.

- Tjenestenektangrep mot ruteren eller andre enheter på nettverket er ikke tillatt da det vil ødelegge for andre.

- Siden alle er koblet på samme nettverk er det lurt å unngå permanente endringer av filer, rettigheter og lignende dersom du skaffer deg tilgang til en server.

## Oppgaver

### Bryt deg inn på wifi-nettverket HackMe
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
Nmap kan brukes til å scanne et nettverk etter tilgjengelige enheter og åpne porter.
  
```
sudo nmap -A 192.168.38.0-100

```

</details>

### Finn ut så mye som mulig om serveren på nettverket

### Skaff deg tilgang til serveren

### Finn et flagg på serveren

### Skaff deg tilgang til databasen

### Finn et flagg i databasen
