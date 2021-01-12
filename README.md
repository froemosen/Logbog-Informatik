# **Logbog-Informatik**
#### Indholdsfortegnelse:
  1. [Makeraktiviteter](#makeraktiviteter)
      * [3D-print af geometriske figurer](#3d-print-af-geometriske-figurer)
  2. [Systemdesign](#systemdesign)
      * [Analyse af Tello-dronen som IT-system](#analyse-af-tello-dronen-som-it-system)
  3. [Sikkerhed og Privatliv](#sikkerhed-og-privatliv)
      * [Sikkerhed og privacy](#sikkerhed-og-privacy)
      * [Password-sikkerhed](#password-sikkerhed)
      * [HTTP vs HTTPS](#http-vs-https)
      * [Privacy, crypto og VPN](#privacy-crypto-og-vpn)
      * [John Snowden](#john-snowden)
  4. [Programmering](#programmering)
      * [Python](#python)
      * [Arduino (C++)](#arduino-c++)
  5. [Projekt Dronecontroller](#projekt-dronecontroller)
      * [Iteration 0](#iteration-0)
      * [Iteration 1](#iteration-1)
      * [Design af brugerundersøgelse](#design-af-brugerundersøgelse)
      * [Iteration 1.5](#iteration-15)
      * [Resultater af brugerundersøgelse](#resultater-af-brugerundersøgelse)
      * [Iteration 2](#iteration-2)
  6. [Webtek](#webtek)
      * Tilføj flere her
  7. [Logbog (Markdown)](#logbog)
      * [Markdown](#markdown)
  8. <a href="https://youtu.be/dQw4w9WgXcQ?t=43" target="_blank" rel="noopener noreferrer">NGGYU</a> 
      <!--Herover ville normalt åbne linket i ny tab, men understøttes ikke af github. (Normal HTML) -->
 
## Makeraktiviteter
### 3D-print af geometriske figurer

  * Vi fik til opgave at fremstille forskellige geometriske figurer, og printe dem på skolens 3D-printere. Herunder ses en guide de forskellige trin i processen, som er at 3D-printe. 

  * #### **_Skabning af 3D-modeller (Fusion 360)_**
    * I Fusion 360 kan man konstruere forskellige 3D-modeller og derefter eksportere dem til blandt andet ".stl"- filer som kan bruges i Cura til at printe ens figurer. Programmet viser også mål på alle figurerne, hvilket gør det let og overskuligt at lave sine figurer i de rigtige størrelser.                    
    **Funktionerne som kan bruges er mange, men her er nogle af dem:**
      1. Oprette forskellige figurer (Box, cylinder, kugle, torus, rør, mm.)
      2. Ekstruder og sammentræk ud fra markeringer på figur
      3. Afrunde kanter
      4. Udhule på forskellig vis
      5. Sammensætte figurer
      6. Standard formattering (Flyt, skalér, rotér, osv)
      
         **_Som sagt er der mange flere funktioner i Fusion 360, men der var nogle af dem._** 

    * Screenshot fra Autodesk Fusion 360:
    
    
    ![Screenshot fra Autodesk Fusion 360](Autodesk.PNG "Screenshot fra Autodesk Fusion 360")
      
    
  * #### **_Slicing (Indstillinger i Cura)_**
    * Ultimaker Cura kan tage 3D-modeller og omdanne dem til ".gcode"-filer, som er kommandoer som en 3D-printer kan forstå. Det fungerer ret enkelt, ved at give lange lister med specifikke kommandoer til printeren om, hvor den skal placere sig over tid, samt en lang række indstillinger. Hele processen som man skal foretage sig i Cura kan beskrives som:
      
      1. Vælg printer
      2. Indsæt fil (.stl, .3mf, .amf, .obj, mm.)
      3. Vælg indstillinger
      4. Slice
      5. Eksporter og print
      
    * Her ses et billede med indstillingerne som kan ændres i Cura:
    ![Screenshot fra Ultimaker Cura](CuraSettings.PNG "Screenshot fra Ultimaker Cura")
      
  * #### **_Udregning af afvigelse på mål_**
    * Man kan bruge følgende formel for at udregne afvigelsen på diverse størrelser på et 3D-print:
    
    ![Udregning af afvigelse](afgUdregning.PNG "Udregning af afvigelse")
    



## Systemdesign
  ### Analyse af Tello-dronen som IT-system
  * Vi fik til opgave at analyse Tello-dronen som et IT-system, ved at benytte nye tillærte analysemetoder. Analysen skulle senere bruges som base til selv at konstruere en dronecontroller. Disse metoder, og deres sammenhæng med tellodronen ses i følgende afsnit. Generelt fandt vi ud af at systemet bestod af to grundlæggende bestanddele. Selve dronen og telefonen der styrer den. 
  
  * #### **_Blokdiagram over systemets bestanddele_**
    * Herunder ses et boksdiagram som inddeler systemet i de to hovedbestanddele (dronen og telefonen), og angiver hvilke dele der hører til de to.
    
    <p/>
    
    ![Tello-Drone Boksdiagram](TelloDroneBoksdiagram.png "Tello-Drone Boksdiagram")
  
  * #### **_Navigation: Analyse af dronens IMU (inertial measurement unit) og nedadseende kamera_**
    * Dronen består primært af to forskellige dele, som den kan bruge til at vide hvor
dens flyvning skal justeres:
      * IMU (inertial measurement unit):
        * _Dronens IMU består af fire forskellige sensorer. Et barometer, et termometer, et gyroskop og et accelerometer. Her er hvad de hver især kan registrere:_
          * _**Barometer** - Højde (Lufttryk)._
          * _**Termometer** - Bruges ved kalibrering, for at kende lufttemperatur._
          * _**Gyroskop** - Dronens rotation på alle akser._
          * _**Accelerometer** - Ændringer i hastighed._
      * Nedadseende kamera:
        * _Dronens **nedadseende kamera** bruges til at registrere både afstand (højde) og fart. Dette kan teknisk set også registreres ud fra barometeret, gyroskopet og accelerometeret, men kameraet fungerer som en slags dobbeltsikring, for at værdierne er korrekte. Derudover kan kameraet også være mere præcis end alternativerne. Kameraet har også en laser, som bruges til at måle højden._

  * #### **_Kommunikaion mellem telefon og drone_**
    * TELLO-dronen bruger wifi til at kommunikere med telefonen. Dette er tydeligt, da
dronen selv opretter et hotspot, som man skal tilslutte sin telefon til. Protokollen der
bliver brugt hedder UDP. 
    * **Sådan virker wifi:**
      * Wifi virker ved at sende data gennem elektromagnestisk stråling, lidt ligesom
radio, tv-signal og gps fungerer. Dog er den primære forskel på disse systemer,
at wifi er lavet til at kunne sende meget mere data ad gangen, hvilket dog
betyder at den data ikke kan udsendes særligt langt. Dette opnås ved at hæve
frekvensen på signalet, hvilket gør at der er flere svingninger i lyset per sekund.
(Derfor ses det for eksempel at 5 GHz internet er hurtigere end 2,4 GHz
internet).
Signalet sender den binære kode ude som systemet har sammensat, og denne
binære kode kan ses enten ved små ændringer i frekvensen, som der bliver
gjort ved fm-radio, eller ændringer i styrken af signalet, som ved am-radio. Dog
er wifi-signaler er en blanding af disse, for at kunne sende op til 8 bits hver
gang der kommer en ny lysbølge, i stedet for kun at sende 1 bit. 

  * #### **_Inddeling af systemet i 3-lagsarkitekturen_**
    * Herunder ses en tabel som inddeler systemet i 3-lagsarkitekturen:
  
  |    |  Dronen  |  Telefonen  |
  |----|----|----|
  |  **Præsentationslaget**  |  - Blinkende LED-indikator <br/>- Sluk-/tænd-knap  |  - Live-kamera<br/>**UI:**<br/>1.	Højde og Horisontal hastighed<br/>2.	Settings<br/>3.	Forprogrammerede kommandoer vælges af bruger (Nævnes i ”1.”)<br/>4.	Styring af dronen (joysticks)<br/>5.	Flere funktioner (Optag video, Tag billede)  |
  |  **Logiklaget**  |  - Behandling af input fra telefonen <br/>- Stabilitet fra sensorer <br/>- Send info til telefon <br/>- Information om interne systemer behandles, for at kunne sendes til telefon  |  - Klargør data fra dronen som skal vises til brugeren<br/>- Layout af UI<br/>- Udsending og fortolkning af brugerinput  |
  |  **Datalaget**  |  - Nedadseende kamera og laser<br/>- Input fra telefon<br/>**IMU:**<br/>1.	Accelerometer<br/>2.	Termometer<br/>3.	Barometer<br/>4.	Gyrometer<br/>- Information om interne systemer (Batteri, osv.)<br/>- Kollisions-detektion fra propellerne  |  - Settings<br/>**Data fra dronen:**<br/>1.	Fejlkoder<br/>2.	Højde<br/>3.	Batteri<br/>4.	Horisontal hastighed<br/>5.	Kamera-feed  |
  
  * #### **_Flowchart over en simpel mission for dronen_**
    * Herunder ses et flowchart over en simpel mission for dronen:
    
    <p/>
    
    ![Tello-Drone Flowchart](FlowchartTelloMission.png "Tello-Drone Flowchart")




## Sikkerhed og privatliv
### Sikkerhed og privacy
  * **IT-sikkerhed**:
    * Beskyttelse af information (Kreditkortinfo, kodeord, persondata generelt, osv)
    * God IT-sikkerhed findes når det er svært at få uautoriseret adgang til.
    * IT-sikkerhed er både vigtigt på personligt og på større plan (Firmaer, statssystemer, osv.)
    
  * **Fortrolighed, integritet og tilgængelighed:**
    * Fortrolighed:
      * Fortrolighed dækker over at kun de personer og systemer som bør have adgang til systemet har adgang. Altså at ingen kan få uautoriseret agang. 
    * Integritet:
      * Integritet dækker over at brugerere og systemer får de rigtige data, når de anmoder om dem. (F.eks. at man får vist de rigtige beløber i sin bank)
    * Tilgængelighed:
      * Alle den som er autoriserede til at benytte systemet bør have nem adgang til det. Ellers mister systemet relevans.
    * CIA-modellen:
       
       ![CIA-modellen](https://blog.jamestyson.co.uk/wp-content/uploads/2019/09/CIA-triad.png "CIA-modellen")
  
  * **Privacy:**
    * **_Stadig under debat_**
    * Bør man være anonym på nettet?
    * Hvor mange personoplysninger må sider lagre om dets brugere?
    * Må staten få adgang til brugeres privatliv?
    
  * **Brugere og hackere:**
    * Et IT-system er ofte ikke bedre end brugerne på det.
      * Hvis brugere ikke beskytter sig selv ordentligt, er deres kontoer nemmere at få fat i. (Beskytter deres kodeord, vælger gode kodeord, osv.)
    * Usikkerheder i IT-systemer:
      * Menneskelige fejl kan frigive følsomme oplysninger.
      * Password leaks. (Sker oftere end man skulle tro)
    * Hvorfor bliver systemer hacket?
      * Penge (Kreditkortoplysninger mm.)
      * Politiske årsager (For at finde snavs om kandidater mm.)
      * Berømmelse
      * Forskning og udvikling af antivirus-software (Legitimt arbejde for at finde huller i IT-sikkerheden)
  
### Password-sikkerhed
  * **Lastpass eller lignende:**
    * Der findes masser af services som _sikkert_ kan opbevare ens kodeord, hvilket gør at man kan have uendeligt mange autogenerede kodeord, som er ekstremt svære at knække.
    * Når man har forskellige kodeord, er det i værste fald kun én sides kodeord som bliver leaked, hvis at en side skulle komme til at leake brugernes kodeord. Det betyder at man ikke mister nært så meget ved et eventuelt leak. 
  * **Tjek sikkerheden på dit kodeord:**
    * Man kan finde hjemmesider som beregner (i cirkatal) hvor længe det ville tage en normal computer at cracke dit kodeord. Benytter man de langetogenrede kodeord som benytter sig af en masse forskellige typer tegn, når man hurtigt op på enormt lange crackingstider. Dette betyder at det er sværere for hackere at skaffe ens kodeord. 
  * **haveibeenpwned:**
    * På [haveibeenpwned](www.haveibeenpwned.com) kan man se om ens mailadresse har fået kodeord leaked. Dette kan give en idé om man bør ændre sit kodeord. 
  
### HTTP vs HTTPS
  * **HTTP:**
    * HTTP står for Hypertext Transfer Protocol, og er en protokol til at sende informationer mellem bruger og hjemmeside. HTTP kræver intet certifikat for at kunne bruges. 
  * **HTTPS:**
    * HTTPS står for Hypertext Transfer Protocol Secure, og fungerer lidt ligesom HTTP, bortset fra at det er sikkert. Det kræver et CAA-certifikat for en hjemmeside at benytte sig af https, et certifikat som gives ud af en [CA](https://en.wikipedia.org/wiki/Certificate_authority). Dette betyder at ens forbindelse er sikret, og at man kan være mere tryg på HTTPS, end på HTTP. 

### Privacy, crypto og VPN
  * VPN:
    * En VPN (Virtual Private Network) er en service, hvor en bruger kobler sig på en server et sted i verden, og browser igennem den server. Det gør at ens internetudbyder ikke længere kan se hvilke sider man besøger, udover generelt at være mere sikkert fra ondsindede personer på nettet. 

### John Snowden
  * Vi så filmen "Snowden", som filmatiserer hvordan John Snowden afslørede hemmelig masseovervågning af befolkninger verden over, som blev foretaget af blandt andet NSA og CIA. Filmen sætter tanker i gang om privacy, og har også været med til at lave en del af den lovgivning som blandt andet EU er i gang med at indføre, for at sikre borgernes ret til privatliv.  

## Programmering
### Python
  * Python er et relativt simpelt programmeringssprog, som ikke er begrænset til én platform. Det er specielt på den måde, at det er lavet til at være "readable", hvilket betyder at der er ændringer på syntaksen i forhold til mange andre programmeringssprog. I python betyder _indents_ noget for hvad bestemte dele af koden tilhører, i stedet for at man skal bruge "{}". Meget specielt er også at man ikke skal deklarere datatyper på sine variabler når man laver dem. En anden fordel ved python er også, at det kører meget hurtigt, og at man ikke skal konstruere sit program i python for at kunne køre det, ligesom i mange andre sprog. 
  
  
    I Informatik har vi brugt python til at kode vores dronecontroller, ved også at benytte tellopy-biblioteket.
### Arduino (C++)
  * En arduino er en lille computer, som man kan bruge til alverdens ting. I Informatik har vi brugt arduinoer til at konstruere vores dronecontroller. Arduinoen kører maskinkode, som konstrueres ud fra c++ sammen med Arduino IDE. I c++ koder man arduinoen, til at frigive den data man gerne vil have. Arduinoen har forskellige porte som kan registrere input, og det er vigtigt at koden registrer de rigtige data. Til sidst i koden til arduinoen udskriver arduinoen data i serial, som derefter kan registreres i python. 


## Projekt Dronecontroller
### Iteration 0

### Iteration 1

### Design af brugerundersøgelse

### Iteration 1.5

### Resultater af brugerundersøgelse

### Iteration 2




## Webtek




## Logbog
### Markdown
  * Til skrivning af logbogen benytter vi github's readme-dokument, som benytter sig af Markdown-formatet (.md). Markdown er en simplificeret version af HTML, som gør det nemmere at lave simple webbaserede dokumenter. Ud over den syntaks som er lavet specielt til Markdown, for at gøre det simpelt, kan man stadig benytte sig af normal HTML, hvilket gør at det ikke er begrænset, selvom det er simplificeret. 
  
  * [Guide til Markdown](https://guides.github.com/features/mastering-markdown/)
