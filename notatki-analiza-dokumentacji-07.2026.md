# Analiza dokumentacji klienta (rev. 07.2026) + plan v0.8 — ZAAKCEPTOWANY

Notatka robocza dla Claude Code (odporna na zmianę modelu/streszczenie kontekstu).
Źródło: 5 PDF od Krzyśka (13.07). UWAGA: czekamy jeszcze na XLSX — wciągnąć do tej samej struktury.
Dokument sam zastrzega: czasy linii B = założenia inżynierskie do walidacji w B+R (M1–M18).

## Wspólny mianownik
wsad 20 t/d złomu LAB · 312 dni/rok → 1455 kWh ogniw/dobę (454 MWh/rok) · zużycie Pb 8,1 kg/kWh.
Lead time partii 75 h (2,5–4 doby); po pakiecie optymalizacji 49 h i 481 MWh/rok (330 dni).

## HALA (zastępuje robocze 96×48!)
156 × 72 m · 11 230 m² brutto · procesy 6 800 m² · dach PV ~1 MWp · światło użytkowe +8,00 m
(suwnice/AS-RS), attyka +10,50, komin B4 +18, skruber +12, wyrzutnia H₂ +13, biura 2 kond. +7,50.
Przepływ: przyjęcie od E (doki przyjęcia, waga 60 t), wysyłka na W — jednokierunkowo, bez krzyżowań.
Brama A8 + śluzy = separacja strefy Pb (brudnej, podciśnienie+HEPA) od czystej. Korytarz AGV/AMR
= kręgosłup (MES/LIMS genealogia partii). Strefy ATEX: EtOH (B1/B2/B3).

## KATALOG URZĄDZEŃ/STREF — pełna podmiana (decyzja Krzyśka: stare 10 W-draft wyrzucamy)
Pozycje z planu hali (metraż · pozycja na siatce 10 m z rzutu 1/2, hala 156×72, N↑, 0,0 = NW róg):
LINIA A (strefa brudna, niebieskie): A1 Przyjęcie 400 m² (x≈130–155, y≈0–15) · A2 Breakowanie+separacja
350 m², 1,35 t/h (1,2–1,5), takt 2,25 t/h po optym. (x≈105–130, y≈0–15) · A3 Elektrolit 150 m² (x≈133–155,
y≈15–26) · A4 Piec topielny + hot-to-product 300 m², 6,2 t/d Pb, 12 h/d (x≈105–133, y≈15–30) ·
A5 Hydrometalurgia 500 m², 7,0 t/d→PbO, PRACA CIĄGŁA — wąskie gardło kalendarzowe (x≈83–105, y≈0–21) ·
A6 PP mycie+regranulacja 300 m², 0,3 t/h → 2,4 t/d (x≈83–105, y≈21–30) · A7 EHS odpylanie·skruber 250 m²
(x≈65–83, y≈0–17) · A8/LAB CQ 150 m² ICP-OES·XRD·BET (x≈65–83, y≈17–27).
MAGAZYNY (brama A→B): A8 Magazyn surowców+CQ 400 m² Pb·PbO·H₂SO₄·PP (x≈85–125, y≈30–40) ·
B1 Magazyn linii B 400 m² pianka·żywice·separatory·SiO₂ (x≈125–155, y≈30–40, ATEX).
LINIA B (strefa czysta, zielone/czerwone/żółte): B2 Żywice rektyf. EtOH 150 m² ATEX (x≈143–155, y≈40–56) ·
B3 Impregnacja PU roll-to-roll 350 m² ATEX, 0,75 h, suszenie 140°C+odzysk EtOH, CUSTOM (x≈128–155, y≈56–72) ·
B4 Piroliza LT 250–900/HT 1200–1400 °C · N₂ · 450 m² · 24/7 — DYKTUJE TAKT, bufory WIP 8–12 h, 4,5 h
(x≈108–128, y≈43–72) · B5 Galwanika Pb ~100 µm @2–3 A/dm², 1,5 h, 300 m² (x≈95–108, y≈43–60) ·
B6 Szkielet+kontakty 200 m², 0,3 h, takt 1–2 min/szt, CUSTOM (x≈95–108, y≈60–72) · B7 Pasty (R7) 150 m²,
0,75–1 h/szarża, 2–3 szarże/d ≈7 t past/d, HEPA (x≈85–95, y≈40–52) · B8 Pastowanie dwustronne 250 m²,
0,25 h, CUSTOM (x≈70–85, y≈52–65) · B8b Curing komory T/RH 350 m², 36 h (24–48), po optym. fast-cure
65–85 °C 22 h; komory ≥1,5× dobowej produkcji płyt (x≈57–70, y≈43–60) · B9 Strapy COS + B10 kopertowanie
300 m² (x≈57–70, y≈60–72) · B11 Jar assembly 100 m², zgrzew+szczelność (x≈48–57, y≈60–72) · B12 Zalewanie/
żelowanie 150 m², 4 h (2–6), po optym. 1,5 h w buforze kolejki (x≈48–57, y≈43–52) + bufor stabilizacji żelu ·
B13 Formacja wanny 316L 700 m², 24 h (18–30), po optym. z recyrkulacją 14 h, 24/7, wentylacja H₂+detekcja,
wanny ≥1,0–1,25× dobowej produkcji; DOSTAWCY z dok.: Inbatec, BM Rosendahl, profile IUI (x≈2–25, y≈40–72) ·
B14 Testy EoL+pakowanie 300 m², 3 h, SoC/SoH (x≈15–40, y≈0–15).
POZOSTAŁE: Ekspedycja (x≈0–15, y≈0–15) · Magazyn wyrobów 600 m² (x≈0–40, y≈15–30) · Zaplecze techniczne
700 m² sprężarkownia·demi·warsztat UR·rozdzielnie (x≈40–65, y≈0–30) · Korytarz/śluza czysto↔formacja
(x≈25–48, y≈40–72) · bufor WIP przy B4 (x≈108–128, y≈40–43).
TEREN (poza halą): silos Na₂CO₃ · oczyszczalnia ścieków+zbiornik ppoż./retencja · chłodnie wentylatorowe
(formacja·piroliza) · zbiorniki H₂SO₄ retencja 110% (rurociągi dwupłaszczowe z detekcją) · biura+socjal 2 kond.
~10 os. adm., szatnie czysto-brudno · skruber formacji (aerozole+H₂) · BESS (bufor PV/formacji) · trafo 2×SN/nn
+rozdzielnia gł. · zbiornik N₂+parownica · waga 60 t · portiernia · wiata bufor złomu · parking · plac manewrowy.

