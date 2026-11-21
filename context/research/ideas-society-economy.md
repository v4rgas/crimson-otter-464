# Ideas: society and economy

Research brief. Area: LLM-agent societies, policy, pensions, transport, opinion.
Status: proposal. Humans choose.

## Shared base for all 3 ideas

A synthetic population of Santiago built from real data. Each agent is one person
with an age, a home block (manzana), a job, an income, and a short life story.

- INE Censo 2024 microdata (persons, households) and block (manzana) data + cartography (geoparquet):
  https://www.ine.gob.cl/press-room/news/2025/12/04/ine-has-published-the-database-of-the-2024-population-and-housing-census
  , block data: https://www.radioagricultura.cl/noticias/nacional/ine-publica-bases-de-datos-a-nivel-de-manzana-y-cartografia-del-censo-de-poblacion-y-vivienda-2024_20251216/
  , example code: https://github.com/bastianolea/censo_2024_mapas
- CASEN 2024 microdata (income, poverty, work): https://bidat.gob.cl/directorio/Encuestas%20Observatorio%20Social/casen-2024
- Persona method: Park et al. 2024, "Generative Agent Simulations of 1,000 People".
  Agents from interviews replicate GSS answers at 85% of human self-consistency.
  Agents from demographics only are more biased. https://arxiv.org/abs/2411.10109

How depth grows with agent time (same for all ideas):
1. Coarse: one agent per comuna, built from averages.
2. Finer: agents per manzana, sampled from Censo + CASEN (iterative proportional fitting).
3. Deeper: each agent gets an LLM life story conditioned on its real microdata row.
4. Calibrated: the simulation compares itself with a real observed number and adjusts.
   The error number goes down on screen.

The demo can show this growth live: a counter of agents, a map that goes from
large comuna blobs to many small dots, and an error bar that goes down.

---

## 1. "Dos horas para llegar": the commute of Santiago (public transport)

- **Problem:** People in the periphery of Santiago (Puente Alto, La Pintana, Bajos de Mena)
  lose 2 to 4 hours each day in transport. Planners change bus routes without
  seeing the effect on each person.
- **Story:** Rosa, 52, lives in Bajos de Mena and cleans houses in Las Condes. We follow her
  day. Then we apply one real change (a route cut, a new Metro line 7 or 9). We see
  her day, and the day of 1 million people like her.
- **Wow:** One small route change that looks good on paper (fewer buses, same coverage)
  adds 40 minutes to 80,000 women of more than 50 years in the south. The map turns red
  only in one area. Or the reverse: Metro line 9 gives back X years of life per year to La Pintana.
- **Depth with agent time:** more agents per manzana; each agent gets real trip
  chains from the mobility survey; routing on the real GTFS network; calibration against
  real bus validations per stop. Error vs. real loads goes down live.
- **Data:** GTFS Red/Metro (DTPM): https://mobilitydatabase.org/feeds/gtfs/mdb-3357 ;
  Encuesta de Movilidad de Santiago 2024 (CEDEUS): https://en.cedeus.cl/blog/agenda/lanzamiento-de-la-encuesta-movilidad-de-santiago-ems-2024/ ;
  EOD 2012 (SECTRA, public); OpenStreetMap; Censo 2024 (commute mode question:
  https://www.cnnchile.com/pais/censo-2024-54-personas-utiliza-transporte-publico-para-ir-a-sus-trabajos/).
- **Reuse:** MATSim Santiago scenario (OSM + GTFS + EOD 2012): https://matsim.org/gallery/santiago/ ;
  r5py (fast multimodal routing on GTFS + OSM); AgentSociety (LLM agents in a city, mobility + jobs,
  10k agents): https://github.com/tsinghua-fib-lab/AgentSociety ; deck.gl TripsLayer for the animation.
- **Visual:** dark map of Santiago, 100k moving dots at 6:00 AM, one dot highlighted (Rosa).
- **Main risk:** EMS 2024 microdata may not be public yet (use EOD 2012 + Censo 2024).
  Bus validation data per stop may need a request to DTPM.

## 2. "La pensión de Carmen": the pension reform, life by life

- **Problem:** Ley 21.735 (2025) changes pensions. The averages look good. Nobody sees which
  real lives still fall below the poverty line, and why (gaps in contributions, care work, informality).
- **Story:** Carmen, 34, works informally and takes care of her children. We fast-forward her life
  to 65 with and without the reform. Then 1 million lives like hers run at the same time.
- **Wow:** Lives run as lines that fall into a pension bar. The reform lifts the average,
  but one cluster stays below the line: women with long gaps. We click one line and Carmen
  "speaks" her life story in first person, with each gap and its reason.
- **Depth with agent time:** more lives simulated; each year of each life is an LLM decision
  (work formal/informal, care, migration) calibrated to real transitions from the EPS panel;
  outputs calibrated to real pension amounts by sex and age. Error vs. real distribution goes down live.
- **Data:** EPS panel (work histories, admin pension data): https://previsionsocial.gob.cl/?p=824 ;
  Nota Técnica Ley 21.735 (parameters): https://previsionsocial.gob.cl/wp-content/uploads/2025/08/Nota-Tecnica-Reforma-de-Pensiones-Ley-N°21.735.pdf ;
  CASEN 2024; Superintendencia de Pensiones statistics: https://spensiones.cl
- **Reuse:** Concordia (generative ABM, "game master" decides life events):
  https://github.com/google-deepmind/concordia ; pension formula is simple arithmetic (write it).
- **Visual:** thousands of lines that flow left to right (age 20 to 65), color by sex, end in a histogram.
- **Main risk:** EPS admin data requires a signed use form. Self-reported EPS is open. Less "simulation", more "projection".

## 3. "Chile en mil voces": a synthetic Chile that votes (opinion)

- **Problem:** Polls in Chile failed in 2022 (plebiscite: 62% Rechazo, polls showed a close result).
  Governments design policies without asking the people that the policy affects.
- **Story:** We build a synthetic Chile, comuna by comuna. We ask it the 2022 plebiscite question
  and compare with the real SERVEL result per comuna. Then we ask it a new question
  (for example a new policy) and watch how opinion spreads in neighborhood conversations.
- **Wow:** The map of predicted votes appears comuna by comuna, then the real SERVEL map
  appears on top and they match. Then a new policy runs live and one comuna changes color.
- **Depth with agent time:** more agents per comuna; richer personas (Park interview method);
  agents talk with neighbors (social network); calibration on past elections, so error per
  comuna goes down live on screen.
- **Data:** SERVEL results per comuna/mesa (servel.cl, public results files); Censo 2024; CASEN 2024;
  CEP survey microdata (cepchile.cl, public).
- **Reuse:** OASIS (1M LLM agents, opinion spread, polarization): https://github.com/camel-ai/oasis ;
  silicon sampling papers: https://arxiv.org/abs/2411.03486
- **Visual:** map of Chile, comunas fill with color, live error bar.
- **Main risk:** data leakage. The LLM knows the 2022 result, so the backtest is not honest.
  LLM answers are homogeneous and biased by stereotype (https://web3.arxiv.org/pdf/2507.02919).
  Politically sensitive.

---

## Recommendation

1 is the strongest: concrete person, real network data, clear motion on a map,
and depth (manzana, trip chains, calibration) is easy to show. 2 is the most emotional.
3 has the best "wow" but the weakest honesty (leakage).
