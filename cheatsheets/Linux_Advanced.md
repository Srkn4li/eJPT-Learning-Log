# Linux Cheatsheet

## Argumente & Hilfe
Die meisten Befehle ändern ihr Verhalten durch **Flags** (Schalter).
- Kurzform: `-a`
- Langform: `--all`

| Befehl | Beschreibung | Beispiel |
| :--- | :--- | :--- |
| `ls -a` | Zeigt **alle** Dateien (auch versteckte). | `ls -a` |
| `ls --help` | Zeigt Hilfe und Optionen. | `ls --help` |
| `man` | Öffnet das Handbuch eines Befehls. | `man ls` |
| `file` | Zeigt den tatsächlichen Dateityp. | `file unknown_file` |

> **Navigation im `man`:**  
> `↓` Scrollen • `h` Hilfe • `q` Beenden

---

## Dateien & Ordner erstellen/bearbeiten

| Befehl | Vollständiger Name | Funktion | Beispiel |
| :--- | :--- | :--- | :--- |
| `touch` | touch | Erstellt eine leere Datei. | `touch notiz.txt` |
| `mkdir` | make directory | Erstellt einen Ordner. | `mkdir urlaubsbilder` |
| `cp` | copy | Kopiert Datei/Ordner. | `cp notiz.txt backup.txt` |
| `mv` | move | Verschiebt oder benennt um. | `mv alt.txt neu.txt` |
| `rm` | remove | Löscht eine Datei. | `rm muell.txt` |
| `rm -R` | remove recursive | Löscht Ordner + Inhalt. | `rm -R mein_ordner` |

---

## Benutzer & Berechtigungen

Linux unterscheidet zwischen **Owner**, **Group**, **Others**.

**Rechte anzeigen:**  
`ls -l` → `-rw-r--r-- 1 user group ...`

| Zeichen | Bedeutung | Erklärung |
| :--- | :--- | :--- |
| `r` | Read | Datei lesen / Ordnerinhalt anzeigen |
| `w` | Write | Datei ändern / Dateien im Ordner erstellen/löschen |
| `x` | Execute | Datei ausführen / Ordner betreten |

**Benutzer wechseln:**

| Befehl | Beschreibung |
| :--- | :--- |
| `su [user]` | Wechselt zum Benutzer. |
| `su -l [user]` | Wechselt Benutzer + lädt dessen Umgebung. |

---

## Wichtige Systemverzeichnisse

| Pfad | Zweck / Inhalt |
| :--- | :--- |
| `/etc` | Konfigurationen, `passwd`, `shadow`. |
| `/var` | Variable Daten: Logs, Datenbanken, Backups. |
| `/root` | Home-Verzeichnis des root‑Users. |
| `/tmp` | Temporäre Dateien, für alle beschreibbar. |

---

# Berechtigungen & Eigentümer (erweitert)

## `chmod` – Rechte ändern

| Befehl | Beispiel | Erklärung |
| :--- | :--- | :--- |
| `chmod +x skript.sh` | Macht Datei ausführbar. | Notwendig für Skripte, Tools, Exploits. |
| `chmod 777 datei` | Gibt jedem alle Rechte. | Unsicher, aber in CTFs oft nützlich. |
| `chmod 600 id_rsa` | Nur Besitzer darf lesen/schreiben. | Pflicht für SSH‑Private‑Keys. |

---

## `chown` – Besitzer ändern

| Befehl | Beispiel | Erklärung |
| :--- | :--- | :--- |
| `chown user:user datei` | `chown serkan:serkan config.txt` | Ändert Besitzer und Gruppe. |
| `chown -R user:group ordner/` | `chown -R www-data:www-data /var/www` | Rekursiv für Ordner. |

---

# Netzwerk & Remote

## `ssh` – Remote‑Login

| Befehl | Beispiel | Was es tut |
| :--- | :--- | :--- |
| `ssh user@IP` | `ssh serkan@10.10.10.10` | Baut eine SSH‑Verbindung auf. |
| `ssh -i key.pem user@IP` | `ssh -i id_rsa root@192.168.1.10` | Login mit Private Key. |

---

## `yes` – Automatisches Bestätigen

| Befehl | Beispiel | Erklärung |
| :--- | :--- | :--- |
| `yes` | `yes | apt install paket` | Gibt unendlich oft „y“ aus. |
| `yes` (bei SSH) | Wird genutzt, um Fingerprint automatisch zu akzeptieren. | Praktisch für Skripte. |

---

## Texteditoren (Nano & VIM)
**Nano** ist einsteigerfreundlich. **VIM** ist mächtiger, aber komplexer.

