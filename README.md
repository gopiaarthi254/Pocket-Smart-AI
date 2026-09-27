# PocketSmart AI – Your Smart Budget & Recommendation Assistant

AI-powered budget planner for **Home Interiors**, **Party Planning** and **Jewelry** (with outfit-image analysis),
built with **FastAPI + Google Gemini + Jinja2 (HTML/CSS/JS)**.

## Project structure
```
nanmudhalvar project/
├── app.py              # FastAPI app: routes, JWT auth, sessions, history, startup task
├── gemini_utils.py     # Gemini calls, prompts, JSON parsing, shopping links, fallback plans
├── models.py           # Pydantic input/output schemas
├── requirements.txt
├── .env                # your API key goes here (copy of .env.example)
├── static/
│   ├── styles.css
│   ├── script.js       # result rendering, API helper
│   └── uploads/        # uploaded outfit images
├── templates/
│   ├── base.html  index.html  login.html  register.html  dashboard.html
│   ├── home_planner.html  party_planner.html  jewelry_planner.html  history.html
└── data/               # users.json + history.json (created automatically)
```

## How to run (Windows)
1. Install **Python 3.10 – 3.12** from python.org (tick "Add Python to PATH").
2. Open a terminal in this folder (in VS Code: *Terminal → New Terminal*) and run:
   ```
   python -m venv venv
   venv\Scripts\activate
   pip install -r requirements.txt
   ```
3. Get a free Gemini API key at https://aistudio.google.com/app/apikey and paste it into `.env`:
   ```
   GOOGLE_API_KEY=your_key_here
   ```
4. Start the app:
   ```
   python app.py
   ```
5. Open **http://127.0.0.1:8000** → Register → Login → use the planners.

> Without an API key the app still runs and shows rule-based **default recommendations** (Activity 5.4 fallback),
> marked with a "Default plan" badge. With a key, results show a "Gemini AI" badge.

## Routes (as per the project document)
| Route | Method | Purpose |
|---|---|---|
| `/` | GET | Landing page |
| `/register`, `/login` | GET/POST | Registration & login pages |
| `/token` | POST | Issues JWT (stored in an HTTP-only cookie) |
| `/logout` | GET/POST | Blacklists token, clears session |
| `/dashboard` | GET | User dashboard with recent activity |
| `/home-planner`, `/party-planner`, `/jewelry-planner` | GET | Planner pages |
| `/home-budget` (alias `/generate-home`) | POST | Home interior recommendations |
| `/party-budget` (alias `/generate-party`) | POST | Party recommendations |
| `/jewelry-budget` (alias `/generate-jewelry`) | POST | Jewelry recommendations (text + optional image) |
| `/recommendation-history` | GET | User's past recommendations |
| `/recommendation-details/{id}` | GET/DELETE | Full details of one recommendation |
| `/history` | GET | History page |
| `/session-info`, `/session-data` | GET/POST | Session metadata / personalization data |
| `/docs` | GET | Auto-generated Swagger API docs |

## Notes
- **Gemini model:** the document specifies *Gemini 1.5 Flash*. Google has retired the 1.5 models, so the app uses
  `gemini-2.5-flash` by default (set `GEMINI_MODEL` in `.env` to change it). It automatically tries newer Flash
  models if one isn't available.
- **Product data:** as allowed in Activity 3.4, real e-commerce APIs are not called. Gemini suggests products with
  INR prices, and the app builds search links for Amazon, Flipkart, IKEA, Pepperfry, Swiggy, Zomato, OYO,
  MakeMyTrip, BookMyShow, Tanishq, CaratLane, BlueStone, Melorra, Meesho and more.
- **Budget adherence (Milestone 5):** totals, the allocation table and remaining budget are recalculated on the
  server from the actual items, and the UI warns if a plan exceeds the budget.
- **Error handling:** input validation on both frontend and backend; if Gemini fails or returns bad JSON, the
  fallback plan is shown instead of an error.
