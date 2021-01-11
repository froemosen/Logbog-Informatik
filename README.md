# **Logbog-Informatik**
#### Indholdsfortegnelse:
  1. [Makeraktiviteter](#makeraktiviteter)
      * [3D-print af geometriske figurer](#3d-print-af-geometriske-figurer)
  2. [Systemdesign](#systemdesign)
      * [Analyse af Tello-dronen som IT-system](#analyse-af-tello-dronen-som-it-system)
  3. [Sikkerhed og Privathed](#sikkerhed-og-privathed)
      * Tilføj flere her
  4. [Programmering](#programmering)
      * Tilføj flere her
  5. [Projekt Dronecontroller](#projekt-dronecontroller)
      * Tilføj flere her
  6. [Webtek](#webtek)
      * Tilføj flere her
  7. [Logbog (Markdown)](#logbog)
      * Tilføj flere her
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
    

## Sikkerhed og privathed




## Programmering

## Projekt Dronecontroller

## Webtek

## Logbog
### Markdown
  
