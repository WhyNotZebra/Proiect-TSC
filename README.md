# Proiect TSC – E-Reader Hardware

Acest proiect își propune să construiască un **e-reader open-source**, bazat pe microcontrollerul **ESP32-C6**, cu un display **e-Paper de 7.5"**, o baterie **Li-Po** reîncărcabilă și conexiuni **Wi-Fi/BLE**.  
Scopul este obținerea unui dispozitiv cu **consum de energie redus**, ideal pentru citirea de cărți și documente în format electronic.

---

## Cuprins

- [Diagrama bloc](#diagrama-bloc)
- [Bill of Materials (BOM)](#bill-of-materials-bom)
- [Descriere hardware și funcționalitate](#descriere-hardware-și-funcționalitate)
  - [Microcontroller – ESP32-C6](#microcontroller--esp32-c6)
  - [Ecran e-Paper (7.5")](#ecran-e-paper-75)
  - [Alimentare și baterie](#alimentare-și-baterie)
  - [Buton / BOOT](#buton--boot)
  - [Consum și durată baterie](#consum-și-durată-baterie)
- [Pin Mapping ESP32-C6](#pin-mapping-esp32-c6)
- [Cum să începi (Software)](#cum-să-începi-software)
- [Randări și fișiere CAD](#randări-și-fișiere-cad)
- [Observații de proiectare](#observații-de-proiectare)
- [Licență și contribuții](#licență-și-contribuții)
- [Referințe](#referințe)
- [Concluzie](#concluzie)

---

## Diagrama bloc

Fișierul **block_diagram.png** descrie schema generală a sistemului, incluzând:

- **ESP32-C6** – Microcontroller principal  
- **Ecran e-Paper** – Interfață SPI  
- **Baterie Li-Po + modul de încărcare**  
- **Conectivitate USB** pentru încărcare și date  
- **Buton/Buton BOOT** pentru interacțiunea cu utilizatorul (reset, mod debug etc.)  

---

## Bill of Materials (BOM)

Mai jos este prezentată o listă cu principalele componente folosite în proiect. Pentru toate componentele, datasheet-urile sunt indicate pentru a facilita selectarea și verificarea specificațiilor.

| Referință         | Valoare / Cod               | Dispozitiv / Tip          | Descriere                                 | Link / Datasheet                      |
|-------------------|-----------------------------|---------------------------|-------------------------------------------|----------------------------------------|
| **ESP32-C6**      | ESP32-C6-DevKitC-1         | Placă DevKitC-1 ESP32-C6 | Microcontroller Wi-Fi 6 + BLE 5           | [Mouser Link](#) <br> [Datasheet](#)   |
| **Ecran e-Paper** | 7.5" Waveshare V2          | Interfață SPI            | Afișaj E-Ink 800×480                      | [Waveshare Link](#) <br> [Ex. datasheet](#) |
| **Baterie Li-Po** | 3.7V / 1800mAh             | Cellevia LP584174 (ex.)  | Sursă portabilă de alimentare            | [Exemplu datasheet](#)                |
| **Modul încărcare** | TP4056 / similar         | TP4056                   | Încărcare baterie Li-Po via USB           | [TP4056 Datasheet](#)                 |
| **Regulator LDO** | AMS1117-3.3V              | SOT223 / SOT25           | Stabilizator 3.3V                        | [AMS1117 Datasheet](#)                |
| **Buton push**    | PBS-110 / BOOT_BTN         | 6×6mm tact switch        | Buton de interacțiune (reset, BOOT)       | [Omron B3F](#)                         |
| **Diverse**       | Condensatori, rezistențe, diode TVS, conectori USB-C, etc. | - | Pentru filtre, protecție și fixare tensiune | Vezi fișierul complet BOM din repository |

---

## Descriere hardware și funcționalitate

### Microcontroller – ESP32-C6
- Oferă **conectivitate Wi-Fi 6** și **Bluetooth 5**.  
- Gestionează afișajul e-paper prin **SPI**.  
- Are GPIO-uri disponibile pentru interacțiuni suplimentare (buton, senzori etc.).  

### Ecran e-Paper (7.5")
- **Rezoluție**: 800×480 pixeli.  
- **Consum foarte redus** în mod static (menține imaginea fără consum suplimentar).  
- **Interfață SPI** pentru actualizare.  
- Ideal pentru citirea textelor mari datorită contrastului ridicat și lipsei de flicker.  

### Alimentare și baterie
- **Baterie Li-Po** de 3.7V, 1800mAh (sau altă capacitate, la alegere).  
- **Încărcare prin USB (5V)** cu modul dedicat (TP4056 sau similar).  
- **Regulator LDO (3.3V)** pentru alimentarea ESP32-C6 și a perifericelor.  

### Buton / BOOT
- Permite **resetarea** microcontrollerului, intrarea în **mod de programare** sau alte interacțiuni definite de utilizator (ex. schimbare pagină, meniu etc.).  

### Consum și durată baterie
- În mod de citire (fără actualizări frecvente), **e-paper** consumă extrem de puțin.  
- În momentul schimbării paginii (actualizare e-paper), consumul crește pentru scurt timp.  
- **Estimare durată baterie**: de la câteva zile la câteva săptămâni, în funcție de frecvența actualizărilor și de modul de somn (sleep) al ESP32-C6.  

---

## Pin Mapping ESP32-C6

Tabelul de mai jos prezintă pinii importanți folosiți în proiect pentru conectarea ecranului e-paper și a butonului:

| Funcționalitate | Pin ESP32-C6  | Descriere                 |
|-----------------|--------------|---------------------------|
| **SPI CLK**     | GPIO6        | Semnal de ceas SPI        |
| **SPI MOSI**    | GPIO7        | Linie de date             |
| **E-Paper CS**  | GPIO10       | Chip Select display       |
| **E-Paper DC**  | GPIO5        | Selectare date/command    |
| **E-Paper RST** | GPIO4        | Reset pentru e-paper      |
| **E-Paper BUSY**| GPIO3        | Linie READY (ocupat)      |
| **Buton**       | GPIO0/1 etc. | Buton reset/interacțiune  |
| **UART TX/RX**  | GPIO21/20    | Debug (opțional)          |

> **Notă**: Pinii pot fi modificați în funcție de necesități. Tabelul de mai sus reprezintă doar un exemplu.  

---

## Cum să începi (Software)

1. **Clonează repository-ul**:
    ```bash
    git clone https://github.com/WhyNotZebra/Proiect-TSC.git
    ```

2. **Instalează suportul** pentru **ESP32-C6** în mediul de dezvoltare (Arduino IDE sau PlatformIO).  
   - Asigură-te că ai ultima versiune de **ESP32 Boards** în Arduino IDE / PlatformIO.

3. **Deschide folderul** cu codul sursă (ex. `software/`) în Arduino IDE / PlatformIO.  
   - Exemple de librării necesare:  
     - `Adafruit-GFX`  
     - `GxEPD2` (sau altă librărie dedicată e-paper)

4. **Selectează placa** corespunzătoare (ex. *ESP32C6 DevKit*) și conectează placa prin USB.

5. **Încărcă (upload)** sketch-ul pe microcontroller.

6. **Monitorizează** comportamentul în *Serial Monitor* (viteză tipică: 115200 bps) pentru log-uri de debug.

---

## Randări și fișiere CAD

- Folderul `Images/` conține randări 3D (`render_device.png`, `render_pcb.png`) și diagrama bloc (`block_diagram.png`).  
- Fișierele de design (Eagle, KiCad sau Fusion 360 PCB) se află în folderul `hardware/`.  
- Gerbers și BOM detaliată pentru fabricație se găsesc în `hardware/output/`.  

---

## Observații de proiectare

- **Consum redus**: e-paper-ul menține imaginea fără consum suplimentar, deci microcontrollerul poate intra des în mod *sleep*.  
- **Protecția la încărcare**: modului de încărcare TP4056 i se pot adăuga circuite de protecție suplimentare (TVS, diode) pe portul USB-C.  
- **Dimensiuni și ergonomie**: poziționarea butoanelor de navigare la îndemână îmbunătățește experiența de lectură.  

---

## Licență și contribuții

Acest proiect este disponibil sub licența **MIT** (sau alta specificată în repository).  
Orice contribuții (pull request-uri, issue-uri) sunt încurajate, fie că este vorba de:
- Optimizări hardware  
- Îmbunătățiri de cod  
- Documentație suplimentară  
- Orice alt element care poate aduce valoare proiectului  

---

## Referințe

- [Documentația oficială ESP32-C6](#)  
- [TP4056 Battery Charger Datasheet](#)  

---

## Concluzie

Proiectul **TSC – E-Reader Hardware** oferă o platformă completă pentru a crea un dispozitiv de citire electronică cu autonomie ridicată și hardware flexibil.  
Fie că vrei să înveți, să experimentezi sau să creezi propriul tău eReader personalizat, acest repository îți pune la dispoziție:

- **Schemă și layout PCB**  
- **BOM detaliată**  
- **Cod sursă configurabil**  
- **Documentație pas cu pas**

Spor la **cafelutza**!
