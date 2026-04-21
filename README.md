AI-Driven Job Matching Automation
Dieses Projekt automatisiert das Monitoring und die Analyse von Stellenanzeigen. Das System läuft 24/7 auf einer Azure VM und benachrichtigt mich bei relevanten Treffern via Telegram.

Funktionsweise
Trigger: Der Workflow startet alle 8 Stunden automatisch.
Scraping: Ein Apify-Actor extrahiert aktuelle Stellenanzeigen von LinkedIn.
Heuristischer Filter: Ein JavaScript-Node filtert die Ergebnisse vorab nach harten Kriterien (Ort, Job-Typ, Blacklist für Support/Sales), um Rechenlast und Kosten zu senken.
KI-Analyse: Top-Treffer werden an Llama 3.3 (Groq API) gesendet. Die KI bewertet die Anzeige gegen mein Profil und gibt einen Score sowie eine kurze Begründung aus.
Deduplizierung: Vor dem Versand wird via PostgreSQL (Unique Constraints) geprüft, ob die Stelle bereits verarbeitet wurde.
Notification: Bei einem hohen Match-Score erfolgt eine Nachricht per Telegram.

Tech-Stack
Orchestrierung: n8n
Infrastruktur: Azure VM (Ubuntu)
LLM: Llama 3.3 (70b-versatile) via Groq
Datenbank: PostgreSQL
Monitoring: Separater n8n-Workflow für Error-Trigger und Alarmierung.

Repository Inhalt
job_matcher.json: Haupt-Workflow (Logik, Filter, KI-Anbindung).
error_handler.json: Monitoring-Workflow für System-Fehler.