**Nano starten:** `nano dateiname`

| Shortcut (Nano) | Funktion |
| :--- | :--- |
| `Ctrl + X` | Beenden (fragt nach Speichern). |
| `Ctrl + O` | Speichern (Write Out). |
| `Ctrl + W` | Suchen (Where Is). |
| `Ctrl + K` | Ganze Zeile ausschneiden. |
| `Ctrl + U` | Text einfügen. |

---

## Dateien transferieren

### Download aus dem Web
| Befehl | Beschreibung | Beispiel |
| :--- | :--- | :--- |
| `wget URL` | Lädt Dateien über HTTP/HTTPS herunter. | `wget http://seite.com/datei.txt` |
| `curl -O URL` | Lädt Datei herunter und speichert sie unter gleichem Namen. | `curl -O http://seite.com/datei` |

> **Hinweis:**  
> `curl` ohne `-O` zeigt den Inhalt nur im Terminal an – speichert aber NICHT.

---

### Eigener Webserver (Python)
Startet einen temporären Webserver im aktuellen Verzeichnis.

| Befehl | Port | Download von anderem PC |
| :--- | :--- | :--- |
| `python3 -m http.server` | 8000 (Standard) | `wget http://[IP]:8000/datei` |

---

### SCP (Secure Copy via SSH)
Kopiert Dateien sicher zwischen Computern. Syntax ist immer `cp QUELLE ZIEL`.

| Richtung | Syntax | Beispiel |
| :--- | :--- | :--- |
| **Upload** (Lokal → Remote) | `scp [Datei] [User]@[IP]:[Pfad]` | `scp file.txt root@10.10.10.10:/root/` |
| **Download** (Remote → Lokal) | `scp [User]@[IP]:[Pfad] [Ziel]` | `scp root@10.10.10.10:/root/file.txt .` |

---

## Prozessmanagement

Jedes Programm ist ein Prozess mit einer **PID** (Process ID).

### Anzeigen & Beenden
| Befehl | Beschreibung |
| :--- | :--- |
| `ps` | Zeigt Prozesse der aktuellen Session. |
| `ps aux` | Zeigt **alle** Prozesse aller User. |
| `top` | Echtzeit‑Prozessmonitor. |
| `kill [PID]` | Beendet Prozess (SIGTERM). |
| `kill -9 [PID]` | Erzwingt Beenden (SIGKILL). |

---

### Dienste verwalten (systemctl)
`systemctl [Option] [Dienstname]`

* `start` / `stop` – Dienst starten/stoppen  
* `enable` / `disable` – Autostart an/aus  
* `status` – Status anzeigen  

---

### Hintergrund & Vordergrund
| Befehl | Beschreibung |
| :--- | :--- |
| `&` | Startet Befehl im Hintergrund. |
| `Ctrl + Z` | Pausiert Prozess → Hintergrund. |
| `fg` | Holt Prozess zurück in den Vordergrund. |

---

## Automatisierung (Cronjobs)

**Cron editieren:** `crontab -e`

**Syntax:**  
`MIN STD TAG MONAT WOCHENTAG BEFEHL`  
`*` = Wildcard

| Beispiel | Bedeutung |
| :--- | :--- |
| `0 * * * *` | Jede volle Stunde. |
| `0 12 * * *` | Täglich um 12:00 Uhr. |
| `*/5 * * * *` | Alle 5 Minuten. |
| `@reboot` | Beim Systemstart. |

---

## Paketmanagement (APT) & Logs

### APT
| Befehl | Beschreibung |
| :--- | :--- |
| `apt update` | Aktualisiert Paketlisten. |
| `apt install [Name]` | Installiert Paket. |
| `apt remove [Name]` | Entfernt Paket. |
| `add-apt-repository` | Fügt Repository hinzu. |

---

### Wichtige Log-Dateien (`/var/log/`)
* `/var/log/apache2/access.log` – Webserver Zugriffe  
* `/var/log/apache2/error.log` – Webserver Fehler  
* `/var/log/syslog` – Systemnachrichten  
* `/var/log/auth.log` – Login‑Versuche (SSH, su, sudo)

---

## History & Spurenanalyse

| Befehl | Beschreibung | Warum wichtig? |
| :--- | :--- | :--- |
| `history` | Zeigt die letzten ~1000 Befehle an. | Oft Passwörter sichtbar, wenn Admin sie versehentlich direkt eingibt (`mysql -u root -pSecret123`). |
| `cat ~/.bash_history` | Zeigt gespeicherte History‑Datei des Users. | Wenn du Zugriff auf einen anderen User hast, kannst du seine History lesen. |
