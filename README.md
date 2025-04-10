
Proiect-TSC
Acest proiect își propune să construiască un e-reader open-source, bazat pe microcontrollerul ESP32-C6, cu un display e-Paper de 7.5", o baterie Li-Po reîncărcabilă și conexiuni Wi-Fi/BLE. Scopul este obținerea unui dispozitiv cu consum de energie redus, ideal pentru citirea de cărți și documente în format electronic.

1. Diagrama bloc
Fișierul block_diagram.png descrie schema generală a sistemului, incluzând:

ESP32-C6 – Microcontroller principal

Ecran e-Paper – Interfață SPI

Baterie Li-Po + modul de încărcare

Conectivitate USB pentru încărcare și date

Buton/Buton BOOT pentru interacțiunea cu utilizatorul (reset, mod debug etc.)

2. Bill of Materials (BOM)
Tabelul de mai jos prezintă principalele componente folosite în proiect. Pentru toate componentele, datasheet-urile sunt indicate pentru a facilita selectarea și verificarea specificațiilor.

Referință	Valoare / Cod	Dispozitiv / Tip	Descriere	Link / Datasheet
ESP32-C6	ESP32-C6-DevKitC-1	Placă DevKitC-1 ESP32-C6	Microcontroller Wi-Fi 6 + BLE 5	Mouser Link
Datasheet
Ecran e-Paper	7.5" Waveshare V2	Interfață SPI	Afișaj E-Ink 800×480	Waveshare Link
Exemplu datasheet
Baterie Li-Po	3.7V / 1800mAh	Cellevia LP584174 (exemplu)	Sursă portabilă de alimentare	Exemplu datasheet
Modul încărcare	TP4056 / similar	TP4056	Încărcare baterie Li-Po via USB	TP4056 Datasheet
Regulator LDO	AMS1117-3.3V	SOT223 / SOT25	Stabilizator 3.3V	AMS1117 Datasheet
Buton push	PBS-110 / BOOT_BTN	6×6mm tact switch	Buton de interacțiune	Omron B3F
Diverse	Condensatori, rezistențe, diode TVS, conectori USB-C, etc.	-	Pentru filtre, protecție și fixare tensiune	Vezi fișierul complet BOM din repository pentru link-uri și datasheet-uri suplimentare
3. Descriere hardware și funcționalitate
Microcontroller – ESP32-C6

Oferă conectivitate Wi-Fi 6 și Bluetooth 5.

Gestionează afișajul e-paper și comunică prin SPI.

Are GPIO-uri pentru interacțiuni suplimentare (buton, senzor de lumină, etc.).

Ecran e-Paper (7.5")

Rezoluție de 800×480 pixeli, consum foarte redus în mod static.

Interfață SPI pentru actualizare.

Perfect pentru citirea textelor mari datorită contrastului ridicat și lipsei de flicker.

Alimentare și baterie

Baterie Li-Po de 3.7V, 1800mAh (sau altă capacitate dorită).

Încărcare prin port USB (5V) folosind un modul dedicat (TP4056 sau similar).

Regulator LDO (3.3V) pentru alimentarea ESP32-C6 și a perifericelor.

Buton / BOOT

Permite resetul microcontrollerului, intrarea în mod de programare sau alte interacțiuni definite de utilizator (ex. schimba paginile, meniu, etc.).

Consum și durată baterie

În mod de citire (fără actualizări frecvente), e-paper consumă extrem de puțin.

În momentul schimbării paginii (actualizare e-paper), consumul crește scurt pe durată.

Estimare durată baterie: poate varia de la câteva zile la câteva săptămâni, în funcție de frecvența actualizărilor ecranului și de modul de somn al ESP32-C6.

4. Pin Mapping ESP32-C6
Tabelul de mai jos prezintă pinii importanți folosiți în proiect pentru conectarea ecranului e-paper și a butonului:

Funcționalitate	Pin ESP32-C6	Descriere
SPI CLK	GPIO6	Semnal de ceas SPI
SPI MOSI	GPIO7	Linie de date
E-Paper CS	GPIO10	Chip Select display
E-Paper DC	GPIO5	Selectare date/cmd
E-Paper RST	GPIO4	Reset pentru e-paper
E-Paper BUSY	GPIO3	Linie READY (ocupat)
Buton	GPIO0/1/etc.	Buton reset/interacțiune
UART TX/RX	GPIO21/20	Debug (opțional)
(Notă: pinii pot fi modificați în funcție de necesitățile tale, dar schema de mai sus oferă un exemplu util.)

5. Cum să începi (Software)
Clonează repository-ul:

bash
Copiază
Editează
git clone https://github.com/WhyNotZebra/Proiect-TSC.git
Instalează suportul pentru ESP32-C6:

Asigură-te că ai ultima versiune de ESP32 Boards în Arduino IDE sau PlatformIO.

Deschide folderul cu codul sursă (ex. software/) în Arduino IDE/PlatformIO:

Exemplu de librării necesare:

Adafruit-GFX

GxEPD2 sau altă librărie dedicată e-paper

Selectează placa:

În Arduino IDE: Tools -> Board -> ESP32C6 DevKit sau varianta corespunzătoare.

Conectează placa prin USB și încărcă (upload) sketch-ul.

Monitorizează comportamentul în Serial Monitor (viteză tipică: 115200 bps) pentru log-uri de debug.

6. Randări și fișiere CAD
Folderul Images/: conține randări 3D (render_device.png, render_pcb.png) și diagrama bloc (block_diagram.png).

Fișierele de design (Eagle, KiCad sau Fusion 360 PCB) se află în folderul hardware/.

Gerbers și BOM detaliată pentru fabricație se găsesc în hardware/output/.

7. Observații de proiectare
Consum redus: E-paper-ul menține imaginea fără consum suplimentar, deci microcontrollerul poate intra des în mod sleep.

Protecția la încărcare: Modului de încărcare TP4056 i se pot adăuga circuite de protecție suplimentare (TVS, diode) pe portul USB-C.

Dimensiuni și ergonomie: Poziționarea butoanelor de navigare la îndemână îmbunătățește experiența de lectură.

8. Licență și contribuții
Acest proiect este disponibil sub licența MIT (sau altă licență deschisă specificată).
Orice contribuții (pull request-uri, issue-uri) sunt încurajate: optimizări hardware, îmbunătățiri de cod, documentație suplimentară etc.

9. Referințe
Documentația oficială ESP32-C6

TP4056 Battery Charger Datasheet

10. Concluzie
Proiectul TSC – E-Reader Hardware oferă o platformă completă pentru a crea un dispozitiv de citire electronică, cu autonomie ridicată și hardware flexibil. Fie că vrei să înveți, să experimentezi sau să creezi propriul tău eReader personalizat, acest repository îți pune la dispoziție:

Schemă și layout PCB

BOM detaliată

Cod sursă configurabil

Documentație pas cu pas