# Ideas: disasters and the city

Area: earthquakes, tsunamis, wildfires, evacuation, Santiago and Valparaíso.
Ranked. Recommendation: idea 1.

## 1. "El Olivar": the fire and the street that closes (Viña del Mar, 2 Feb 2024)

- **Problem:** In Viña del Mar 2024, fire killed ~131 people. 73 of the first 108 identified victims were older adults. Narrow, irregular streets jammed and blocked the evacuation ([UC study](https://www.uc.cl/noticias/estudio-uc-advierte-causas-que-dificultaron-la-evacuacion-en-los-recientes-incendios/), [CIGIDEN report](https://eldesconcierto.cl/2024/02/22/informe-cientifico-trazado-de-calles-y-falta-de-simulacros-agravaron-incendios-de-valparaiso)). Contraloría says CONAF ran no fire-spread projection that day.
- **Story:** One woman, 78, in El Olivar. 15:00, fire in the reserve. The map shows the fire move down the ravine with the wind. Her neighbors take cars. Her street becomes a line of red dots. Then we replay with one change: the alert 20 minutes earlier, or one pasaje open for pedestrians only.
- **Wow:** The simulated burn area lands on top of the real satellite burn scar (Copernicus). Then: "this street, at 15:47, is where people die" — and a one-line change saves her.
- **Depth grows with agent time:**
  1. Grid 100 m, uniform wind, one fuel → 30 m with KITRAL fuel classes from the CONAF land-use map.
  2. Uniform wind → hourly ERA5 wind → wind corrected for terrain (WindNinja).
  3. Add ember spotting, then house-to-house spread (OSM building footprints as fuel).
  4. Evacuation: one agent per block → one agent per household (INE census, age), car or foot, alert delay, car jams on each OSM street.
  - Demo shows it: a "depth" slider/timeline of versions. Each version overlays the real scar; the match score (IoU) goes up: e.g. 41% → 63% → 82%.
- **Data:** Burn perimeter [Copernicus EMSR715](https://mapping.emergency.copernicus.eu/activations/EMSR715) and damage [EMSN188](https://mapping.emergency.copernicus.eu/activations/EMSN188); Sentinel-2 (dNBR); [INE census blocks 2017](https://www.ine.gob.cl/herramientas/portal-de-mapas/geodatos-abiertos); CONAF land-use cadastre ([SIMEF](https://simef.minagri.gob.cl)); [ERA5](https://cds.climate.copernicus.eu); OSM roads and buildings ([Geofabrik Chile](https://download.geofabrik.de/south-america/chile.html)); Copernicus DEM 30 m.
- **Reuse:** [Cell2Fire](https://github.com/cell2fire/Cell2Fire) (U. de Chile, C++), [C2F+K with Chilean KITRAL fuels](https://github.com/fdobad/C2FK), [Fire2a docs](https://fire2a.github.io/docs/Cell2FireW), [WindNinja](https://github.com/firelab/windninja), OSMnx for the street graph, deck.gl for the map.
- **Main risk:** Sensitive (real deaths, still in court). Tell it with respect. Fire on the urban edge is less calibrated than forest fire.

## 2. "8 minutes": tsunami at night in Viña / Valparaíso

- **Problem:** A M8+ quake off central Chile gives ~10–20 min before the first wave. In Iquique, up to 20% of evacuation route space is blocked by urban obstacles ([Cienfuegos et al. 2024, NHESS](https://nhess.copernicus.org/articles/24/1485/2024/)).
- **Story:** A kid on the beach in Viña with his grandfather, summer night. Thousands of dots run uphill. Water enters the grid. Some dots do not arrive.
- **Wow:** Add one vertical-evacuation building (or open one staircase). The dots that die go from N to almost zero.
- **Depth grows with agent time:** bathtub flood from the SHOA chart → shallow-water model (GeoClaw, adaptive mesh) on real bathymetry → finer nesting near the coast. Agents: same speed → speed by age (census) → reaction delay from surveys → quake debris that blocks streets → crowd density on stairs (JuPedSim).
- **Data:** [SHOA CITSU flood charts](https://www.shoa.cl/php/citsu.php) (PDF/KMZ only); GEBCO bathymetry; Copernicus DEM; OSM; INE census; model comparison for Viña/Valparaíso ([GeoHazards 2022](https://www.mdpi.com/2624-795x/3/2/18)).
- **Reuse:** [GeoClaw/Clawpack](https://www.clawpack.org/geoclaw.html), [JuPedSim](https://pypi.org/project/jupedsim/), Viña ABM with vertical evacuation (Zamora, León, Gubler, Catalán 2023).
- **Main risk:** Hypothetical event, no ground truth to match. CITSU has no shapefile; we must digitize KMZ.

## 3. "The ambulance that does not arrive": San Ramón fault, Santiago

- **Problem:** The San Ramón fault runs under east Santiago. A M6.7–7 quake damages buildings and bridges; nobody models how long an ambulance then takes to reach a hospital.
- **Story:** A man has a heart attack in Peñalolén, 10 minutes after the quake. Before: 9 min to the hospital. After: the map shows each blocked street, and the reach area of each hospital shrinks.
- **Wow:** The hospital reach areas (isochrones) collapse on the projector; half of the east side is outside 30 min.
- **Depth grows with agent time:** uniform shaking → ground-motion model by distance → site effects (Santiago soils, Vs30) → building damage per block from the GEM exposure + fragility curves → debris that blocks streets → traffic agents that flee and jam (SUMO/MATSim).
- **Data:** [GEM SARA model](https://sara.openquake.org), [OpenQuake Chile profile](https://downloads.openquake.org/countryprofiles/CHL.pdf), Santiago exposure and fragility ([NHESS 2019-30](https://nhess.copernicus.org/preprints/nhess-2019-30/nhess-2019-30.pdf)), [IDE MINSAL hospitals](https://www.minsal.cl/ide-minsal-geoportal-descarga-de-datos/), INE census, OSM.
- **Reuse:** [OpenQuake engine](https://github.com/gem/oq-engine), OSMnx, SUMO.
- **Main risk:** Hypothetical event, no ground truth. Many chained models: hard to make deep in 36 h. Weaker emotional hook.

## Why idea 1 first

It is the only one with a real event and a real satellite burn scar. So "more agent time = deeper" is visible as a number that goes up (match with reality). It has a Chilean open-source simulator (Cell2Fire/KITRAL) and a clear person.
