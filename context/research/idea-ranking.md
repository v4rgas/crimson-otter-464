# Idea ranking (round 1 + round 2)

## Re-rank with 3D criterion

New rule from a human (2026-11-21): the demo shows a 3D scene of the simulation itself. The simulated thing moves in 3D space. Charts and data layers alone fail. Style: isometric diorama (tinyrj.com, isometric.nyc). An idea with a weak 3D scene drops.

### 1. "El Olivar": the fire and the street that closes (visual 8.5)

- **3D scene:** Isometric clay diorama, 4 x 4 km of the hills above Viña, cut as a block with earth sides. Copernicus DEM, vertical x1.5 so the quebradas read. Terrain colored by fuel. 15-20k white house boxes (Open Buildings 2.5D or OSM). Fire front glows orange with bloom, burned cells go black, houses turn orange then black and drop. Smoke, embers, wind streaks as points. People dots on OSM streets at L4.
- **Depth in 3D:** Same camera, same hill, a different fire each loop. White EMSR715 scar outline stays on the ground as the target. L0 blocky 100 m ellipse that ignores terrain. L1 30 m cells, fuel colors, the shape bends. L2 wind streaks curve over ridges, the fire runs down the quebradas. L3 embers arc ahead, houses ignite box by box, black lands inside the white outline. L4 dots leave houses, cars jam into a red line. Big IoU number each loop.
- **Wow:** 16:40, one street. Red line of stuck car dots, the front 100 m away. Clock freezes: "20 minutos antes". The fire rewinds, replays, the street is empty, her yellow dot reaches the main road.
- **Stack:** three.js, DEM heightfield PlaneGeometry, fire state as DataTexture in a terrain shader, InstancedMesh houses, Points for smoke/embers/wind, UnrealBloomPass. Offline Numpy cellular automaton (slope, wind, fuel) exports ignition time per cell per level. Browser plays `ignition_time < clock`.
- **Checked on the web:** Open Buildings 2.5D Temporal lists Chile, 2016-2023, heights 0-100 m, but heights are not validated in the Global South. EMSR715 has delineation products; the burned-area figures differ by product, so pick the final delineation. C2FK needs a C++ build and has 0 stars: use the Numpy CA, not Cell2Fire. The phileasdg prior-art link returns 404.
- **Harsh note:** The best 3D and the best emotion. Risk is still overfit to one perimeter and the court case. Hold out a second fire. Never show figures that die.

### 2. "Seis horas de luz": where is the fisherman? (visual 7)

- **3D scene:** Isometric tile of the Golfo de Arauco sea, about 150 km, Tumbes coast on the east edge, cut-away walls show GEBCO bathymetry. Flat-shaded Gerstner sea. 5,000 instanced ghost lanchas follow OpenDrift particles. Sun drops from noon to sunset over 6 h.
- **Depth in 3D:** L0 flat sea, one boat, a 60 km cylinder. L1 cyan current ribbons, the boat splits into 5,000, the swarm bends north. L2 white wind streaks, boats yaw, the swarm splits in two lobes. L3 wave height from real Hs, the swarm gets tight. L4 the swarm becomes a glowing probability surface, one small box locks on the peak.
- **Wow:** Sunset, low camera. Gold drifter line ends far outside the L0 cylinder. Cut to L4: it ends inside the small box, the C-212 passes over it as the sun touches the horizon. "40 km -> 3 km", "17:58".
- **Stack:** three.js, OrthographicCamera, InstancedMesh boats, Sky shader + DirectionalLight, custom flat Gerstner shader. OpenDrift Leeway offline, particles x hours as binary. CMEMS, ERA5, NOAA GDP.
- **Checked on the web:** three.js `webgl_shaders_ocean` example exists. Kenney Watercraft Kit is CC0 (a small fishing boat is not confirmed; a low-poly box boat is enough). OpenDrift Leeway has many object categories; get the skiff code with `list_object_categories("skiff")`.
- **Harsh note:** Strongest metric, weakest scene. Open sea has no structure, so the scene depends on ribbons, sunset and the lobe split. Truth is a drifter, not a lancha, and it is offshore, not in the gulf.

