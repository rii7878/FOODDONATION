# 🍲 PlateShare – Food Donation App

A small full-stack web app that connects people with **surplus food** (homes, restaurants, hostels, caterers) to **people and NGOs who need it**, before it goes to waste.

- **Donors** post leftover food with quantity, pickup address and a best-before time.
- **Receivers (individuals / NGOs)** browse live listings, sorted by soonest expiry, and claim a meal.
- Pickup address and phone numbers are shared **only** between the donor and the receiver who claimed it.
- Donor confirms pickup; expired listings are flagged automatically.

## Tech stack
| Layer    | Tech |
|----------|------|
| Backend  | Node.js, Express, JWT auth, bcrypt |
| Database | JSON file (`data/db.json`) – zero setup, swappable for SQLite/Postgres/MongoDB |
| Frontend | Vanilla HTML, CSS, JavaScript (served by Express) |

## Run locally
```bash
git clone <your-repo-url>
cd food-donation-app
npm install
cp .env.example .env     # then set JWT_SECRET to a long random string
npm start
```
Open http://localhost:3000

Run the end-to-end API test: `npm test`

## How it works
1. Sign up as **Donor** or **Receiver**.
2. Donor → *Share food* → publish a listing.
3. Receiver → *Claim this food* → sees pickup address and donor phone in *My claims*.
4. Donor → *Mark as collected*. Done.

## API
| Method | Endpoint | Who | Description |
|--------|----------|-----|-------------|
| POST | `/api/auth/register` | public | Create account |
| POST | `/api/auth/login` | public | Log in, returns JWT |
| GET | `/api/auth/me` | logged in | Current user |
| GET | `/api/donations?city=&category=&q=` | public | Available food |
| GET | `/api/donations/stats` | public | Live counters |
| GET | `/api/donations/mine` | logged in | My donations / claims |
| POST | `/api/donations` | donor | Create listing |
| POST | `/api/donations/:id/claim` | receiver | Claim food |
| POST | `/api/donations/:id/release` | receiver | Give up a claim |
| POST | `/api/donations/:id/collect` | donor | Mark as collected |
| DELETE | `/api/donations/:id` | donor | Remove unclaimed listing |

## Project structure
```
food-donation-app/
├── server/
│   ├── index.js            # starts the server
│   ├── app.js              # Express app and routes
│   ├── auth.js             # JWT + role middleware
│   ├── db.js               # JSON-file database
│   └── routes/{auth,donations}.js
├── public/                 # frontend (index.html, css/, js/)
├── test/smoke.test.js
└── package.json
```

## Ideas to extend
- Photo upload for food, map view with geolocation
- SMS / email / WhatsApp notifications when food is claimed
- Ratings for donors and receivers, NGO verification by an admin
- Move the database to PostgreSQL or MongoDB
- Deploy on Render / Railway (set `JWT_SECRET` as an environment variable)

## Food safety note
Only donate food that is fresh, properly stored and safe to eat.

## License
MIT
