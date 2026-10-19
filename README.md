# MapAble

**Know before you go.** MapAble is a web map that shows people with disabilities which places in Vilnius they can actually enter.

🌐 **Live app:** https://mapablelt.onrender.com
🏆 **Hack4Vilnius 2026 finalist** 

---

## The problem

People with disabilities cannot find out in advance whether a place in Vilnius is accessible to them.

- The information is **scattered**: official assessments on data.gov.lt, disabled parking on JUDU's map, restaurant details on separate websites.
- For most places there is **nothing at all**: 73% of the 3,751 Vilnius places on OpenStreetMap say nothing about wheelchair access.

So people call ahead, search for photos of the entrance, or simply take the risk.

## Our solution

One map with a **clear answer for every place**, and where that answer came from:

| Status | Meaning |
|---|---|
|  Accessible | OpenStreetMap `wheelchair=yes`, listed as adapted for disabled visitors, or official mobility score ≥ 50 |
|  Partly accessible | OpenStreetMap `wheelchair=limited`, or official mobility score 1–49 |
|  Not accessible | OpenStreetMap `wheelchair=no`, or official mobility score 0 |
|  Not checked yet | No source yet, so visitor answers decide |
|  Disabled parking | Parking spaces from JUDU |

## Features

- **One status per place**, always with its source (data.gov.lt, OpenStreetMap, JUDU, restaurant listing or visitors)
- **Nearest disabled parking** for every place: all spots within 500 m, with distance and walking time
- **Filters by need** (wheelchair, vision, hearing, cognitive) and by type of place
- **Visitor answers**: places with no data ask *"Is the entrance step-free?"*. One answer marks a place as *reported*; 3 matching answers confirm it. Official and OpenStreetMap data always come first.
- **Search** by name or category
- **Live counter** of places that still have no wheelchair information
- **Near me**: shows your location and what is around you
- **Directions** to any place or parking spot via Google Maps
- **Accessible design**: Atkinson Hyperlegible font, high contrast, large tap targets, works on phones and desktop

## Data sources

| Source | What it gives us | Records |
|---|---|---|
| [Lithuanian Open Data Portal](https://data.gov.lt) | Official accessibility assessments (mobility, vision, hearing, cognitive scores) | 2,488 buildings |
| JUDU (Vilnius traffic authority) | Disabled parking: spaces, layout, drop-off area | 224 locations, 378 spaces |
| [OpenStreetMap](https://www.openstreetmap.org) | Places, categories, wheelchair tags | 3,751 places |
| Meniu.lt (public listing) | Restaurants listed as adapted for disabled visitors | 30 restaurants |

Combined into one MySQL database of **6,252 places**.

## Tech stack

- **Back end:** Node.js, Express, mysql2
- **Database:** MySQL (distances with `ST_Distance_Sphere`)
- **Front end:** HTML, CSS, JavaScript, [Leaflet](https://leafletjs.com) with OpenStreetMap tiles
- **Hosting:** Render

## Run it locally

**Requirements:** Node.js 18+ and MySQL 8+

1. Clone the repository and install dependencies:
```bash
   git clone https://github.com/YOUR-USERNAME/MapAble.git
   cd MapAble
   npm install
```
2. Create the database and import the data:
```bash
   mysql -u root -p -e "CREATE DATABASE mapable_db"
   mysql -u root -p mapable_db < database/mapable_db.sql
```
3. Create a `.env` file in the project folder:
```env
   DB_HOST=localhost
   DB_PORT=3306
   DB_USER=root
   DB_PASSWORD=your_password
   DB_NAME=mapable_db
   PORT=3000
```
4. Start the server and open http://localhost:3000
```bash
   npm start
```

> `.env` is in `.gitignore`. Never commit your database password.

## API

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/places` | Places in an area, with status data |
| GET | `/api/places/nearby` | Places within a radius of a point |
| GET | `/api/places/accessible` | Only places that fit a need (mobility, vision, hearing, cognitive) |
| GET | `/api/places/:id` | One place with official scores, visitor answers and parking within 500 m |
| POST | `/api/places/:id/verifications` | Save a visitor answer (e.g. step-free entrance yes/no) |
| GET | `/api/parking` | Disabled parking spots |
| GET | `/api/restaurants` | Restaurants listed as adapted |
| GET | `/api/categories` | Place categories |
| GET | `/api/stats` | Totals and the "still unchecked" counter |

## Next steps

- Pilot in the Old Town with Vilnius City Municipality and Go Vilnius
- Wheelchair-friendly routes that avoid steps and steep slopes
- Accessible public transport stops and low-floor buses
- Automatic data refresh from data.gov.lt, JUDU and OpenStreetMap

## Team

- **Azar Taghizade**: Team Lead, Full-Stack Developer
- **Said Mansimov**: Front-End Developer
- **Ali Zamanov**: Back-End Developer

Built in one weekend at **Hack4Vilnius 2026**.
