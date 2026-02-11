<a name="english"></a>
# PathPulse Command Line Interface (CLI) Help

This document contains the output of the help commands for PathPulse v2.2.

---

## US English Help (`--help`)

```text
PathPulse v2.2 - Host Connectivity & RTT Monitor (by Kiss Előd)
Network diagnostic tool for path integrity verification, stability analysis
and LAN/ISP fault detection.

Usage:
  PathPulse --target <address> [options]

Required Parameters:
  --target <address>      Target Host: The hostname or IP address to monitor.
                          (e.g., 8.8.8.8)

Timing Options:
  --pingfreq <ms>         Interval between individual pings. (default: 1000)
  --pingtimeout <ms>      Wait time for each ping response in milliseconds.
                          (default: 50)
  --procfreq <ms>         Aggregation interval for statistics.
                          (default: 60000)
  --start <time>          Scheduled start time (format: HH:mm:ss).
                          If the time has passed, it schedules for tomorrow.
  --stop <value>          Stop after duration in minutes or hours.
                          (e.g., 42m, 3h)
  --timesync              Enable high-precision NTP time synchronization.
                          Requires internet connection. (Default: disabled)

Logging & Output:
  --logfolder <path>      Primary log directory. (e.g., Network storage)
                          (default: "_PathPulseLogs")
  --seclogfolder <path>   Secondary log directory. (e.g., Local storage)
                          Note: If specified, logs are always written to
                          both locations for redundancy.
  --logbase <name>        Base name or full template for the log file.
                          Supports dynamic tags.
                          (Default: "{dt}_PathPulseLogs_{cn}")
                          Dynamic Tags (Use within curly braces):
                          {clientname} or {cn}  - Initiating host
                                                      (local machine name)
                          {clientip}   or {ci}  - Local machine IP address
                          {publicip}   or {pi}  - Public IP address
                          {target}     or {t}   - Target host to pinged
                          {date}       or {d}   - Current date (yyyy-MM-dd)
                          {time}       or {h}   - Current time
                                                      (HH-mm-ss-fff)
                          {datetime}   or {dt}  - Full timestamp
                                                      (yyyy-MM-dd_HH-mm-ss-fff)
  --logprefix <string>    Optional prefix for the filename. Supports tags.
  --logpostfix <string>   Optional postfix for the filename. Supports tags.
  --logext <extension>    Log file extension. (Default: "txt")
  --events                Flag: Log connection drop/recovery events.
                          (default: disabled)
  --quietevents           Flag: Log connection drop/recovery events in quiet
                          mode (no console output).
                          (Default: disabled)
  --chart                 Flag: Log detailed statistics table (Sent, Received,
                          Loss%, and all Latency metrics) at the end of each
                          interval. (default: disabled)
  --acceptlate            Flag: Include "Late Arrival" RTTs in main
                          statistics. When set, late responses are treated
                          as "Success" and will NOT trigger
                          "Connection dropped" events.
                          (Default: disabled, late arrivals are failures)
  --async                 Flag: Enable asynchronous pinging.
                          When enabled, the program does not wait for a ping
                          to finish before starting the next cycle. This
                          allows --pingfreq to be set lower than
                          --pingtimeout.
                          When disabled, each ping must complete or timeout
                          before the next cycle begins. (Default: disabled)
  --help                  Show this help message.
  --helphu                Show this help message in Hungarian.

Features & Logic:
  * Dual Logging: The program alternates between the standard filename and
    a version with the "_cc" suffix every cycle to ensure data persistence.
  * Redundancy: Simultaneous logging to primary and secondary folders.
  * Comprehensive Statistics: Every "--procfreq" interval, the following are
    calculated:
    - Packet Counters: Sent, Received, Loss (%)
    - Latency Metrics: Min, Max, Mean, Median, P95 (95th percentile) in
      milliseconds
    - Stability Metrics: StdDev (Standard Deviation), Jitter in milliseconds
  * High-Res Monitoring: All RTT metrics are tracked with 0.1ms precision.
  * SLA (Service Level Agreement) Enforcement (--acceptlate):
    - Disabled (Default): Late arrivals are treated as failures. They trigger
      connection drop events and are excluded from main stats to maintain a
      strict performance baseline.
    - Enabled (Optional): When --acceptlate is used, late responses are
      treated as successes, keeping the connection state "Up" and including
      their values in statistics.
  * Frequency Distribution: Responses are grouped by RTT and sorted by
    frequency in descending order to help identify recurring latency patterns
    and bottlenecks.
  * Frequency Distribution: Late arrivals are binned into 1ms ranges and ranked
    by impact (frequency × latency). This highlights both recurring congestion
    patterns and high-latency anomalies, ensuring that severe spikes remain
    visible even if they occur infrequently.
  * Scheduled Operations: Support for precise --start and --stop timestamps.
    Automatic time synchronization at startup to ensure distributed monitoring
    nodes remain aligned.
  * Advanced Cancellation (4-Step Rule):
    Prevents accidental exit via a multi-press Ctrl+C logic.
    Steps: Request confirmation -> 2 more times -> 1 more -> Graceful Shutdown.
    A 5th press acts as a Force Quit (Emergency Override).
  * Smart Naming: Fully case-insensitive parameters and tags ({T}, {DT}),
    while preserving the original casing of your custom text.
  * Color-Coded Console: Real-time visual feedback using a color system for
    info (Blue), errors (Red) and events (DarkYellow).

File Naming Schema:
  - Odd cycles:  [logprefix][logbase][logpostfix].[logext]
  - Even cycles: [logprefix][logbase][logpostfix]_cc.[logext]

Notes on Case Sensitivity:
  - Parameters: Switches like --CHART, --chart, or --Chart are all accepted.
  - Tags:       Tokens like {DT}, {datetime}, or {DateTime} are
                case-insensitive.
  - Values:     Custom text in your template (e.g., folder names or file
                extensions) will preserve their original casing.
  - Examples:   --logbase "Ping_{TARGET}"  ->  "Ping_8.8.8.8.log"
                --LOGBASE "ping_{target}"  ->  "ping_8.8.8.8.log"

Example:
  PathPulse --target 8.8.8.8 --chart --events --logbase {clientname}_to_{target}_{datetime}


Press any key to exit...
```
<a name="hungarian"></a>
## HU Magyar Help (`--helphu`)