## Czasy linii B (balans): B3 0,75 (0,5–1) · B4 4,5 (3–6) · B5 1,5 (1–2) · B6 0,3 · B8 0,25 · B8b 36 (24–48)
· B9–11 0,75 · B12 4 (2–6) · B13 24 (18–30) · B14 3. Równoległe poza ścieżką: B2 żywice 2–4 h/szarża, B7.

## Wąskie gardła: A2 zmianowe (93% obłożenia 2 zmian) · A5 kalendarzowe (ciągła, limituje 312 d/r) ·
B4 taktowe (24/7, 1 h postoju = −4,2% produkcji dobowej; bufor 4–8 h taktu).

## Pakiet optymalizacji (ΔCAPEX razem +2,85 mln zł ≈ +8% CAPEX A+B → CAPEX A+B ≈ 35 mln zł):
1. A2 breaker 2,25 t/h (+0,4) → 20 t/d w 1 zmianę, rezerwa do 30 t/d (faza II) · 2. A5 reaktor N+1+bufor pasty
~50 m³ (+0,55) → 312→330 d = +5,8% · 3. B4 bufory WIP+mufla sekcyjna (+0,4) → postój ≤8 h bez strat, +20%
zapas taktu · 4. B8b fast-cure (+0,3) 36→22 h · 5. B13 formacja z recyrkulacją+chłodzeniem (+1,2) 24→14 h,
energia −25–30% (Inbatec/BM Rosendahl) · 6. B12 żelowanie w buforze (0) 4→1,5 h. Efekt: 75→49 h, −40% WIP,
454→481 MWh/rok, ścieżka do ~580 MWh/rok (faza II).

