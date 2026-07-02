# Star-chart --- Principal Engineer Design Plan

## Goal

**Star-chart** lets a user enter:

-   Date
-   Location
-   Optional time/night window

Then it shows:

-   Which constellations are visible
-   Best viewing time and direction
-   How to spot them
-   Bright stars / anchor patterns
-   Short mythology/lore
-   Practical tips based on moon phase and light pollution

## Product Shape

One `index.html` containing:

-   HTML structure
-   CSS
-   JavaScript
-   Embedded constellation metadata JSON
-   Optional CDN scripts

No backend required.

## Recommended Architecture

``` text
index.html
 ├─ UI
 │   ├─ Date picker
 │   ├─ Location input / geolocation
 │   ├─ Viewing time selector
 │   └─ Results cards
 │
 ├─ Astronomy engine
 │   ├─ Convert date/time/location
 │   ├─ Compute local sidereal time
 │   ├─ Determine constellation/star altitude
 │   └─ Filter visible targets
 │
 ├─ Data layer
 │   ├─ Constellation metadata
 │   ├─ Bright guide stars
 │   ├─ Lore text
 │   └─ Spotting instructions
 │
 └─ Presentation
     ├─ Sky visibility ranking
     ├─ Compass direction
     ├─ "How to find it"
     └─ Lore card
```

## Core Technical Approach

Use a lightweight browser astronomy library such as:

``` html
<script src="https://cdn.jsdelivr.net/npm/astronomy-engine@2/astronomy.browser.min.js"></script>
```

For each constellation, store a representative anchor point:

``` js
{
  id: "orion",
  name: "Orion",
  raHours: 5.5,
  decDeg: 5,
  season: "Winter",
  guideStars: ["Betelgeuse", "Rigel", "Bellatrix"],
  spotting: "Look for three bright stars in a short straight line: Orion’s Belt.",
  lore: "In Greek mythology, Orion was a great hunter placed among the stars."
}
```

Then calculate whether that anchor point is above the horizon for the
selected location and time.

## MVP Behavior

1.  Ask for date and location.
2.  Use the user's latitude and longitude.
3.  Test visibility at 9 PM, midnight, and 3 AM.
4.  Show constellations that rise above roughly 20° altitude.
5.  Sort by maximum altitude.
6.  Display direction: north, northeast, east, southeast, south,
    southwest, west, northwest.
7.  Add "easy", "moderate", or "hard" difficulty based on altitude and
    brightness.

## Result Card Design

``` text
Orion
Best time: 9:00 PM
Direction: Southeast
Altitude: 43°
Difficulty: Easy

How to spot it:
Look for three bright stars in a short straight line: Orion’s Belt.

Lore:
In Greek mythology, Orion was a great hunter placed among the stars.
```

## Single-file Implementation Plan

Build in this order:

1.  Static UI shell
    -   Header
    -   Inputs
    -   Empty results state
    -   Cards
2.  Location handling
    -   Browser geolocation
    -   Manual latitude/longitude fallback
    -   Optional city-name lookup later
3.  Astronomy calculations
    -   Convert RA/Dec to altitude/azimuth
    -   Sample several times during the night
    -   Determine best viewing time
4.  Constellation database
    -   Start with 15--25 recognizable constellations
    -   Expand later to all 88
5.  UX polish
    -   Sky quality notes
    -   Moon phase warning
    -   "Look north/east/south/west" guidance
    -   Mobile-first layout

## Good MVP Constellation Set

-   Orion
-   Ursa Major
-   Ursa Minor
-   Cassiopeia
-   Cygnus
-   Lyra
-   Aquila
-   Scorpius
-   Sagittarius
-   Taurus
-   Gemini
-   Leo
-   Pegasus
-   Andromeda
-   Canis Major
-   Boötes
-   Corona Borealis
-   Hercules

## Key Design Principle

Do **not** try to render a perfect planetarium first. Make the first
version answer the user's real question:

> "What can I actually see tonight, where should I look, and why is it
> interesting?"

A simple, accurate, friendly guide will be more useful than a complex
sky map.
