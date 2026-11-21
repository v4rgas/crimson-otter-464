# Ideas: health and biology

Area brief from `research-first`. Three ideas, best first.
Note: links to DEIS urgency data are to catalog pages. Confirm the exact CSV before you build.

## 1. "Invierno sin sincicial" — respiratory epidemic in Santiago

- **Problem:** Each winter, the respiratory virus VRS fills pediatric beds in Chile. Babies die.
- **Story:** A baby, Agustina, is born in Puente Alto in April 2023. We follow her through the winter. The city map fills with red dots. The hospital near her house fills up. Then we replay the same winter with 2024 policy (nirsevimab for all newborns).
- **Wow:** The same city and the same virus. One dose changes the winter: the hospital never saturates. Real result: about 77% fewer VRS cases, and 2024 was the first winter with zero reported VRS infant deaths (NIRSE-CL, Lancet Infect Dis). The simulation finds this result itself, from data.
- **Depth grows with agent time:**
  1. Comuna-level SEIR with age groups.
  2. Synthetic population: households, schools, salas cuna from the census.
  3. Mobility from the Origen-Destino survey and metro flows.
  4. Agents calibrate the model to weekly DEIS and ISP curves. The error line between the model and the real curve gets smaller on screen.
  5. Each hospital has a bed count and a queue.
  Demo: a "resolution" slider (v1 to v5). The model curve moves onto the real 2023 curve.
- **Real data:**
  - Nirsevimab results: <https://pubmed.ncbi.nlm.nih.gov/40513593/>, <https://repositorio.uchile.cl/handle/2250/209569>, <https://www.minsal.cl/campana-de-invierno-2024-inmunizacion-con-nirsevimab-redujo-un-816-la-hospitalizacion-por-causa-respiratoria-en-menores-de-un-ano/>
  - DEIS urgency data (respiratory causes per epidemiological week, SADU): <https://data.amerigeoss.org/dataset/?tags=DEIS>, <https://datos.gob.cl>
  - MINSAL weekly winter campaign reports: <https://www.minsal.cl/wp-content/uploads/2026/06/12-Reporte-Semanal-Campana-de-Invierno-18-06-2026_.pdf>
  - ISP respiratory virus surveillance (weekly VRS positivity): ispch.cl, "Informe de circulación de virus respiratorios".
  - INE Census 2017/2024 (age per comuna and manzana): <https://www.ine.gob.cl>
  - Encuesta Origen-Destino Santiago 2012 (SECTRA).
- **Libraries / prior art:** Starsim (Python ABM, RSV already modeled): <https://pypi.org/project/starsim/>. EpiModel RSV multilayer example: <https://epimodel.github.io/EpiModel-Gallery/examples/rsv/>. RSV ODE models: <https://github.com/abhogan/rsv_models>. Santiago ABM with 40 comunas: <https://arxiv.org/pdf/2005.09556>. ISCI microsimulation: <https://isci.cl/en/covid19/pandemic-progress-microsimulation-model/>. Frontend: deck.gl on a Santiago map.
- **Main risk:** Calibration in 36 h. Fallback: the counterfactual on the comuna-level model only.

## 2. "Sala de espera" — digital twin of one Chilean emergency room

- **Problem:** In winter, patients wait many hours in the public urgency rooms. Some die on a stretcher.
- **Story:** Don Luis, 78, arrives at 21:00 at Hospital Sótero del Río with pneumonia. We see each patient as a dot that moves: triage, box, bed, exit. His dot does not move for 9 hours.
- **Wow:** The agent finds that the bottleneck is not doctors but beds. One small change (for example, discharge before noon) makes Don Luis wait 2 hours, not 9. The room "empties" on screen.
- **Depth grows with agent time:**
  1. One queue with one arrival rate.
  2. Real arrival curve per hour and per day from DEIS.
  3. ESI triage levels, boxes, staff shifts.
  4. Inpatient beds: occupancy and discharges.
  5. Calibration of each hospital, then all the network of one Servicio de Salud (patients go to the next hospital).
  Demo: the twin of the floor plan gets more rooms and more rules. The simulated wait time moves onto the real one.
- **Real data:**
  - DEIS daily urgency care per establishment: <https://data.amerigeoss.org/dataset/?tags=DEIS>, <https://datos.gob.cl>
  - Establishment master table (codes, location): <https://datos.gob.cl/dataset/3bf4cf7c-f638-4735-9a01-f65faae4beca/resource/7127fc99-b858-4e74-8971-51413e484fb1/download/diccionario-de-datos.pdf>
  - DEIS bed counts (camas) per establishment: deis.minsal.cl.
  - Weekly urgency reports: <https://www.minsal.cl/reporte-de-virus-respiratorios-atenciones-de-urgencia-disminuyeron-un-93-respecto-de-la-semana-anterior/>
- **Libraries / prior art:** SimPy / Ciw (discrete-event queues). EDSim (LLM agents in an ED): <https://ohotnik.duckdns.org/denoslab/EDSim>. ED ABM on MIMIC-IV: github.com/rddelosreyes/ed-abm-synthetic-ehr. Paper: <https://arxiv.org/pdf/2610.30148>, ICCS 2025: <https://www.iccs-meeting.org/archive/iccs2025/papers/159060041.pdf>.
- **Main risk:** No public data on time inside the hospital (triage to bed). We must use published averages and papers for that layer.

## 3. "La fila" — the national waiting list

- **Problem:** About 40,000 people per year die in Chile while they are on a public waiting list (MINSAL, 2021).
- **Story:** Rosa needs a hip operation. She is number 12,431 in the list of her Servicio de Salud. We see the national list as a river of people that moves slowly; some dots go grey.
- **Wow:** The same number of operations, in a different order (by risk, or by moving patients between services), gives fewer grey dots. The audience sees lives come back.
- **Depth grows with agent time:**
  1. One queue per Servicio de Salud.
  2. Queues per specialty and per establishment.
  3. Individual patients with age and diagnosis risk.
  4. Survival curves per diagnosis from papers.
  5. Transfers between services with travel time.
- **Real data:**
  - Glosa 06 quarterly reports (lists per establishment, deaths while waiting): <https://www.minsal.cl/wp-content/uploads/2026/08/Glosa-06-LE-II-trimestre.pdf>, <https://www.minsal.cl/wp-content/uploads/2026/07/Glosa-06-letra-a-b-c-i-j-k-comun-a-la-partida-1er-trimestre-1.pdf>
  - CIPS-UDD analyses: <https://gobierno.udd.cl/cips/files/2026/07/informe-cips-lista-espera-ges-y-no-ges.pdf>, <https://gobierno.udd.cl/cips/files/2026/03/lista-de-espera-2020-2025.pdf>
  - Deaths while waiting: <https://www.elmostrador.cl/noticias/pais/2024/01/12/minsal-reporto-que-44-mil-pacientes-murieron-en-2022-mientras-estaban-en-listas-de-espera/>
- **Libraries / prior art:** SimPy; IDB study on waiting-list determinants: <https://publications.iadb.org/publications/spanish/document/Determinantes-de-la-espera-por-atencion-de-especialidad-una-propuesta-de-optimizacion-de-la-gestion-de-las-listas-de-espera-en-salud-en-Chile.pdf>
- **Main risk:** Data is in PDFs and aggregated. No public data per patient. MINSAL says that a death on the list is not proof that the wait caused it. The claim "lives saved" is weak.

## Discarded

- Defibrillator (DEA) placement in Santiago: strong story, but Chile has no public DEA registry and no out-of-hospital cardiac arrest registry.