### 3. "La noche de la helada": the invisible cold lake (visual 7.5)

- **3D scene:** Night isometric block, 8 x 8 km cherry valley in Cachapoal, cut sides show soil. Instanced white-blossom cherry trees on real orchard polygons. Pale-cyan particles stream downhill. A glowing blue "cold lake" surface rises in the valley floor. Stations are light pillars with sim vs real min temperature.
- **Depth in 3D:** L0 flat blue tint on low cells, nothing moves, pillars red. L1 frost texture follows sky view. L2 the scene comes alive: particles run downslope, the lake fills hollows, the Ruta 5 fill dams it. L3 a warm orange belt appears on mid slopes, hillside trees stay white, pillars turn green. L4 frost fans punch warm holes in the lake. Big RMSE number each loop.
- **Wow:** 03:40. Camera at eye level inside the lake, blossoms turn black, "-4.8 sim / -5.0 real". Camera rises through the glowing lake surface: white living slopes above, dead valley below. "El cerro estaba más tibio que el valle."
- **Stack:** three.js, DEM PlaneGeometry, InstancedMesh trees, GPUComputationRenderer particles on a baked flow texture (mapbox/webgl-wind pattern), second displaced plane for the lake with fresnel + bloom. Solver: SAGA Cold Air Flow offline, baked as 10-min frames.
- **Checked on the web:** SAGA "Cold Air Flow" exists (`saga_cmd sim_air_flow 0`: DEM, production, friction, 6 h, 10-min frames). Use it for L2 and do not write our own solver. mapbox/webgl-wind is ISC, 1M particles at 60 fps. The CIREN fruit cadastre is a map service on IDE Minagri; a shapefile download is not confirmed.
- **Harsh note:** The best surprise and a true flow simulation. Risk: 4+ stations with hourly data on 17 Sep 2013 inside the tile. Check this first (2 h). The emotion is commercial; a named farmer is necessary.

### All 7

| Idea | Visual | Critics (A / B) | New rank | Reason |
|---|---|---|---|---|
| El Olivar fire | 8.5 | 8 / 8 | 1 | Fire moves on real terrain, houses burn, people move. Best 3D + emotion. |
| Seis horas de luz | 7 | 8.5 / 8.5 | 2 | Best metric. Scene is open sea: needs polish to read. |
| La noche de la helada | 7.5 | 6.5 / 6.5 | 3 | Cold air flows like water. Depth L0->L2 is a clear visual jump. Station risk. |
| Rapa Nui | 7.5 | 7.5 / 5.5 | 4 | Beautiful island. Fit stops after L2, the end is scripted history. |
| Calbuco 2015 tephra | 7 | 7 / 5.5 | 5 | Honest loops, good plume. Falling particles are decoration; weak emotion. |
| 8 minutos tsunami | 8 | 6 / 3 | 6 | Most cinematic. Deadly event is hypothetical, 0.7 m gauge truth is weak. |
| 10 grados | 6 | 7.5 / 7.5 | 7 | Drops. A calibrated surface model, nothing flows in 3D. |

**Recommendation:** Build #1 "El Olivar". It is the only idea where the simulated thing, the people, and the emotion all move in the same 3D scene. Day one: get EMSR715 final delineation, a second held-out fire, and the L0->L3 IoU numbers.

## Earlier ranking (before the 3D criterion)

Two critics scored 31 ideas. The tie breaker: the simulation gets deeper with each agent loop, and the gain is visible and measurable against real data.

## 1. "Seis horas de luz": where is the fisherman? (critics: 8.5 / 8.5)

