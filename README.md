# RFID Bag-Check – Minimal-Implementierung

Zweck: Dokumentation von Taschenkontrollen mittels Zufallsauswahl (rot/grün) und RFID-Scan von Wachdienst + Mitarbeiter. 
Backend: **FastAPI** (Python, SQLite). Frontend: **React (Vite)**.

## Schnellstart

### 1) Backend
```bash
cd backend
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
# DB aus Personal.txt befüllen:
python seed_from_personal.py
# Server starten
uvicorn app:app --reload --host 0.0.0.0 --port 8000
```

### 2) Frontend
```bash
cd frontend
npm install
# API-Basis optional:
# echo "VITE_API_BASE=http://localhost:8000" > .env
npm run dev
```

Danach: http://localhost:5173 öffnen.

## Hardware
Die meisten RFID-Leser verhalten sich wie eine USB‑Tastatur („Keyboard Wedge“). Kursor in das RFID-Eingabefeld setzen und Karte kurz an den Leser halten.

## Wichtig
- **Wahrscheinlichkeit** für „rot“ (Kontrolle) per Umgebungsvariable `DRAW_PROBABILITY` (Standard 0.2 = 20%).
- **CORS**: `CORS_ORIGINS` in `.env.example` anpassen, falls Frontend anders läuft.
- **Rollen**: Die Seeder-Logik weist allen Namen mit „Wachdienst“ die Rolle `GUARD` zu, sonst `EMPLOYEE`. Dies kann bei Bedarf erweitert werden.
- **Datenschutz/DSGVO**: Protokolle enthalten personenbezogene Daten. Legen Sie Aufbewahrungsfristen fest (z. B. automatische Löschung nach X Tagen), Rollen- und Rechtekonzept, Zweckbindung, Informationspflichten und Betriebsratsbeteiligung. 

## Endpunkte (Kurzüberblick)
- `POST /api/draw` → { selected, color }
- `POST /api/scan` → Person zu RFID
- `POST /api/event` → Kontrolle speichern (nur bei rot)
- `GET /api/events?limit=50` → Letzte Kontrollen
- `GET /api/stats` → Kennzahlen (nach Wachdienst/Mitarbeiter)

## Datenmodell
**persons**(rfid PK, name, role)  
**events**(id PK, timestamp, selected, employee_rfid, guard_rfid, status, notes)

Viel Erfolg! ✨
