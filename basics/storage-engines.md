# Storage Engines 

## Warum ?

```
Du triffst die Auswahl:
Wie sollen Deine gespeichert werden
```

## Wie unterscheiden sich die Storage Engines ?

  * In der Performance, Features und anderen Charakteristiken, die Du brauchst 

## Was machen Sie ?

  * Sie sind zuständig für: Speichern und Lesen aller den in MySQL
  * Jede Storage Engine hat:
    * Vor- und Nachteile  
  * Der Server kommuniziert den Storage Engines über die storage engine API 
    * Unterschiede kann ich durch das Interface nicht sehen.
    * Die api enthält contains x 12 low-level Funktionen e.g. “Beginne eine Transkation”, “Hole die Zeilen, die diesen Primärschlüssel hat”

## Storage Engine machen folgendes NICHT ....

  * Storage Engines parsen kein SQL
  * Storage Engines kommunizieren nicht miteinander.

## Welches sind die Wichtigsten ?

  * MyISAM/Aria
  * InnoDB (Default) 
  * Memory
  * CSV
  * Blackhole (/dev/null)
  * Archive
  * Federated/FederatedX
