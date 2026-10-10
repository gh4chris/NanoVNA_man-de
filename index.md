1. toc
{:toc}

# Einleitung

Dieses Dokument ist ein inoffizieller Benutzerleitfaden für das NanoVNA. Die URL lautet: https://github.com/gh4chris/NanoVNA_man-de/

Es wird im [GitHub-Repository](https://github.com/gh4chris/NanoVNA_man-de) verwaltet.

Falls Korrekturen erforderlich sind, z. B. wegen Abweichungen von der neuesten Firmware, senden Sie bitte einen Pull-Request.

Das Dokument steht außerdem im PDF-Format auf der GitHub-Releases-Seite zur Verfügung.

  * https://github.com/gh4chris/NanoVNA_man-de/releases

## Was ist das NanoVNA?

Es gibt mehrere Varianten der NanoVNA-Hardware. Dieses Dokument bezieht sich auf die folgenden Varianten:

- ttrftech-Version (Original) [ttrftech/NanoVNA](https://github.com/ttrftech/NanoVNA)
- hugen79-Version [hugen79/NanoVNA-H](https://github.com/hugen79/NanoVNA-H)

Diese Hardware-Varianten haben nahezu identische Schaltungskomponenten und können dieselbe Firmware verwenden.

## Für den Betrieb erforderliche Ausrüstung

Mindestens wird Folgendes benötigt:

 * NanoVNA-Gerät
 * SMA LOAD 50 Ω
 * SMA SHORT
 * SMA OPEN
 * SMA-Female-to-Female-Durchgangskonnektor (Thru)
 * SMA-Male-to-Male-Kabel × 2

## Grundlagen des NanoVNA

Ein VNA (Vector Network Analyzer) misst die Frequenzabhängigkeit der reflektierten und der durchgelassenen Leistung eines Hochfrequenznetzwerks (RF Network).

Das NanoVNA misst die folgenden Größen:

 * I/Q-Signal der Eingangsspannung
 * I/Q-Signal der reflektierten Spannung
 * I/Q-Signal der durchgelassenen Spannung

Daraus werden folgende Größen berechnet:

 * Reflexionsfaktor (reflection coefficient) S11
 * Übertragungsfaktor (transmission coefficient) S21

Die folgenden daraus berechenbaren Größen können angezeigt werden:

 * Rückflussdämpfung (Reflexionsverlust)
 * Einfügedämpfung (Durchgangsverlust)
 * Komplexe Impedanz
   * Wirkwiderstand (Resistanz)
   * Blindwiderstand (Reaktanz)
 * SWR

usw.

## Oszillationsfrequenzen des NanoVNA

Das NanoVNA misst den Reflexions- und Übertragungsfaktor an 101 Punkten des zu messenden Frequenzbands.

Die lokale Oszillatorfrequenz des NanoVNA liegt zwischen 50 kHz und 300 MHz. Höhere Frequenzen werden im Harmonischen-Modus (Oberwellen-Modus) gemessen. Auch in diesem Modus wird die Grundwelle nicht gedämpft.
Die je nach Frequenz verwendeten Modi sind:

* bis 300 MHz: Grundwelle
* 300 MHz bis 900 MHz: 3. Harmonische
* 900 MHz bis 1500 MHz: 5. Harmonische

Insbesondere bei der Überprüfung der Verstärkung eines Verstärkers ist zu beachten, dass stets auch die Grundwelle am Eingang anliegt.

Der Eingang wird in allen Fällen auf eine Zwischenfrequenz von 5 kHz umgesetzt. Das Signal wird mit 48 kHz Abtastrate analog-digital gewandelt. Die digitalen Daten werden im Mikrocontroller (MCU) signalverarbeitet.

# Erste Schritte

Bevor Sie das Gerät verwenden, müssen Sie unbedingt zunächst eine Kalibrierung durchführen. Kalibrieren Sie zum ersten Mal wie folgt:

 * Prüfen, dass START 50 kHz beträgt
 * Prüfen, dass STOP 900 MHz beträgt
 * Kalibrierung gemäß <a href="#kalibrierungsverfahren">Kalibrierungsverfahren</a> durchführen

# Eingabemöglichkeiten

Das NanoVNA verfügt über folgende Eingaben:

 * Tippen bzw. langes Tippen auf das Touchpanel
 * Hebelschalter (Joystick)
   * L / langes L
   * R / langes R
   * Drücken / langes Drücken
 * Ein/Aus-Schiebeschalter

# Die Bildschirmanzeige

## Hauptbildschirm

<img class="" src="./images/nanovna-display-infor_0000_start-stop.png"/>

### 1. START / 2. STOP-Frequenz
Zeigt die jeweilige Frequenz an, die als Start bzw. Stopp festgelegt wurde.

### 3. Marker
Zeigt die Position der Marker jedes Traces an. Der ausgewählte Marker kann auf folgende Weise bewegt werden:

 * Marker auf dem Touchpanel ziehen (Drag)
 * L bzw. R des Hebelschalters gedrückt halten

### 4. Kalibrierstatus

Zeigt die Nummer der geladenen Kalibrierdaten sowie die angewendeten Fehlerkorrekturen an.

 * `C0` `C1` `C2` `C3` `C4`: Zeigt an, dass die Kalibrierdaten der entsprechenden Nummer geladen sind.
 * `c0` `c1` `c2` `c3` `c4`: Zeigt an, dass die Kalibrierdaten der entsprechenden Nummer geladen sind, aber nach dem Laden der Frequenzbereich geändert wurde und für die Fehlerkorrektur Interpolation verwendet wird.
 * `D`: Zeigt an, dass die Fehlerkorrektur „directivity" (Richtwirkung) angewendet wird
 * `R`: Zeigt an, dass die Fehlerkorrektur „reflection tracking" angewendet wird
 * `S`: Zeigt an, dass die Fehlerkorrektur „source match" angewendet wird
 * `T`: Zeigt an, dass die Fehlerkorrektur „transmission tracking" angewendet wird
 * `X`: Zeigt an, dass die Fehlerkorrektur „isolation" (Übersprechen) angewendet wird

### 5. Referenzposition

Zeigt die Referenzposition des jeweiligen Traces an. Die Position kann unter `DISPLAY` `SCALE` `REFERENCE POSITION` geändert werden.

### 6. Markerstatus

Es werden der aktuell aktive Marker sowie der zuvor aktive Marker (je einer) angezeigt.

### 7. Trace-Status

Zeigt für jeden Trace den Zustand des jeweiligen Formats sowie die zum aktiven Marker gehörenden Werte an.

Die Anzeige `CH0 LOGMAG 10dB/ 0.02dB` liest sich z. B. wie folgt:

 * Kanal CH0 (Reflexion)
 * Format `LOGMAG`
 * Skalierung 10 dB
 * aktueller Wert 0.02 dB

Beim aktiven Trace wird die Kanalanzeige invertiert.

### 8. Batteriestatus

Wenn ein Akku eingesetzt ist und auf der Platine `D2` bestückt ist, wird je nach Akkuspannung ein Symbol angezeigt.

---

## Hauptbildschirm 2

<img class="" src="./images/nanovna-display-infor_0001_center-span.png"/>

### 9. CENTER-Frequenz / 10. Spannweite (Span)

Zeigt die jeweilige Frequenz an, wenn Mittenfrequenz und Spannweite festgelegt wurden.

---

### Menübildschirm

<img class="" src="./images/nanovna-display-infor_0002_menu.png"/>

### 11. Menü

Das Menü kann wie folgt aufgerufen werden:

 * Tippen auf eine Stelle des Touchpanels, die kein Marker ist
 * Drücken des Hebelschalters

---

## Tastenfeld-Bildschirm

<img class="" src="./images/nanovna-display-infor_0003_keypad.png"/>

### 12. Zifferntasten

Durch Antippen einer Ziffer wird ein Zeichen eingegeben.

### 13. Löschtaste (Back)

Löscht ein Zeichen. Wurde noch kein Zeichen eingegeben, wird die Eingabe abgebrochen und zum vorherigen Zustand zurückgekehrt.

### 14. Einheitentasten

Multipliziert die aktuelle Eingabe mit der zugehörigen Einheit und beendet die Eingabe sofort. Bei ×1 wird der eingegebene Zahlenwert unverändert übernommen.

### 15. Eingabefeld

Zeigt den Namen des einzustellenden Parameters sowie die eingegebenen Ziffern an.

# Messung starten

## Grundlegender Messablauf

 1. Frequenzbereich festlegen
 2. Kalibrieren
 3. DUT (Prüfling) anschließen

# Kalibrierungsverfahren

<img class="capture" src="./images/cal.png">

Die Kalibrierung muss grundsätzlich bei jeder Änderung des zu messenden Frequenzbereichs erneut durchgeführt werden. Liegt die Fehlerkorrektur korrekt vor, zeigt der Bildschirm als Kalibrierstatus `Cn D R S T X` an. Dabei ist n die Nummer der geladenen Daten.

Das NanoVNA kann jedoch vorhandene Kalibrierinformationen interpolieren und dadurch bis zu einem gewissen Grad korrekte Anzeigen liefern. Wird nach dem Laden von Kalibrierdaten der Frequenzbereich geändert, tritt dieser Zustand ein. In diesem Fall zeigt der Kalibrierstatus auf dem Bildschirm `cn D R S T X`. Dabei ist n die Nummer der geladenen Daten.

 1. Aktuellen Kalibrierstatus zurücksetzen: `CAL` `RESET`
 2. OPEN-Standard an den CH0-Anschluss anschließen und `CAL` `CALIBRATE` `OPEN` ausführen.
 3. SHORT-Standard an den CH0-Anschluss anschließen und `CAL` `CALIBRATE` `SHORT` ausführen.
 4. LOAD-Standard an den CH0-Anschluss anschließen und `CAL` `CALIBRATE` `LOAD` ausführen.
 5. LOAD-Standard an die Anschlüsse CH0 und CH1 anschließen und `CAL` `CALIBRATE` `ISOLN` ausführen. Wenn nur ein LOAD vorhanden ist, darf der CH0-Anschluss offen bleiben.
 6. Kabel an die Anschlüsse CH0 und CH1 anschließen, die Kabelenden mit dem Durchgangskonnektor (Thru) verbinden und `CAL` `CALIBRATE` `THRU` ausführen.
 7. Kalibrierung abschließen und Fehlerkorrekturinformationen berechnen: `CAL` `CALIBRATE` `DONE`
 8. Unter Angabe der Datennummer speichern: `CAL` `CALIBRATE` `SAVE` `SAVE 0`

Hinweis:
Die Aufnahme der einzelnen Kalibrierdaten muss erfolgen, nachdem die Anzeige ausreichend stabilisiert ist.

# Funktionen

## Trace-Anzeige

<img class="capture" src="./images/traces.png"/>

Es können bis zu 4 Traces (Messkurven) angezeigt werden, von denen einer der aktive Trace ist.

Traces können selektiv eingeblendet werden. Zum Umschalten `DISPLAY` `TRACE` `TRACE n` wählen.

Es gibt folgende Möglichkeiten, den aktiven Trace zu wechseln:

 * Auf den Marker des Traces tippen, der aktiviert werden soll
 * `DISPLAY` `TRACE` `TRACE n` wählen, um ihn einzublenden. (Falls er bereits angezeigt wird, muss er zuvor kurz ausgeblendet werden.)

### Trace-Formate

Jeder Trace kann ein eigenes Format haben. Um das Format des aktiven Traces zu ändern: `DISPLAY` `FORMAT` wählen und das gewünschte Format auswählen.

Die einzelnen Formate zeigen Folgendes:

 * `LOGMAG`: Logarithmus des Betrags des Messwerts
 * `PHASE`: Phase im Bereich −180° bis +180°
 * `DELAY`: Laufzeit (Verzögerung)
 * `SMITH`: Smith-Diagramm
 * `SWR`: Stehwellenverhältnis (Standing Wave Ratio)
 * `POLAR`: Polarkoordinatendarstellung
 * `LINEAR`: Betrag des Messwerts
 * `REAL`: Realteil des Messwerts
 * `IMAG`: Imaginärteil des Messwerts
 * `RESISTANCE`: Wirkwiderstandsanteil (Resistanz) der Impedanz des Messwerts
 * `REACTANCE`: Blindwiderstandsanteil (Reaktanz) der Impedanz des Messwerts

### Trace-Kanäle

Das NanoVNA verfügt über zwei Anschlüsse: `CH0` und `CH1`. An diesen Anschlüssen können folgende S-Parameter gemessen werden:

 * `CH0` S11 (Rückflussdämpfung)
 * `CH1` S21 (Einfügedämpfung)

Um den Kanal eines Traces zu ändern, unter `DISPLAY` `CHANNEL` entweder `CH0 REFLECT` oder `CH1 THROUGH` wählen.

## Marker

<img class="capture" src="./images/marker.png">
<img class="capture" src="./images/marker-select.png">

Bis zu 4 Marker können angezeigt werden. Das Anzeigen der Marker erfolgt über `MARKER` `SELECT MARKER` `MARKER n`. Beim Anzeigen eines Markers wird der aktive Marker auf den neu angezeigten Marker gesetzt.

## Zeitbereichs-Operationen (Time Domain)

Das NanoVNA kann durch Signalverarbeitung der Frequenzbereichsdaten eine Zeitbereichsmessung simulieren.

Um die Messdaten in den Zeitbereich umzuwandeln, `DISPLAY` `TRANSFORM` `TRANSFORM ON` wählen. Ist `TRANSFORM ON` aktiv, werden die Messdaten sofort in den Zeitbereich umgerechnet und angezeigt.

Zwischen Zeit- und Frequenzbereich bestehen folgende Zusammenhänge:

 * Je höher die maximale Frequenz, desto besser die Zeitauflösung
 * Je enger der Abstand der Messfrequenzen (d. h. je niedriger die maximale Frequenz), desto länger die maximal darstellbare Zeitdauer

Maximale Zeitdauer und Zeitauflösung stehen daher in einem Trade-off-Verhältnis.

Ersetzt man die Zeitdauer durch eine Distanz, gilt Folgendes:

 * Um die maximale Messdistanz zu vergrößern, muss die maximale Frequenz verringert werden.
 * Um die Distanz präzise zu bestimmen, muss die maximale Frequenz erhöht werden.

### Zeitbereich: Bandpass

Im Bandpass-Modus kann die Antwort des DUT (Prüflings) auf ein Impulssignal simuliert werden.

Das Trace-Format kann auf `LINEAR`, `LOGMAG` oder `SWR` eingestellt werden.

Nachfolgend ein Beispiel für die Impulsantwort eines Bandpassfilters:

<img class="capture" src="images/tdr-bandpass-bandpass.png">

### Zeitbereich: Tiefpass – Impuls

Im Tiefpass-Modus kann ein TDR (Zeitbereichsreflektometer) simuliert werden. Im Tiefpass-Modus muss die Startfrequenz 50 kHz betragen; die Stoppfrequenz ist entsprechend der zu messenden Distanz einzustellen.

Das Trace-Format kann auf `REAL` eingestellt werden.

Nachfolgend Beispiele für die Sprungantwort bei offenem Ende und die Impulsantwort bei kurzgeschlossenem Ende:

<img class="capture" src="images/impulse-open.png">
<img class="capture" src="images/impulse-short.png">

### Zeitbereich: Tiefpass – Sprung (Step)

Im Tiefpass-Modus kann ein TDR simuliert werden. Im Tiefpass-Modus muss die Startfrequenz 50 kHz betragen; die Stoppfrequenz ist entsprechend der zu messenden Distanz einzustellen.

Das Trace-Format kann auf `REAL` eingestellt werden.


Offen:<br>
<img class="capture" src="images/step-open.png">

Kurzschluss:<br>
<img class="capture" src="images/step-short.png">

#### Beispiele für Sprungantworten

Kapazitiver Kurzschluss:<br>
<img class="capture" src="images/tdr-capacitive-termination.png">

Induktiver Kurzschluss:<br>
<img class="capture" src="images/tdr-inductive-termination.png">

Kapazitive Diskontinuität (paralleles C):<br>
<img class="capture" src="images/tdr-c-shunt-discontinuity.png">

Induktive Diskontinuität (serielles L):<br>
<img class="capture" src="images/tdr-l-series-discontinuity.png">

### Zeitbereich: Fensterfunktion

Der messbare Bereich ist endlich; es gibt eine niedrigste und eine maximale Frequenz. Mit der Fensterfunktion lassen sich diese unstetigen Messdaten glätten und Nachschwingen (Ringing) unterdrücken.

Es gibt drei Stufen:

 * MINIMUM (kein Fenster, entspricht dem Rechteckfenster)
 * NORMAL (entspricht dem Kaiser-Fenster mit β=6)
 * MAXIMUM (entspricht dem Kaiser-Fenster mit β=13)

Bei MINIMUM ist die Auflösung maximal, bei MAXIMUM der Dynamikbereich maximal. NORMAL liegt dazwischen.

### Einstellung des Verkürzungsfaktors (Velocity Factor) im Zeitbereich

Die Ausbreitungsgeschwindigkeit elektromagnetischer Wellen im Kabel hängt vom Material ab. Das Verhältnis dieser Geschwindigkeit zur Ausbreitungsgeschwindigkeit im Vakuum wird als Verkürzungsfaktor (Velocity Factor, Velocity of Propagation) bezeichnet. Er ist immer in den Spezifikationen des Kabels angegeben.

Im Zeitbereich kann die angezeigte Zeit auch als Distanz dargestellt werden.
Der für die Distanzanzeige verwendete Verkürzungsfaktor kann unter `DISPLAY` `TRANSFORM` `VELOCITY FACTOR` eingestellt werden. Für die TDR-Messung eines Kabels mit einem Verkürzungsfaktor von 67 % gibt man z. B. bei `VELOCITY FACTOR` `67` ein.

### Frequenz aus dem Marker übernehmen

Der Frequenzbereich kann wie folgt aus einem Marker festgelegt werden:

 * `MARKER` `→START`: setzt die Frequenz des aktiven Markers als Startfrequenz
 * `MARKER` `→STOP`: setzt die Frequenz des aktiven Markers als Stoppfrequenz
 * `MARKER` `→CENTER`: setzt die Frequenz des aktiven Markers als Mittenfrequenz. Die Spannweite wird so angepasst, dass der aktuelle Bereich möglichst erhalten bleibt.
 * `MARKER` `→SPAN`: setzt die beiden angezeigten Marker (einschließlich des aktiven Markers) als Spannweite. Ist nur ein Marker sichtbar, geschieht nichts.

## Einstellen des Messbereichs

<img class="capture" src="./images/stimulus.png">

Es gibt drei Arten, den Messbereich einzustellen:

 * Start- und Stoppfrequenz festlegen
 * Mittenfrequenz und Spannweite festlegen
 * Zero-Span

### Start- und Stoppfrequenz festlegen

Dazu jeweils `STIMULUS` `START` bzw. `STIMULUS` `STOP` wählen und einstellen.

### Mittenfrequenz und Spannweite festlegen

Dazu jeweils `STIMULUS` `CENTER` bzw. `STIMULUS` `SPAN` wählen und einstellen.

### Zero-Span

Zero-Span ist ein Modus ohne Frequenzsweep, bei dem eine einzelne Frequenz kontinuierlich ausgegeben wird.

Zur Einstellung `STIMULUS` `CW FREQ` wählen.

### Messung vorübergehend anhalten

Mit `STIMULUS` `PAUSE SWEEP` wird die Messung vorübergehend angehalten.


## Aufrufen von Kalibrierung und Einstellungen

<img class="capture" src="./images/save.png">

Es können bis zu 5 Kalibrierdatensätze gespeichert werden. Direkt nach dem Einschalten lädt das NanoVNA den Datensatz 0.

Die Kalibrierdaten enthalten folgende Informationen:

 * Eingestellter Frequenzbereich
 * Fehlerkorrektur an jedem Messpunkt
 * Trace-Einstellungen
 * Marker-Einstellungen
 * Einstellung des Domain-Modus (Frequenz-/Zeitbereich)
 * Einstellung des Verkürzungsfaktors
 * Electrical Delay (elektrische Verzögerung)

Mit `CAL` `SAVE` `SAVE n` lassen sich die aktuellen Einstellungen speichern.

Mit `CAL` `RESET` lassen sich die aktuellen Kalibrierdaten zurücksetzen. Für eine erneute Kalibrierung muss `RESET` durchgeführt werden.

`CAL` `CORRECTION` zeigt an, ob derzeit eine Fehlerkorrektur aktiv ist. Durch Auswählen lässt sich die Fehlerkorrektur vorübergehend deaktivieren.

<img class="capture" src="./images/recall.png">

Mit `RECALL` `RECALL n` können gespeicherte Einstellungen aufgerufen werden.

## Geräteeinstellungen

<img class="capture" src="./images/config.png">

Unter `CONFIG` lassen sich allgemeine Geräteeinstellungen vornehmen.

### Kalibrierung und Test des Touchpanels

<img class="capture" src="./images/touchcal.png">
<img class="capture" src="./images/touchcal2.png">

Mit `CONFIG` `TOUCH CAL` kann das Touchpanel kalibriert werden. Bestehen große Abweichungen zwischen der tatsächlichen und der erkannten Tipp-Position, lässt sich das hiermit beheben. Führen Sie nach `TOUCH CAL` einen `TOUCH TEST` durch, um zu prüfen, ob alles korrekt eingestellt ist, und speichern Sie die Einstellungen mit `SAVE`.

<img class="capture" src="./images/touchtest.png">

Mit `CONFIG` `TOUCH TEST` lässt sich das Touchpanel testen. Während das Touchpanel berührt wird, wird eine Linie gezeichnet; beim Loslassen kehrt die Anzeige in den Ausgangszustand zurück.

### Speichern der Geräteeinstellungen

Mit `CONFIG` `SAVE` lassen sich die allgemeinen Geräteeinstellungen speichern. Diese enthalten folgende Informationen:

 * Kalibrierdaten des Touchpanels
 * Gitterfarbe
 * Trace-Farben
 * Standardmäßig geladene Kalibrierdaten-Nummer

 Außer den Touchpanel-Kalibrierdaten gibt es derzeit keine Möglichkeit, diese einzustellen.

### Version anzeigen

<img class="capture" src="./images/version.png">

Mit `CONFIG` `VERSION` lassen sich die Versionsinformationen des Geräts anzeigen.

### Firmware-Update

<img class="capture" src="./images/dfu.png">

Mit `CONFIG` `→DFU` `RESET AND ENTER DFU` wird das Gerät zurückgesetzt und in den DFU-Modus (Device Firmware Update) versetzt. In diesem Modus kann die Firmware über USB aktualisiert werden.

# Firmware aktualisieren

## Firmware beziehen

### ttrftech-Firmware

Dies ist die Original-Firmware. Sie ist versioniert und wird aktiv weiterentwickelt.

 * [GitHub releases](https://github.com/ttrftech/NanoVNA/releases)
 * [CircleCI-Build](https://cho45.stfuawsc.com/NanoVNA/dfu.html)

 Auf GitHub releases finden sich einigermaßen stabile Release-Versionen der Firmware.

 Auf CircleCI gibt es die Firmware zu jedem Commit. Wer die neuesten Funktionen ausprobieren oder Fehler überprüfen möchte, verwendet diese.

### hugen79-Firmware

 * [Google Drive](https://drive.google.com/drive/folders/1IRz6E1wlkRyV0u7sbqj0lhWST-GV1szY)
 
Die neueste Firmware befindet sich auf Google Drive.

### Selbst bauen

Man kann das GitHub-Repository auch klonen und die Firmware selbst bauen – das ist problemlos möglich.


## Firmware flashen

Es gibt verschiedene Möglichkeiten zu flashen; hier wird [dfu-util](http://dfu-util.sourceforge.net/) verwendet.
dfu-util ist ein plattformübergreifendes Werkzeug; für Windows werden Binärdateien angeboten, sodass es direkt nach dem Herunterladen einsatzbereit ist.

### Flashen mit dfu-util (Ubuntu)

dfu-util ist im Standard-Paketrepository enthalten.

```
sudo apt-get install dfu-util
dfu-util --version
```

Starten Sie das Gerät im DFU-Modus. Dies gelingt auf eine der folgenden Arten:

 * Beim Einschalten den BOOT0-Pin auf der Platine überbrücken (nach dem Einschalten die Überbrückung wieder entfernen). Der Bildschirm bleibt danach weiß – das ist normal.
 * `CONFIG` `→DFU` `RESET AND ENTER DFU` auswählen


Führen Sie den folgenden Befehl aus. Ersetzen Sie build/ch.bin durch den Pfad zur .bin-Datei der heruntergeladenen Firmware.

```
dfu-util -d 0483:df11 -a 0 -s 0x08000000:leave -D build/ch.bin
```

### Flashen mit dfu-util (macOS)

Die Installation über [homebrew](https://brew.sh/index_de.html) wird empfohlen.

Zuerst den brew-Befehl (Homebrew) installieren:

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Dann dfu-util installieren:

```
brew install dfu-util
```

Prüfen, ob dfu-util korrekt funktioniert:

```
dfu-util --version
```

Starten Sie das Gerät im DFU-Modus. Dies gelingt auf eine der folgenden Arten:

 * Beim Einschalten den BOOT0-Pin auf der Platine überbrücken (nach dem Einschalten die Überbrückung wieder entfernen). Der Bildschirm bleibt danach weiß – das ist normal.
 * `CONFIG` `→DFU` `RESET AND ENTER DFU` auswählen


Führen Sie den folgenden Befehl aus. Ersetzen Sie build/ch.bin durch den Pfad zur .bin-Datei der heruntergeladenen Firmware.

```
dfu-util -d 0483:df11 -a 0 -s 0x08000000:leave -D build/ch.bin
```


### Flashen mit dfu-util (Windows 10)

Unter Windows wird beim Anschließen des NanoVNA im DFU-Modus automatisch ein Gerätetreiber installiert; mit diesem Treiber kann dfu-util jedoch nicht verwendet werden.
Hier wird der Treiber mithilfe von [Zadig](https://zadig.akeo.ie) ausgetauscht.

Starten Sie das Gerät im DFU-Modus. Dies gelingt auf eine der folgenden Arten:

 * Beim Einschalten den BOOT0-Pin auf der Platine überbrücken (nach dem Einschalten die Überbrückung wieder entfernen). Der Bildschirm bleibt danach weiß – das ist normal.
 * `CONFIG` `→DFU` `RESET AND ENTER DFU` auswählen

Starten Sie Zadig, während das NanoVNA im DFU-Modus angeschlossen ist, und stellen Sie wie unten gezeigt ein, dass für den „STM32 BOOTLOADER" WinUSB als Treiber verwendet wird.

<img src="images/win-zadig.png">

Hinweis: Um den Treiber zurückzusetzen, suchen Sie im „Geräte-Manager" unter „Universelle Serial-Bus-Controller" das betreffende Gerät und führen Sie „Gerät deinstallieren" aus. Beim erneuten Einstecken des USB-Kabels wird der Treiber automatisch installiert.

Als Nächstes richten Sie dfu-util ein. Laden Sie die Datei [dfu-util-0.9-win64.zip](http://dfu-util.sourceforge.net/releases/dfu-util-0.9-win64.zip) aus dem releases-Bereich herunter und entpacken Sie sie.
In diesem Beispiel wird nach C:\dfu-util entpackt (der Speicherort ist beliebig).

Klicken Sie mit der rechten Maustaste auf das Startmenü und wählen Sie „Windows PowerShell". Das Shell-Fenster öffnet sich.

<img src="images/win-run-dfu-util.png">

Ziehen Sie dfu-util.exe aus dem Explorer per Drag & Drop in das PowerShell-Fenster; der Pfad wird automatisch eingefügt. Der folgende Aufruf mit `--version` zeigt die Version von dfu-util an:

```
C:\dfu-util\dfu-util.exe --version
```

<img src="images/win-firmware.png">

Ebenso lässt sich der Pfad der Firmware-Datei per Drag & Drop aus dem Explorer in PowerShell einfügen.


Führen Sie den folgenden Befehl aus. Ersetzen Sie build\ch.bin durch den Pfad zur .bin-Datei der heruntergeladenen Firmware.

```
C:\dfu-util\dfu-util.exe -d 0483:df11 -a 0 -s 0x08000000:leave -D build\ch.bin
```

## Firmware flashen (Windows GUI)

Für alle, die mit der Kommandozeile nicht vertraut sind, wird zusätzlich eine Methode mit dem von ST bereitgestellten Werkzeug DfuSE Demo vorgestellt. Sie erfordert einige zusätzliche Schritte, ist hier aber der Vollständigkeit halber aufgeführt.

Laden Sie [STSW-STM32080 von der ST-Website](https://www.st.com/ja/development-tools/stsw-stm32080.html) herunter.

Enthalten sind:

 * DFU File Manager: Werkzeug zum Erstellen von .dfu-Dateien aus .bin- oder .hex-Dateien
 * DfuSe Demo: Werkzeug zum Flashen von .dfu-Dateien auf das Gerät

### Dateiformat mit dem DFU File Manager konvertieren

Starten Sie zuerst den DFU File Manager.

<img src="images/dfu-file-manager-open.png">

Wählen Sie „I want to GENERATE a DFU file from S19, HEX or BIN files".

<img src="images/dfu-file-manager-from-hex.png">

Klicken Sie auf die Schaltfläche „S19 or Hex...". Wählen Sie die .hex-Datei der Firmware, z. B. „ch.hex".

<img src="images/dfu-file-manager-generate.png">

Klicken Sie auf die Schaltfläche „Generate..." und erstellen Sie unter einem geeigneten Namen die .dfu-Datei.

### Firmware mit DfuSe Demo flashen

Starten Sie zuerst das Gerät im DFU-Modus. Dies gelingt auf eine der folgenden Arten:

 * Beim Einschalten den BOOT0-Pin auf der Platine überbrücken (nach dem Einschalten die Überbrückung wieder entfernen). Der Bildschirm bleibt danach weiß – das ist normal.
 * `CONFIG` `→DFU` `RESET AND ENTER DFU` auswählen

Starten Sie DfuSe Demo. Vergewissern Sie sich, dass unter „Available DFU Devices" der Eintrag „STM Device in DFU Mode" erscheint, und klicken Sie auf „Choose...".

<img src="images/dfuse-demo-choose.png">

Wählen Sie die zuvor gespeicherte .dfu-Datei aus.

<img src="images/dfuse-upgrade.png">

Klicken Sie auf die Schaltfläche „Upgrade".

<img src="images/dfuse-leave-dfu-mode.png">

Nach Abschluss des Flashvorgangs erscheint dieser Bildschirm. Klicken Sie auf „Leave DFU mode", um den DFU-Modus zu verlassen. Das Gerät wird zurückgesetzt und startet mit der neuen Firmware.

# Leitfaden zur Firmware-Entwicklung

Für die Entwicklung der NanoVNA-Firmware werden folgende Werkzeuge benötigt:

 * Git
 * gcc-arm-none-eabi
 * make

Wenn diese bereits vorhanden sind, lässt sich die Firmware einfach mit `make` bauen.

```
git clone git@github.com:ttrftech/NanoVNA.git
cd NanoVNA
git submodule update --init --recursive
make
```

## Bauen mit Docker

Mit Docker lässt sich die Firmware ohne lästige Einrichtung bauen. Docker ist ein kostenloses, plattformübergreifendes Container-Werkzeug. Es eignet sich, um eine bestimmte Umgebung (hier: die Build-Umgebung) schnell zu reproduzieren.

Installieren Sie [Docker](https://hub.docker.com/) und führen Sie einfach den folgenden Befehl aus:

```
docker run -it --rm -v $(PWD):/work edy555/arm-embedded:8.2 make
```

## On-Chip-Debugging mit Visual Studio Code

Visual Studio Code (im Folgenden VSCode) ist ein kostenloser, plattformübergreifender Code-Editor von Microsoft.
Mit der Extension [Cortex-Debug](https://marcelball.ca/projects/cortex-debug/) lässt sich On-Chip-Debugging über eine GUI durchführen.

Plattformspezifische Details werden hier ausgelassen; zusätzlich zu oben Genanntem werden folgende Komponenten benötigt:

 * openocd
 * VSCode
 * Cortex-Debug

Cortex-Debug wird über die Extension-Suche in VSCode gefunden und installiert.

### tasks.json

Definieren Sie zuerst in VSCode einen „Task", um das gesamte NanoVNA-Projekt mit make zu bauen.

```json
{
    "tasks": [
        {
            "type": "shell",
            "label": "build",
            "command": "make",
            "args": [
            ],
            "options": {
                "cwd": "${workspaceRoot}"
            }
        }
    ],
    "version": "2.0.0"
}
```

Damit lässt sich make als Task in VSCode ausführen.

### launch.json

Definieren Sie als Nächstes, wie beim Debuggen gestartet werden soll. Richten Sie die Konfiguration nach der Anleitung von Cortex-Debug ein.

Nachfolgend die Konfiguration für die Verwendung eines ST-Link. Bei Verwendung eines J-Link ersetzen Sie `interface/stlink.cfg` durch `interface/jlink.cfg`.

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "cortex-debug",
            "servertype": "openocd",
            "request": "launch",
            "name": "OpenOCD-Debug",
            "executable": "build/ch.elf",
            "configFiles": [
                "interface/stlink.cfg",
                "target/stm32f0x.cfg"
            ],
            "svdFile": "./STM32F0x8.svd",
            "cwd": "${workspaceRoot}",
            "preLaunchTask": "build",
        }
    ]
}
```

Die unter `svdFile` anzugebende Datei kann von der [ST-Website](https://www.st.com/ja/microcontrollers-microprocessors/stm32f072c8.html#) heruntergeladen werden.
Ohne Angabe von `svdFile` funktioniert das Debugging ebenfalls.

### Debugging starten

Mit Start Debugging (`F5`) wird nach dem make-Build automatisch OpenOCD gestartet und die Firmware übertragen.
Nach Abschluss der Übertragung hält das Programm am Reset-Handler an.

<img src="./images/vscode-openocd.png">

Wenn `svdFile` angegeben ist, werden die vordefinierten MCU-Register im Debug-Fenster angezeigt.

<img src="./images/vscode-svd.png">

# Anwendungsbeispiele

## Abstimmung eines Bandpassfilters
TODO

## Abstimmung einer Antenne

<img class="capture" src="images/example-antenna.png">

Hier ein Beispiel für die Verwendung des NanoVNA als Antennenanalysator.

Bei der Antennenabstimmung sind folgende zwei Punkte entscheidend:

 * Befindet sich die Antenne im abgestimmten/resonanten Zustand? (d. h. ist die Reaktanz bei der Zielfrequenz nahe 0?)
 * Ist das SWR der Antenne niedrig? (ist die Anpassung ausreichend?)

### Trace-Einstellungen

Für die Antennenabstimmung wird nur CH0 verwendet; daher führen Sie die Kalibrierung für alle Schritte außer `THRU` und `ISOLN` durch.

Die Trace-Einstellungen sind wie folgt:

 * Trace 0: CH0 SWR
 * Trace 1: CH0 REACTANCE
 * Trace 2: CH0 SMITH
 * Trace 3: OFF

Stellen Sie die Frequenz, auf die die Antenne abgestimmt werden soll, als `CENTER` ein und wählen Sie eine passende `SPAN`.

Suchen Sie die Frequenz, bei der Trace 1 (Reaktanz) nahe 0 liegt. Diese Frequenz ist der Resonanzpunkt. Weicht er ab, justieren Sie die Antenne so, dass der Resonanzpunkt auf die Zielfrequenz fällt.

Liegt der Resonanzpunkt bei der Zielfrequenz, prüfen Sie, ob Trace 0 (SWR) ein ausreichend niedriges SWR (nahe 1) anzeigt. Zeigt er kein ausreichendes SWR (unter 2), führen Sie die Anpassung mit dem Smith-Diagramm durch.
Dafür kann z. B. auch ein Antennentuner unmittelbar an der Antenne verwendet werden.

Sinkt das SWR, ist die Antenne bei der Zielfrequenz resonant und die Abstimmung der Antenne mit niedrigem SWR abgeschlossen.

## Prüfung eines Kabels

Mit dem Tiefpass-Modus im Zeitbereich lässt sich ein TDR simulieren. Mit einem TDR können Fehler auf Übertragungsstrecken aufgespürt werden.

TODO

## Messung eines Gleichtaktfilters (Common-Mode-Filter)

