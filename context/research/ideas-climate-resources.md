# Ideas: climate and resources

Area: water, glaciers, air, energy. 3 best ideas, best first.
Not checked yet: a bulk download for DGA well levels (BNA/SNIA). The portal shows data, but we did not find an API.

## 1. Petorca: the day the tap goes dry

- **Problem:** In Petorca, avocado wells lower the aquifer. Families depend on water trucks. In 2018, 70% of 78,299 people got water from APR systems or trucks.
- **Story:** We follow one APR well and the family that drinks from it. The years pass. Rain drops, irrigated hectares grow, and the water table drops. A water truck arrives.
- **Wow:** The well of the APR goes dry in the simulation on the same month that the DGA bulletin reports dry wells. Then we play "what if": take water from the farms that use the most, and the well stays alive.
- **Depth grows with agent time:** v1 is one aquifer "bucket" per valley. Then a 2D groundwater grid (MODFLOW through FloPy) at 1 km, then 200 m. Then each well with its water right (DGA cadastre). Then each orchard from satellite NDVI, with crop demand per month. Then the river and the rain (CR2MET). Each step, the error against the real well levels gets smaller. **Demo:** a "fidelity" slider that shows the grid become finer and the error curve go down.
- **Data:** [CAMELS-CL](https://doi.pangaea.de/10.1594/PANGAEA.894885) (DGA streamflow, rain), [CR2MET 0.05°](https://zenodo.org/records/7529682), DGA well levels ([boletines](https://dga.mop.gob.cl/uploads/sites/13/2023/07/Boletin-DGA-abril-2022.pdf), SNIA), DGA water rights cadastre, MOP aquifer model ([informe](https://repositoriodirplan.mop.gob.cl/biblioteca/bitstreams/c51a260f-4218-48d3-9721-e421012a0883/download)), water truck orders ([EURE 2022](https://eure.cl/index.php/eure/article/download/EURE.48.145.04/1478/26852)), Sentinel-2 NDVI.
- **Reuse:** MODFLOW 6 + FloPy, the [U. Chile Petorca groundwater thesis](https://repositorio.uchile.cl/handle/2250/199068), deck.gl for the map.
- **Risk:** DGA well data can have gaps or a slow download. Political subject (avocado vs. people): keep it to the data.

## 2. Maipo: the glacier that fills Santiago's glass

- **Problem:** Santiago (7M+ people) takes most of its water from the Maipo river. In dry summers, glacier melt keeps the river alive. The glaciers lose mass every year.
- **Story:** A child in Santiago opens the tap in February. We zoom out: tap, river, glacier. We run the years to 2100.
- **Wow:** "Peak water": glacier melt first goes UP (it helps), then falls off a cliff. The audience sees the year the summer river drops.
- **Depth grows with agent time:** v1 is a degree-day model per basin. Then each of the 800+ glaciers of the upper Maipo, each with its own flowline (OGGM). Then debris cover, snow, and calibration with each glacier's measured mass loss. Then the river routing to the intake of the city. **Demo:** the 3D Andes; glaciers appear one by one as the agents calibrate them, and the error curve goes down.
- **Data:** [DGA Inventario Público de Glaciares 2022](https://dga.mop.gob.cl/inventario-publico-glaciares/) (26,169 glaciers), [CR2MET](https://zenodo.org/records/7529682), [CAMELS-CL](https://doi.pangaea.de/10.1594/PANGAEA.894885), RGI 7, Hugonnet 2021 mass balance (global, per glacier).
- **Reuse:** [OGGM](https://oggm.org) (Python, open source, already used for Maipo: [preprint](https://egusphere.copernicus.org/preprints/2023/egusphere-2023-888/egusphere-2023-888-manuscript-version4.pdf)), CesiumJS or deck.gl terrain.
- **Risk:** Slow story (decades, not hours). Scientists already did it: our value is the visual and the depth live, not new science.

## 3. Coyhaique / Temuco: the smoke that comes down at night

- **Problem:** Wood stoves cause most PM2.5 in south Chile (CR2: 94% of PM2.5 emissions in 2017 came from firewood). Coyhaique is one of the most polluted cities in the Americas. Children with asthma go to the hospital in winter.
- **Story:** A winter evening in Coyhaique. At 19:00, the houses light their stoves. The cold air stays in the valley (thermal inversion). The smoke fills the streets near a school and the hospital.
- **Wow:** The real SINCA station line and the simulated smoke climb together at night. Then we change 2,000 stoves to heat pumps (the real PDA plan) and the smoke over the school goes away.
- **Depth grows with agent time:** v1 is a Gaussian plume per neighbourhood. Then one emission source per house (OSM buildings + census), each family an agent with its schedule and its wood. Then a 3D wind field over the valley terrain. Then calibration against each SINCA hour. **Demo:** the city as dots; each pass adds houses and terrain, and the line comes closer to the real station.
- **Data:** [SINCA](https://sinca.mma.gob.cl) (hourly PM2.5 per station), [CR2 report on air pollution](https://www.cr2.cl/wp-content/uploads/2026/06/Resumen_Informe_Contaminacion_.pdf), [MMA Coyhaique air plan study](https://airecoyhaique.mma.gob.cl/wp-content/uploads/2023/05/Informe-Final-CA.pdf), INE 2017 census (heating), OSM buildings, Copernicus DEM, ERA5.
- **Reuse:** Gaussian plume / puff code (AERMOD concepts, CALPUFF), WindNinja (open source wind on terrain), deck.gl.
- **Risk:** Real 3D physics in 36 h is hard; we need the plume model to look good. SINCA data has gaps.

## Not chosen

- Atacama solar curtailment: good data at the Coordinador, but no person to care about.
- Lithium brine and flamingos: data is weak and contested.