## Bilans ciepła (str. 2/2) — dane pod M2/M3:
ODZYSK: B4 spaliny 100–180 kW 400–700°C→suszarnia B3 (60–100 kW)+CO hali · B2/B3 kondensacja EtOH
20–40 kW ~80°C→CWU · A4 15–25 kW 12h/d 200–350°C→CO · A5 opary 30–60 kW→podgrzew wody · sprężarkownia
20–30 kW ~70°C→CWU · B13/B5 niskotemp. 60–120 kW 30–45°C→pompa ciepła→curing. RAZEM ~250–450 kW śr.
= 1,6–2,6 GWh/rok ≈ 0,5–0,9 mln zł/rok. CHŁÓD: B13 60–80 · B5 20–50 · B4 30–60 · A4/B9 20–40 · A6+B7 40–70
= 170–300 kW szczyt (chłodnie+free-cooling ~6 mies./rok). Rekomendacja: pompa ciepła COP 4–5 (formacja/
galwanika 30–45°C → curing 65–85°C); nadwyżka B4 → moduł PCM/M-TES (technologia własna 21 zmysłów) na
szczyt CO (zima ~280–390 kW). Zakład netto dodatni cieplnie poza szczytem zimowym. Instalacje: NW-1..4
centrale dachowe z odzyskiem, ring wody chłodzącej, ring sprężonego powietrza 38 m, woda demi.

## PLAN v0.8 (zaakceptowany przez Krzyśka) — kolejność:
1. PEŁNA PODMIANA katalogu DEVICES na ~20 pozycji wyżej (stare 10 W-draft usuwamy). Nowe pola karty:
   m² docelowe + wymiary z rzutu, czas etapu/takt [h], przepustowość, praca (24/7|2zm|takt), strefa
   (Pb|czysta|ATEX), WIP/bufory, badge „wąskie gardło", ciepło odpad./chłód [kW], dostawcy z linkami
   (z dok.: Inbatec https://www.inbatec.de , BM Rosendahl https://www.bm-rosendahl.com — ZWERYFIKOWAĆ
   HTTP przed wgraniem; ewentualni dalsi oznaczeni „kandydat do RFQ — dobór 21 zmysłów"). Pola bez danych
   w dok. (kW, obsada, CAPEX/urz.) zostają W-draft z oznaczeniem źródła D/W na karcie.
2. Przycisk „Autorozmieszczenie" obok Auto-układu = wierne odtworzenie planu referencyjnego z dokumentacji:
   hala 156×72, wszystkie pozycje wg współrzędnych wyżej, korytarz AGV, brama A8, doki E/W, obiekty terenu.
   (Auto-układ heurystyczny zostaje bez zmian.)
3. Strona startowa: na dole DWA duże przyciski w stylu CTA (biała pigułka+pomarańczowe kółko):
   „⭱ Zaimportuj dane" i „◱ Zacznij od zera"; obecny „Zacznij projektowanie" USUNĄĆ. Mały tekstowy link
   „kontynuuj zapisany projekt →" tylko gdy jest zapis w localStorage (propozycja — czeka na potwierdzenie).
4. NOWA ZAKŁADKA w lewym pasku (rail): „Karty urządzeń" — pełne karty wszystkich pozycji katalogu ze
   wszystkimi danymi, linkami do dostawców, uwagami; katalog w plannerze zostaje kompaktowy (decyzja
   Krzyśka: w plannerze mniej info, szczegóły w zakładce).
5. Widoki: M1 = schemat z prawdziwej topologii (krawędzie z dok., koniec zgadywania z we→wy) ·
   M2 + karta bilansu ciepła · M3 pierwsza realna wersja (PV 1 MWp, BESS, odzysk, pompa ciepła, PCM/M-TES)
   · M6 cash cost na kWh (454/481 MWh/rok) + przełącznik „pakiet optymalizacji +2,85 mln" · KPI przepustowość
   i lead time. Hala domyślna nowego projektu = 156×72.
6. CLAUDE.md + dziennik; wersjonowanie: nowy plik v0.8.

## Status: CZEKAMY na XLSX od Krzyśka przed startem implementacji. Model może się zmienić na Opus — plan
i dane są w tym pliku, transkrypty PDF w /root/.claude/uploads/b32b0717-*/ (kontener ulotny — dane
merytoryczne są wyciągnięte wyżej).