- **Problem:** A lancha loses its engine off the Chilean coast. The Armada has a few hours of daylight. A coarse drift estimate puts the plane in the wrong square.
- **Story:** WhatsApp voice note from a lancha off Caleta Tumbes: "se nos paró el motor". The phone goes silent. Dark sea, clock: 6 h of light. L0 draws a 60 km circle that one plane cannot cover. Each loop adds one physical mechanism. The particle cloud bends, the search box shrinks and moves.
- **Wow:** The real track of a real drifter appears last. At L0 it ends 40 km outside the circle. At L4 it ends inside a box that one plane sweeps before sunset.
- **Depth ladder:**
  - L0: dead reckoning, mean climatological current, one big circle.
  - L1: hourly CMEMS currents (1/12 deg) in OpenDrift. The cloud bends with the Humboldt current.
  - L2: wind leeway (ERA5 10 m wind, OpenDrift Leeway classes "skiff", "person in water"). The cloud tilts and splits.
  - L3: Stokes drift (CMEMS waves) and tides near the coast. The cloud gets tighter.
  - L4: ensemble over current, wind and leeway uncertainty. The agent fits the windage coefficient on past drifters. Output: probability heatmap and an optimal box sized to one aircraft sweep and the daylight left.
- **Truth metric:** On held-out NOAA GDP tracks off Chile, seed at a past position, compare at +24 h and +48 h. (1) Mean separation in km. (2) Hit rate: % of true end points inside a box of fixed area.
- **Data:** NOAA GDP hourly drifters, use `erddap.aoml.noaa.gov` dataset `drifter_hourly_qc` (the osmc mirror returned nothing). About 64 drifters at 30-42 S for 2015-2022, most with `drogue_lost_date`. Data ends 2022-10-31. [CMEMS](https://data.marine.copernicus.eu) currents and waves. [ERA5](https://cds.climate.copernicus.eu). [Córdova & Flores 2022, JMSE 10:1649](https://mdpi.com/2077-1312/10/11/1649) (C-212 Juan Fernández 2011, used FVCOM).
- **Reuse:** [OpenDrift](https://opendrift.github.io) (Leeway, OceanDrift, readers for CMEMS and ERA5), `copernicusmarine` toolbox, deck.gl TripsLayer on a dark sea. Papers: [arXiv 2201.12688](https://arxiv.org/pdf/2201.12688), [arXiv 2005.09527](https://arxiv.org/pdf/2005.09527).
- **Fatal flaw:** The truth object is a drifter with a 15 m drogue, not a lancha. Adding wind leeway can make drogued drifters WORSE. Gains after L1 can be small at 1/12 deg near the coast. No real rescue case: the emotion is synthetic.
- **Fix:** Use only drogue-lost drifter segments as truth (they behave like surface debris). Say "the real sea, a test object" on stage. Pick the region and month on day one where L2-L3 give a clear gain on held-out tracks. Pre-fetch CMEMS and ERA5 for one region and one month on day one. CASA 212 hook only if humans approve (taste).

## 2. "El Olivar": the fire and the street that closes (Viña del Mar, 2 Feb 2024) (critics: 8 / 8)

- **Problem:** Fire killed about 131 people in Viña del Mar 2024. Most victims were older adults. Narrow streets jammed. CONAF ran no fire-spread projection that day.
- **Story:** A woman, 78, in El Olivar. 15:00, fire in the reserve. The fire moves down the ravine with the wind. Neighbors take cars, her street becomes a line of red dots. Replay with one change: alert 20 min earlier.
- **Wow:** The simulated burn area lands on top of the real satellite burn scar. The IoU goes up each loop. Then the 20-min-earlier alert saves her.
- **Depth ladder:**
  - L0: 100 m grid, one fuel, uniform wind.
  - L1: 30 m grid with KITRAL fuel classes from the CONAF land-use map.
  - L2: hourly ERA5 wind, then terrain-corrected wind (WindNinja).
  - L3: ember spotting, then house-to-house spread (OSM buildings as fuel).
  - L4: evacuation: one agent per household (census age), car or foot, alert delay, jams on each OSM street.
- **Truth metric:** IoU of the simulated burn area vs the Copernicus EMSR715 perimeter. Held-out second fire (see fix) for the same metric.
- **Data:** [EMSR715](https://mapping.emergency.copernicus.eu/activations/EMSR715) (verified: 3 AOIs, 8 products), [EMSN188](https://mapping.emergency.copernicus.eu/activations/EMSN188), Disasters Charter 859 (CIREN delineations), Sentinel-2 dNBR, [INE census blocks](https://www.ine.gob.cl/herramientas/portal-de-mapas/geodatos-abiertos), [CONAF SIMEF](https://simef.minagri.gob.cl), ERA5, OSM ([Geofabrik](https://download.geofabrik.de/south-america/chile.html)), Copernicus DEM 30 m.
- **Reuse:** [Cell2Fire](https://github.com/cell2fire/Cell2Fire), [C2FK (KITRAL)](https://github.com/fdobad/C2FK), [Fire2a C2F-W](https://fire2a.github.io/docs/Cell2FireW), [WindNinja](https://github.com/firelab/windninja), OSMnx, deck.gl.
- **Fatal flaw:** One event, one perimeter: the IoU ladder can become calibration to the answer. Urban spread is house-to-house and embers, which Cell2Fire models weakly. The evacuation level has no truth. The case is in court (intentional ignition), so "this street is where people die" can turn the jury against us.
- **Fix:** Hold out a second Chilean fire (for example Santa Olga 2017) and show the IoU there too. Keep the metric on the burn scar only. Show the end as the "alert 20 min earlier" counterfactual, not as a death map. Tell it with respect.

## 3. "10 grados": the AI scientist who finds why La Pintana burns and Vitacura does not (critics: 7.5 / 7.5)

- **Problem:** In a Santiago heat wave, poor comunas are up to about 10 C hotter at the surface than rich ones. Nobody can say which cause explains how much of the gap.
- **Story:** Rosa, 78, alone in a block in La Pintana. Satellite heat map: red south, blue east. An AI scientist starts on a flat model. Each loop it writes one hypothesis on screen, adds it, tests it on hidden data, keeps it (green tick) or kills it (red X). Model map and satellite map converge. End: the one change on Rosa's block that drops the most degrees.
- **Wow:** The agent kills the obvious hypothesis ("more trees") because it fails on hidden data. It keeps a hidden one: irrigated green. In a 15-year drought, the east is cool because it is watered. The heat gap is a water gap. The error drops in one jump.
- **Depth ladder:**
  - L0: one city temperature plus altitude lapse rate. One flat color, RMSE about 4-5 C.
  - L1: + vegetation (Sentinel-2 NDVI). First east/south contrast.
  - L2: + impervious surface and albedo. 30 m street patterns.
  - L3: + building height and sky view factor (Open Buildings 2.5D / OSM).
  - L4: + irrigation / soil moisture (NDMI) and Andes valley breeze vs distance to the cordillera. The jump. Then per-block "what if" for Rosa.
- **Truth metric:** RMSE (C) of model surface temperature vs Landsat 8/9 ST_B10 at 30 m on held-out comunas and a held-out date. Second: error in the La Pintana minus Vitacura gap. Target: 4-5 C -> 1-1.5 C.
- **Data:** Landsat C2 L2 on [Planetary Computer](https://planetarycomputer.microsoft.com/dataset/landsat-c2-l2) (verified, no account) or [Earth Engine](https://developers.google.com/earth-engine/datasets/catalog/LANDSAT_LC09_C02_T1_L2). Sentinel-2 L2A. OSM and Google Open Buildings. INE Censo 2017. DMC stations for a check. Prior art: [Sarricolea et al. 2022](https://repositorioslatinoamericanos.uchile.cl/handle/2250/9269144), [Montaner-Fernández et al. 2020](https://opus.lib.uts.edu.au/bitstream/10453/145446/2/remotesensing-12-03345.pdf).
- **Reuse:** [LLM-SR](https://github.com/deep-symbolic-mathematics/LLM-SR) (propose equation, scipy fit, keep or drop by held-out error), rasterio / xarray / stackstac, deck.gl or MapLibre for two maps side by side. Optional UMEP/SOLWEIG for sky view factor.
- **Fatal flaw:** It is a regression on rasters, not a simulation. Weak fit for a Simulations track. LST patterns repeat every summer day, so a hidden day is a weak test. The "irrigated green" surprise is not proved.
- **Fix:** Add a small physical surface energy balance (radiation, evaporation from soil moisture, storage in asphalt) so each hypothesis is a physical term, not a free feature. Hold out comunas, not only days. On day one, check that NDMI gives the jump. If not, the surprise is the mechanism that wins. Say "surface temperature" on stage.

## Recommendation

Build #1 "Seis horas de luz". It is the cleanest match to the key property: each loop adds one physical term, and the held-out hit rate cannot be faked. One Python stack (OpenDrift), one dark-sea visual. #2 has stronger emotion but overfit and legal risk. #3 shows the agent loop best, but it is not a simulation.

## Other ideas

| Idea | Critic A | Critic B | Reason |
|---|---|---|---|
| Rapa Nui: did they destroy themselves? | 7.5 | 5.5 | Best surprise, but the SPD fit stops improving after L2 and the 1862 cliff is scripted. |
| Calbuco 2015 tephra | 7 | 5.5 | Red-to-green field points are a good visual; no deaths, points must be digitized. |
| El planeta que nadie ha visto (ALMA) | 7 | 4.5 | Planet match is near tautological; no human emotion. |
| Space junk over Atacama | 6.5 | 7 | Error drops weeks -> hours, but the 24 Jul 2025 object is not confirmed. |
| La noche de la helada | 6.5 | 6.5 | Cold air that flows is a great visual; emotion is commercial, station data needs scraping. |
| Petorca: tap goes dry (r1) | 7 | 5 | No bulk DGA well data; MODFLOW calibration too slow. |
| Coyhaique / Temuco smoke (r1) | 6.5 | 6 | Plume in a valley inversion fits poorly; few stations. |
| Chañaral 2015 mud flow | 6.5 | 6 | Footprint must be hand-drawn; third disaster story. |
| La señora Rosa del cuarto piso | 6.5 | 4.5 | The wow lives at L3-L4, which cannot be validated. |
| Invierno sin sincicial (r1) | 6.5 | 4.5 | Depth does not lower the error; result already published. |
| Chiloé 2016 salmon drift | 6.5 | 4 | Toxin arrival data not open; Armijo 2020 did it already. |
| El latido de Javiera (ECG) | 6.5 | 5 | Accuracy climb copied from CiPA; deep levels have no truth. |
| El sol que botamos (AI scientist) | 6 | 5 | Grid prices are abstract; CEN data is manual. |
| 39 grados en la manzana | 6 | 4.5 | Duplicate of Rosa with a weaker metric. |
| Quintero 2018 | 6 | 4 | Real toxics (VOCs) not measured; fit cannot rise; legal risk. |
| Las golondrinas de Iquique | 6 | 4 | GBIF records are sightings, not groundings. |
| 8 minutes tsunami (r1) | 6 | 3 | Hypothetical event, no truth. |
| Dos horas para llegar (r1) | 6 | 4 | MATSim heavy; DTPM data needs a request. |
| Maipo glacier (r1) | 5.5 | 5 | Metric is circular; story is slow. |
| Villarrica lake | 5.5 | 4 | Too many chained models for 36 h. |
| El sol que botamos: battery siting | 5.5 | 5 | Cold B2B story; CEN data cost. |
| La tómbola (SAE) | 5.5 | 4.5 | Little depth; location may be anonymized. |
| Sala de espera ER (r1) | 5 | 3 | No public in-hospital time data. |
| San Ramón fault ambulance (r1) | 4.5 | 2.5 | Hypothetical, many chained models. |
| La fila waiting list (r1) | 4.5 | 2 | Aggregated PDFs; death is not caused by the wait. |
| La pensión de Carmen (r1) | 4.5 | 2.5 | Projection, restricted data. |
| Chile en mil voces (r1) | 4 | 3 | LLM knows the result: data leakage. |
