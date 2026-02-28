# Bingo-Lose Prüfer

Ein browserbasiertes Tool zur digitalen Verwaltung, Ziehung und automatischen Überprüfung von Bingo-Losen nach dem US-System mit 75 Zahlen. Das System verarbeitet alle Daten lokal und benötigt keine Internetverbindung oder Server.

## 🎲 Ziehungssteuerung und Automatik

Nutzer können gezogene Bingo-Zahlen direkt über ein Textfeld eintippen und mit der Enter-Taste bestätigen. Alternativ zieht der Zufallsziehungs-Button sofort eine einzelne, zufällige Zahl aus dem verbleibenden Pool.

*   **Automatikmodus:** Der Autoplay-Modus zieht alle 300 Millisekunden selbstständig eine zufällige Zahl. Die Automatik stoppt sofort, sobald ein Los ein Bingo erreicht oder das Limit von 22 Ziehungen voll ist. Werden 22 Zahlen ohne Gewinn gezogen, leert das System die Ziehung automatisch, erhöht den Rundenzähler und startet nach einer Zehntelsekunde den nächsten Durchlauf.
*   **Korrekturen:** Ein direkter Klick auf eine bereits gezogene Zahl in der Übersichtsliste entfernt diese zielgenau aus der aktuellen Spielrunde. Nutzer können zudem das gesamte Ziehungsfeld über den "Leeren"-Button nach einer kurzen Sicherheitsabfrage komplett bereinigen.
*   **Rückgängig & Wiederholen:** Ein Verlaufsspeicher erlaubt es, bis zu 200 Aktionen beliebig oft vor- und zurückzuspringen.

## 📊 Live-Statistiken und Prüfung

Während der Ziehung zeigt ein Zähler exakt an, wie viele der maximal 22 erlaubten Zahlen in der aktuellen Runde bereits gezogen wurden. Ein separater Rundenzähler dokumentiert die Anzahl der gespielten Runden, was besonders während der aktiven Automatik hilfreich ist. Gleichzeitig protokolliert eine Echtzeit-Anzeige, wie sich die gezogenen Zahlen auf die fünf Spalten B, I, N, G und O verteilen.

Sobald das System eine Zahl zieht, markiert es diese automatisch farblich auf allen betroffenen Losen im Deck. Ein Prüf-Algorithmus kontrolliert nach jeder Ziehung horizontal, vertikal sowie auf beiden Diagonalen, ob ein Los eine vollständige Reihe erreicht hat. Bei Erfolg blendet das Skript direkt auf dem jeweiligen Los Warntexte wie "BINGO!", "DOPPEL-BINGO!" oder "MEHRFACH-BINGO!" ein.

## 📝 Loserstellung und Import

Über ein integriertes Formular können Nutzer neue Lose mit jeweils 25 Zahlen manuell in das System eintragen. Dabei greifen strenge Validierungsregeln für die US-Bingo-Spalten (B: 1-15, I: 16-30, N: 31-45, G: 46-60, O: 61-75).

**Text-Import (z.B. aus OCR):** Die Text-Import-Funktion ermöglicht das einfache Einfügen von unstrukturiertem Text. **Wichtig:** Das System filtert aus diesem Text alle gültigen Zahlen heraus, ordnet diese jedoch *streng nach der eingegebenen Reihenfolge* in die Bingo-Matrix ein! 
Das bedeutet: Die erste erkannte Zahl landet auf Position B1, die zweite auf B2, die dritte auf B3, bis zu B5. Die sechste Zahl landet auf I1 und so weiter. Die Zuordnung erfolgt also nicht automatisch über den Wert der Zahl in die passende Spalte, sondern rein sequenziell nach der Lesereihenfolge.

## 💾 Datenexport und JSON-Struktur

Da das Skript keine Daten permanent speichert, gehen alle angelegten Lose beim Neuladen der Browser-Seite unwiderruflich verloren. Um einen Verlust zu verhindern, können Nutzer alle aktuell angelegten Bingo-Lose als strukturierte JSON-Datei lokal exportieren und später wieder importieren.

Die JSON-Datei beinhaltet die generelle Versionierung, den Zeitstempel und Arrays für gezogene Zahlen sowie die Los-Karten. Die Matrix der Lose ist als ein Array (Reihen) von Arrays (Spalten) aufgebaut:

```json
{
  "version": "1.0",
  "timestamp": "2026-02-28T12:00:00.000Z",
  "drawnNumbers": [],
  "cards": [
    {
      "name": "Sample 1",
      "numbers": [
        [ 9, 30, 35, 59, 74 ], // Erste Zeile (Reihe 1) mit Werten für B, I, N, G, O
        [ 2, 25, 41, 47, 71 ], // Zweite Zeile (Reihe 2)
        [ 11, 27, 44, 48, 63 ], // Dritte Zeile (Reihe 3)
        [ 15, 29, 33, 60, 66 ], // Vierte Zeile (Reihe 4)
        [ 14, 21, 34, 57, 69 ]  // Fünfte Zeile (Reihe 5)
      ]
    }
  ]
}
```

*Hinweis zur JSON-Struktur:* Das Array `numbers` enthält fünf Unter-Arrays. Jedes Unter-Array repräsentiert eine **horizontale Zeile** auf dem Bingo-Los (nicht die Spalten!). Der erste Wert eines Arrays gehört zur Spalte B, der zweite zur Spalte I, usw.

## 🛠️ Systemsteuerung und Bearbeitung

Für einen kompletten Neustart löscht der "Zurücksetzen"-Button nach einer Sicherheitsabfrage sowohl alle gezogenen Zahlen als auch das komplette Deck an Losen. Im Bearbeitungsmodus können Nutzer alternativ einzelne Lose durch Anklicken aus dem System entfernen oder das gesamte Deck über den "Alle löschen"-Button leeren. Ein zusätzliches Hilfe-Menü erklärt grundlegende Rahmenbedingungen und zeigt die aktuelle Software-Version 1.0 an.