```text
PathPulse v2.2 - Host Connectivity & RTT Monitor (by Kiss Előd)
Hálózatdiagnosztikai eszköz útvonal-integritás igazolására,
stabilitás-elemzésre, valamint a LAN/ISP hibák behatárolására.

Használat:
  PathPulse --target <cím> [beállítások]

Kötelező paraméterek:
  --target <cím>           Célállomás: A figyelni kívánt gépnév vagy IP-cím.
                           (pl. 8.8.8.8)

Időzítési beállítások:
  --pingfreq <ms>          Egyedi pingek közötti időköz.
                           (alapértelmezett: 1000)
  --pingtimeout <ms>       Várakozási idő az egyes ping-válaszokra
                           ezredmásodpercben. (alapértelmezett: 50)
  --procfreq <ms>          Statisztikák összesítésének gyakorisága.
                           (alapértelmezett: 60000)
  --start <idő>            Ütemezett kezdés (formátum: HH:mm:ss).
                           Ha a megadott idő elmúlt, a program másnapra ütemez.
  --stop <érték>           Leállítás a megadott időtartam (perc vagy óra) után.
                           (pl. 42m, 3h)
  --timesync               Nagy pontosságú NTP időszinkronizáció bekapcsolása.
                           Internetkapcsolatot igényel.
                           (Alapértelmezés: kikapcsolva)

Naplózás és kimenet:
  --logfolder <útvonal>    Elsődleges naplózási könyvtár. (pl. hálózati tároló)
                           (alapértelmezett: "_PathPulseLogs")
  --seclogfolder <útvonal> Másodlagos naplózási könyvtár. (pl. helyi tároló)
                           Megjegyzés: Ha meg van adva, a naplók a
                           redundancia érdekében mindkét helyre mentésre
                           kerülnek.
  --logbase <név>          A naplófájl alapneve vagy teljes sablonja. Dinamikus
                           tagek használhatók.
                           (Alapértelmezett: "{dt}_PathPulseLogs_{cn}")
                           Dinamikus tagek (kapcsos zárójelek között):
                           {clientname} vagy {cn}  - Kezdeményező gép
                                                         (helyi gépnév)
                           {clientip}   vagy {ci}  - Helyi IP-cím
                           {publicip}   vagy {pi}  - Publikus IP-cím
                           {target}     vagy {t}   - A pingelt célállomás
                           {date}       vagy {d}   - Aktuális dátum
                                                         (yyyy-MM-dd)
                           {time}       vagy {h}   - Aktuális idő
                                                         (HH-mm-ss-fff)
                           {datetime}   vagy {dt}  - Teljes időbélyeg
                                                      (yyyy-MM-dd_HH-mm-ss-fff)
  --logprefix <szöveg>     Opcionális előtag a fájlnévhez. Tagek használhatók.
  --logpostfix <szöveg>    Opcionális utótag a fájlnévhez. Tagek használhatók.
  --logext <kiterjesztés>  Naplófájl kiterjesztése. (Alapértelmezett: "txt")
  --events                 Kapcsoló: Kapcsolati szakadás/helyreállás események
                           naplózása. (alapértelmezett: kikapcsolva)
  --quietevents            Kapcsoló: Kapcsolati szakadás/helyreállás események
                           naplózása csendes módban (konzol kimenet nélkül).
                           (Alapértelmezett: kikapcsolva)
  --chart                  Kapcsoló: Részletes statisztikai táblázat naplózása
                           (Sent, Received, Loss%, Latency mutatók) minden
                           intervallum végén. (alapértelmezett: kikapcsolva)
  --acceptlate             Kapcsoló: A "késve érkező" csomagok válaszidejének
                           (RTT) beszámítása a fő statisztikába.
                           Ha be van kapcsolva, a késve érkező válaszok
                           "Sikeresnek" minősülnek, és NEM váltanak ki
                           "Kapcsolat megszakadt" eseményt. (Alapértelmezett:
                           kikapcsolva, a későn érkezők hibának minősülnek)
  --async                  Kapcsoló: Aszinkron pingelés engedélyezése.
                           Ha be van kapcsolva, a program nem vár az előző ping
                           befejezésére a következő ciklus indítása előtt.
                           Ez lehetővé teszi a --pingfreq --pingtimeout
                           értékénél kisebbre állítását.
                           Ha ki van kapcsolva, minden pingnek be kell
                           fejeződnie vagy időtúllépésre kell futnia a
                           következő ciklus előtt.
                           (Alapértelmezett: kikapcsolva)
  --help                   Súgó megjelenítése.
  --helphu                 Súgó megjelenítése Magyar nyelven.

Funkciók és logika:
  * Dupla naplózás: A program minden ciklusban váltogat az alapértelmezett
    fájlnév és egy "_cc" utótaggal ellátott verzió között az adatbiztonság
    érdekében.
  * Redundancia: Szimultán naplózás az elsődleges és másodlagos mappákba.
  * Átfogó statisztika: Minden "--procfreq" intervallumban a következők
    kerülnek kiszámításra:
    - Csomagszámlálók: Küldött, Fogadott, Veszteség (%)
    - Válaszidő mutatók: Min, Max, Átlag, Medián, P95 (95. percentilis)
      milliszekundumban
    - Stabilitási mutatók: Szórás, Ingadozás (Jitter) milliszekundumban
  * Nagy pontosságú időmérés: Minden RTT mutató 0,1 ms pontossággal követve.
  * SLA (Service Level Agreement) betartatás (--acceptlate):
    - Kikapcsolva (Alapértelmezett): A késve érkező válaszok hibának
      számítanak, eseményt generálnak és kimaradnak a fő statisztikából a
      mérés tisztaságának megőrzése érdekében.
    - Bekapcsolva: A későn érkező válaszok sikeresek, a kapcsolat állapota
      "folytonos" marad, és az értékek bekerülnek a statisztikába.
  * Gyakorisági eloszlás: A késve érkező válaszok RTT szerint csoportosítva
    és gyakoriság szerint csökkenő sorrendben jelennek meg a naplóban, a
    visszatérő késleltetési minták és anomáliák azonosításához.
  * Gyakorisági eloszlás: A késve érkező válaszokat a program 1 ms-os sávokba
    csoportosítja és a hálózatra gyakorolt hatásuk (gyakoriság × késleltetés)
    szerint rangsorolja.
    Ez a módszer kiemeli a rendszeres lassulásokat (minták), és biztosítja,
    hogy a súlyos kiugrások (anomáliák) akkor is láthatóak maradjanak, ha
    ritkán fordulnak elő.
  * Ütemezett műveletek: Támogatás a pontos --start és --stop időbélyegekhez.
    Automatikus időszinkronizáció indításkor a megosztott figyelési pontok
    összehangolásához.
  * Fejlett megszakítási logika (4 lépéses szabály): Véletlen kilépés
    megakadályozása többszörös Ctrl+C lenyomással.
    Lépések: Megerősítés kérése -> még 2x -> még 1x -> Szabályos leállás.
    Az 5. lenyomás erőltetett kilépésként (vészhelyzeti felülbírálás) működik.
  * Intelligens névkezelés: A paraméterek és tagek nem érzékenyek a kis- és
    nagybetűkre, miközben az egyéni szövegek eredeti formátuma megmarad.
  * Színkódolt konzol: Valós idejű vizuális visszajelzés információk (kék),
    hibák (piros) és események (sárga) színkódolt megjelenítésével.

Fájlnév-séma:
  - Páratlan ciklusok: [logprefix][logbase][logpostfix].[logext]
  - Páros ciklusok:    [logprefix][logbase][logpostfix]_cc.[logext]

Megjegyzések a kis- és nagybetűkről:
  - Paraméterek: A --CHART, --chart, vagy --Chart kapcsolók mind elfogadottak.
  - Tagek:       A {DT}, {datetime}, vagy {DateTime} tagek szintén
                 érzéketlenek a kis- és nagybetűkre.
  - Értékek:     A sablonban megadott egyedi szövegek (pl. mappanevek,
                 célállomás neve) megőrzik az eredeti formájukat.
  - Példák:      --logbase "Ping_{TARGET}"  ->  "Ping_8.8.8.8.log"
                 --LOGBASE "ping_{target}"  ->  "ping_8.8.8.8.log"

Példa:
  PathPulse --target 8.8.8.8 --chart --events --logbase {clientname}_to_{target}_{datetime}


Press any key to exit...
