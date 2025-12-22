# Linux Cheatsheet

## Grundlagen & Benutzer
| Befehl | Beschreibung | Beispiel |
| :--- | :--- | :--- |
| `echo` | Gibt Text im Terminal aus. Gänsefüßchen bei Sätzen verwenden. | `echo "Hallo Welt"` |
| `whoami` | Zeigt an, als welcher Benutzer man eingeloggt ist. | `whoami` |

---

## Navigation im Dateisystem
| Befehl | Vollständiger Name | Beschreibung | Beispiel |
| :--- | :--- | :--- | :--- |
| `pwd` | Print Working Directory | Zeigt den absoluten Pfad des aktuellen Ordners an. | `pwd` |
| `ls` | List | Listet Dateien und Ordner auf. | `ls` |
| `cd` | Change Directory | Wechselt in einen anderen Ordner. | `cd Documents` |
| `ls -la` | List All | Listet **alle** Dateien inkl. versteckter (`.`) + Details (Rechte, Größe, Owner). | `ls -la` |

**Warum wichtig?**  
Zeigt `.secret` Dateien, erkennt falsche Rechte, wichtig für PrivEsc & Forensics.

---

## Dateien Lesen & Finden
| Befehl | Beschreibung | Beispiel |
| :--- | :--- | :--- |
| `cat` | Zeigt den gesamten Inhalt einer Datei an. | `cat todo.txt` |
| `find -name <datei>` | Sucht Dateien im aktuellen Ordner + Unterordnern. | `find -name passwort.txt` |
| `find . -name flag.txt` | Sucht im aktuellen Verzeichnis. | `find . -name flag.txt` |
| `find / -name flag.txt` | Sucht im gesamten System (langsam, viele Errors). | `find / -name flag.txt` |
| `find / -name flag.txt 2>/dev/null` | Sucht überall und blendet „Permission denied“ aus. | `find / -name flag.txt 2>/dev/null` |
| `grep "<text>" <file>` | Sucht nach Text in einer Datei. | `grep "192.168.1.1" access.log` |
| `grep -r "<text>" <ordner>` | Sucht rekursiv in allen Unterordnern. | `grep -r "password" /var/www/html` |

**Warum wichtig?**  
`grep -r` ist essenziell für CTFs, Log-Analyse, Credential-Hunting.

---

## Operatoren & Umleitungen
| Operator | Funktion | Beispiel |
| :--- | :--- | :--- |
| `&` | Führt Befehl im Hintergrund aus. | `copy largefile.iso &` |
| `&&` | Führt zweiten Befehl nur bei Erfolg des ersten aus. | `cd Documents && ls` |
| `>` | Überschreibt Datei mit neuer Ausgabe. | `echo "Neu" > liste.txt` |
| `>>` | Hängt Ausgabe an Datei an. | `echo "Dazu" >> liste.txt` |

