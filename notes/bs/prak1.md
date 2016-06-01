Aufgabenblatt 1
=======
----------

**1 - Was ist die Aufgabe der Shell?**

Mit der Shell können Programme gesucht und ausgeführt werden. Sie kann interaktiv Kommandos ausführen, Ein- und Ausgabe des Terminals steuern, Kind- und Hintergrundprozesse erzeugen, und dient sogar als eigene Programmiersprache (Shell-Skripts).

**2 - Welche Schritte werden durchlaufen, wenn man einen Befehl eingibt?**

Wenn ein Befehl eingegeben wird, prüft die Shell zuerst ob es sich um einen *built-in* Befehl handelt, den sie selbst ausführen kann. Falls das nicht der Fall ist, wird nach einem Befehl mit absoluter Pfadangabe gesucht (also bspw. im aktuellen Verzeichnis wenn nur der Befehlsname eingegeben wurde). Anschließend wird in den Verzeichnissen die im *PATH* angegeben sind gesucht. 

**3 - Welche Funktionen haben Shell-Variablen?**

Mit Shell Variablen können eigene, temporäre Variablen festgelegt werden, welche innerhalb eines Prozesses gespeichert werden. Wichtige Shell-Variablen:
- `$#` (Anzahl der Argumente)
- `$*` (Alle Argumente)
- `$0` (Programm was gerade ausgeführt wird)
- `$1-9` (Bestimmte Argumente).
- Eigene definierte Variablen (`a=10`)

**4 - Nennen Sie mindestens 3 nützliche Befehle für Shell-Variablen**

`set`zeigt die Werte aller aktuellen Shell-Variablen an. Mit `unset` kann eine Variable wieder gelöscht werden. Durch `readonly` kann der Wert einer Variable nicht mehr verändert werden.
Mit `echo` können Variablen ausgegeben werden.

**5 - Was ist der Unterschied zwischen einer Shell-Variablen und einer Umgebungsvariablen und wie wird die Umgebungsvariable angelegt?**

Shell-Variablen sind vom Benutzer definiert und existieren nur in dem Kontext des aktiven Prozesses. Sobald der Prozess beendet wird, werden die Shell-Variablen gelöscht. Shell Variablen sind Kindern des aktiven Prozesses nicht zugänglich. 

Umgebungsvariablen sind vom System definiert und gelten für alle Prozesse. Innerhalb von Prozessen können zwar Umgebungsvariablen verändert oder neu angelegt werden, diese verfallen allerdings sobald der Prozess geschlossen wird. Kinder des aktiven Prozesses haben Zugriff auf die veränderten Umgebungsvariablen des Vater-Prozesses. 

**6 - Welche Ausgabe wird durch folgendes Script erzeugt?**
```shell
 #! /usr/bin/sh
 a=Eins
 b=Zwei
 c = Drei
 echo "$a, $b, $c"
```
Die ersten zwei Variablen werden korrekt ausgegeben, bei `$c` wird eine Fehlermeldung produziert (*not found*).

**7 - Wie müsste das Scriptfile verändert werden, damit die Fehlermeldung verschwindet und wie sieht dann die Ausgabe aus?**

Bei der Deklaration von Shellvariablen dürfen keine Leerzeichen zwischen dem Gleichheitszeichen und den Werten stehen. Die korrekte Definition von `$c` wäre also:  
`c=Drei`  
Damit ist ist Ausgabe dann *Eins, Zwei, Drei* wie gewünscht.

**8 - Was steht nach folgendem Aufruf in der Shell-Variablen benutzer?**
``benutzer=`who | cut -c1-10 | sort` ``

In `benutzer` stehen die Benutzernamen aller aktuell angemeldeten Benutzer, alphabetisch sortiert. Mit `who` wird die Liste allter angemeldeten Benutzer ausgegeben (mit zusätzlichen Informationen wie Anmeldezeitpunkt usw.). Durch `cut -c1-10` werden nur die ersten 10 Zeichen jeder Zeile ausgewählt (und somit nur der Benutzername). `sort`sortiert die Ergebnisse.

**9 - Wie kann der Rückgabewert des zuletzt ausgeführten Programms überprüft werden?**

Der letzte Rückgabewert kann mit `$?` abgerufen werden.

**10 - Was steht nach folgendem Aufruf auf dem Rechner advbs08 in der Shell-Variablen bashuser und wie kann man sich den Inhalt ansehen?**
``bashuser=`ypcat passwd | grep bash | cut -d: -f5` ``

Mit `ypcat passwd`werden alle Benutzer des Systems angezeigt, mit `grep` können Zeilen nach dem Vorkommen eines bestimmten Musters gefiltert bzw. durchsucht werden. Hier werden also nur Benutzer angezeigt, welche ein *bash* Directory in ihrem Path haben (?). 
Durch `cut -d: -f5` wird nur der Name des Benutzers angezeigt. Mit der Option `-d` kann ein eigenes Symbol angegeben werden dass als Trennzeichen dient, hier der Doppelpunkt. Mit `-f4` wird dann das fünfte "Feld" ausgewählt (Felder durch `:` getrennt).
Die Ausgabe von *bashuser* geschieht über:
```shell
echo $bashuser
```
