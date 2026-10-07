# WA Commute Crash Risk

**Live app:** https://arthurjclack.github.io/wa-commute-crash-risk/

A single-page web app that maps every fatal traffic crash in Washington State and estimates how likely you are to crash on your own commute.

## What it does

- **Crash map:** every Washington crash where someone died, from 2010 to 2024, loaded live from NHTSA's Fatality Analysis Reporting System (FARS). Switch between points and a heat map, filter by cause (speeding, alcohol, pedestrian, cyclist, motorcycle, young driver, dark, ran off road), and see crashes by hour of day.
- **Commute check:** enter two addresses or drop pins on the map. The app routes the drive (plus alternate routes), grades each route, and shows your odds of any crash, an injury crash and a fatal crash per trip, per year and over 10 years. It also lists the deadliest one-mile stretches along the way.
- **Personal factors:** driver age, departure and return times, weather, phone use, speeding habit, vehicle type and seat belt or helmet use. Results update as you change them.

## How the estimate works

`P = 1 − e^(−rate × miles)`, where the rate is a Washington baseline multiplied by:

1. **Road-type mix:** each stretch is classed as interstate, highway, arterial or local street from the routing data, with per-mile crash rates for each class.
2. **Local crash history:** FARS crashes within 100 m of the route, compared with how many you'd expect on that road mix. The ratio is shrunk toward 1 and capped between 0.6× and 2.5×.
3. **Personal factors:** from published research (AAA Foundation crash rates by age, NHTSA night-time risk, Qiu & Nixon weather meta-analysis, Virginia Tech naturalistic driving study on phone use).

Baselines: 1.75 police-reported collisions per million vehicle miles (WSDOT, 2023) and 674 fatal crashes over 60.65 billion vehicle miles (FARS / WTSC, 2024).

## Data sources (all open, no API keys)

| Purpose | Service |
|---|---|
| Fatal crash records | [NHTSA FARS on the US DOT map server](https://geo.dot.gov/server/rest/services/NHTSA/FQAcc/MapServer) |
| Routing | [OSRM](https://project-osrm.org/) on OpenStreetMap |
| Address search | [Photon](https://photon.komoot.io/) on OpenStreetMap |
| Base map | [Esri World Gray Canvas](https://services.arcgisonline.com/arcgis/rest/services/Canvas) |

## Limits

- Washington's full crash database (including non-fatal crashes) isn't published as an open API, so the map shows fatal crashes only. The "any crash" odds come from statewide rates adjusted for the route and driver.
- Traffic volume per road type uses typical values, not measured counts on each road.
- This is a statistical estimate for a typical driver with the chosen characteristics. Use it to compare routes and times, not as a personal prediction.

## Run it

It's one HTML file with no build step. Open `index.html` in a browser, or serve the folder with any static host.

Built with Leaflet, plain JavaScript and the open data services above.
