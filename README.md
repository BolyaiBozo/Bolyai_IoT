# Bolyai IoT Modul 🎛️

A **Bolyai IoT** egy robusztus, sokoldalú mikrokontrolleres oktató- és beavatkozó modul, amely a **BMSzC Bolyai János Műszaki Technikum és Kollégium** *Ipari Informatikai Technikus* (szakmai azonosító szám: `507140405`) képzéséhez és IoT tematikájú gyakorlati oktatáshoz készült[cite: 9]. Tulajdonságai révén kiválóan alkalmas otthonautomatizálási rendszerek, távoli beavatkozók és SCADA laborkörnyezetek építéséhez[cite: 9].

---

## 📋 Főbb jellemzők

* **Mikrokontroller:** Microchip PIC18F1320 kristály-oszcillátoros időzítéssel a stabil baud-ráta biztosításához[cite: 9].
* **Soros kommunikáció:** Szabványos UART interfész **115 200 baud** sebességgel (8 adatbit, 1 stopbit, paritás nélkül), 3,3 V-os TTL jelszintekkel (közvetlen Raspberry Pi és ESP kompatibilitás)[cite: 9, 10].
* **Analóg bemenet:** 10 bites felbontású ADC (0–1023), beépített osztóhellyel és integráló kondenzátorral (egyedi feszültségméréshez, pl. 5 V LiPo vagy 15 V ólomakkumulátor)[cite: 9, 10].
* **Hőmérsékletmérés:** Integrált digitális hőmérő ($-10\,{^\circ}\text{C}$ – $+85\,{^\circ}\text{C}$)[cite: 9].
* **Bemenet:** Fedélzeti nyomógomb vagy opcionális IR vevőegység csatlakozási lehetőség[cite: 9, 10].
* **Kimenet:** Optocsatolóval galvanikusan leválasztott beavatkozó: LED vagy nagyáramú relé ($250\,\text{V AC} \;@\; 8\,\text{A}$, NO/NC kivezetésekkel)[cite: 9, 10].
* **Címezhető LED kimenet:** Dedikált WS2801 PWM LED / LED-füzér vezérlő csatlakozó[cite: 9, 11].
* **Vezeték nélküli bővíthetőség:** Közvetlen foglalat ESP-01 (ESP8266) Wi-Fi vagy soros Bluetooth modul számára[cite: 9, 11].
* **Tápellátási rugalmasság:** Széles tápfeszültség-tartomány ($5-15\,\text{V}$ sorkapcson / tüskesoron keresztül, vagy szabványos USB-C csatlakozóval, PTC biztosítékkal védve)[cite: 9].
* **Oktatási szimulációs módok:** 5 különböző beépített szoftveres kommunikációs hibaállapot (táviratvesztés, késleltetés, duplikáció) a diagnosztikai és hibatűrő SCADA folyamatok teszteléséhez[cite: 9, 12].

---

## ⚙️ Műszaki paraméterek

| Paraméter | Érték |
| :--- | :--- |
| **Tápfeszültség ($U_{\text{táp}}$)** | $5-15\,\text{V}$ (tipikus: $5\,\text{V}$)[cite: 9] |
| **Áramfelvétel** | min. $1,3\,\text{mA}$ (@ 5 V), max. $60\,\text{mA}$ (@ 5 V)[cite: 9] |
| **Relé kimenet** | $250\,\text{V AC} \;@\; 8\,\text{A}$ (NO, NC, COM)[cite: 9, 10] |
| **Méret** | $64\,\text{mm} \times 38\,\text{mm}$[cite: 9] |
| **Soros interfész** | $115\,200\,\text{baud}$, 8N1, TTL szint[cite: 9, 10] |

---

## 🔌 Csatlakozók és lábkiosztások

### 1. Soros interfész (`J Soros`)
* `1. TxD` – Adat kimenet (modul küld)[cite: 10]
* `2. RxD` – Adat bemenet (modul fogad)[cite: 10]
* `3. GND` – Közös föld[cite: 10]

### 2. Tápcsatlakozás (`JUSB-C` / Tüskesor)
* `1. GND` – Tápföld[cite: 9]
* `2. V+` – Tápfeszültség (+5 V vagy 5–15 V stabilizátorral)[cite: 9]

### 3. Relé csatlakozó (`J2` - sorkapocs)
* `1. C` – Közös érintkező (Common)[cite: 10]
* `2. NC` – Nyugalomban zárt (Normally Closed)[cite: 10]
* `3. NO` – Nyugalomban nyitott (Normally Open)[cite: 10]

### 4. Analóg bemenet (`JAN`)
* `1. AN` – Analóg bemenet (0–1023 érték)[cite: 10]
* `2. GND` – Föld referencia[cite: 10]

### 5. Wi-Fi / Bluetooth aljzat (`J3` - ESP-01)
`1: RxD` | `2: GND` | `3: EN` | `4: GPIO2` | `5: RST` | `6: GPIO0` | `7: V+ (3,3V)` | `8: TxD`[cite: 11]

### 6. Címezhető LED csatlakozó (`LEDPWM` - WS2801)
`1: V+ (5V)` | `2: CKI (Clock)` | `3: SDI (Data)` | `4: GND`[cite: 11]

---

## 💬 Kommunikációs protokoll

Minden bejövő utasítást a `#` karakter vezet be, és `\r`, `\n` vagy `\r\n` zár[cite: 11]. Az adatok hexadecimális ASCII karakterek[cite: 11].

### Parancskészlet

| Parancs | Leírás | Példa küldés | Válasz / Eredmény |
| :--- | :--- | :--- | :--- |
| `?` | Aktuális adatok azonnali lekérése[cite: 11] | `#?\n`[cite: 11] | `#ID01234567&No3D&+85.000C&03FF&SW1;`[cite: 11] |
| `v` | Firmware verziószám lekérdezése | `#v\n` | `Bolyai IoT v1.1a` |
| `$` | Egyedi azonosító (ID) átírása (8 karakter) | `#$00000001\n`[cite: 12] | Visszaadja az új azonosítójú telegramot[cite: 12] |
| `O` | Kimeneti relé / LED állapot vagy PWM állítása[cite: 12] | `#O01\n` (bekapcsolás)<br>`#O00\n` (kikapcsolás)[cite: 12] | Relé meghúz vagy leold (nincs szöveges válasz)[cite: 12] |
| `L` | WS2801 RGB LED füzér színének beállítása (`RRGGBB`)[cite: 12] | `#LFF0000\n` (Piros szín)[cite: 12] | LED szalag azonnal színt vált[cite: 12] |
| `E` | Oktatási teszthiba előidézése[cite: 12] | `#E01\n`[cite: 12] | Lásd az alábbi hibamódokat[cite: 12] |

#### Szimulált kommunikációs hibák (`E` parancs)[cite: 12]:
* `00`: Normál üzemmód visszaállítása[cite: 12]
* `01`: Hiányzó táviratok (missing telegrams)[cite: 12]
* `02`: Megnövelt táviratküldési időköz (15,3 s)[cite: 12]
* `03`: Ismételt / azonos sorszámú táviratok (duplicated)[cite: 12]
* `04`: Véletlenszerű sorszámok vagy kimaradó csomagok[cite: 12]
* `05`: Extrém hosszú időköz a táviratok között (32,5 s)[cite: 12]

---

### Periodikus kimeneti adatfolyam (Telegram)

A modul alapértelmezetten automatikusan, kb. **1,92 másodpercenként** küldi ki a telemetriai csomagot[cite: 12]:

```text
#ID01234567&No3D&+85.000C&03FF&SW1;
