# Symulator fabryki KLAB — instrukcje dla Claude Code

Pracujesz nad **`Symulator fabryki v0.9.html`** (starsze wersje zostają w repo jako archiwum) — narzędziem do symulacji fabryki akumulatorów KLAB (recykling LAB + produkcja ogniw w obiegu zamkniętym). Zespół: Krzysiek (design/strategia) i Marcin (inżynieria), firma 21 zmysłów. Mów po polsku.

## Zanim zaczniesz — przeczytaj (w tej kolejności)

1. `Symulator fabryki — założenia.md` — architektura, one-way doors, roadmapa
2. `Symulator — język wizualny.md` — design tokens, komponenty, referencje
3. `../START — nowy projekt Cowork/Fabryka akumulatorów.md` — kontekst całego projektu
4. Referencje graficzne: `referencje graficzne/` (styl = jasny dashboard, układ = Magura, kafelki = Movara)

## Twarde zasady (nie zmieniaj bez pytania)

- **JEDEN plik HTML** — zero build stepów, zero zależności z CDN, zero frameworków. Plik musi działać z podwójnego kliknięcia, offline, u każdego. To narzędzie wymieniane przez Google Drive.
- **Katalog urządzeń `DEVICES` = jedno źródło prawdy.** Wszystkie widoki liczą z niego. Nie duplikuj danych.
- **Bilanse statyczne** — NIE implementuj symulacji czasu rzeczywistego / discrete-event. Świadoma decyzja architektoniczna.
- **Dane urządzeń to placeholder (W-draft)** — struktura pól czeka na review Marcina. Nie „poprawiaj" wartości merytorycznych na własną rękę; zmiany danych tylko na wyraźne polecenie.
- **Nie walidujemy chemii klienta** (PbSO₄→PbO, „battery-grade", −52% Pb) — to deklaracje z dokumentacji, tak je opisuj.
- **Wersjonowanie:** przy większych zmianach zapisz jako nowy plik `Symulator fabryki v0.X.html` (poprzednia wersja zostaje), drobne poprawki w bieżącym pliku.
- UI po polsku. Deadline projektu: komplet materiałów 07.08.2026 — „wystarczająco dobre na czas" > „idealne po terminie".

## Styl wizualny (skrót — pełny spec w nocie o języku wizualnym)

Tło `#F2F1EF` · karty białe radius 14–16 px cień `0 2px 12px rgba(0,0,0,.07)` · akcent `#FF4D00` (pomarańcz 21 zmysłów) · linia B na kafelkach `#7C3AED` · etykiety małe szare uppercase · liczby duże cienkie · media: energia `#F97316`, woda `#3B82F6`, N₂ `#9CA3AF`, kwas `#8B5CF6` · czarne przyciski-pigułki · ŻADNEGO dark mode na razie.

## Stan obecny (v0.10)

Nowe w v0.10 — trzy żądania Krzyśka: **(1) „Optymalizuj" = 3. przycisk w górnym rzędzie kanwy** obok „Autorozmieszczenie" i „Auto-układ" (`#optBtn`, zielony), a nie w panelu. **(2) Zero nakładek — gwarancja konstrukcyjna**: `autoLayout` przebudowany na **model pasmowy** (strefy w górnym paśmie, urządzenia w pasmach linii, ciągi TYLKO poziome, wstawiane w przerwach między rzędami) — wszystkie prostokąty to poziome pasy o rozłącznych zakresach y, więc z definicji nic się nie krzyżuje: ani ciąg z ciągiem, ani ciąg z maszyną, ani maszyna z maszyną, ani maszyna ze strefą. `packBand(..., {lastAisle})` — po ostatnim rzędzie linii nie dubluje ciągu bocznego; między liniami wchodzi jeden ciąg główny (`glowny()`). `collisions()` sprawdza też korpus maszyny vs strefa. **(3) Centralny przycisk „⤢ Powiększ halę"** (`#growBtn`) — pojawia się na środku, pod rzędem przycisków, tylko gdy coś wystaje poza obrys hali; w raporcie panelu bliźniaczy `#growInline` („Powiększ halę teraz"). `enlargeToFit()` powiększa halę tak, by zmieścić wszystkie urządzenia/ciągi/strefy (zaokrągla w górę do parzystej). **(4) Optymalizuj ma ręce i nogi** — używa strategii **lean I (kolejność procesu)**: urządzenia w kolejności z łańcucha we→wy, linia A nad linią B, każdy rząd przylega do ciągu komunikacyjnego, strefy zachowane jeśli były; raport zielony „Zoptymalizowano (lean, kolejność procesu zachowana)" z obrysem przed/po. Trzy propozycje (Kompakt/Lean I/Komórka U) i `trialLayout` bez zmian. Audyt Playwright (`verify10.mjs`): 40× PASS, dd/dz/cd/cc=0 dla autorozmieszczenia, optymalizacji i wszystkich 3 propozycji. Uwaga: model pasmowy jest mniej gęsty niż ręczny plan referencyjny — świadomie stawia czyste ciągi + kolejność procesu + zero nakładek ponad maksymalne upakowanie, więc przy pełnym programie stref w 156×72 pokazuje deficyt i proponuje „Powiększ halę".

## Stan poprzedni (v0.9)

Nowe w v0.9: intro — nagłówek zmieniony na krótki opis („Symulator fabryki KLAB — planujesz halę, media, personel i koszty…", słowa jako osobne węzły z realnymi spacjami), reszta bez zmian. **Auto-układ przebudowany pod redukcję strat miejsca**: pakowanie z obrotem urządzeń do poziomu (landscape) + ciaśniejsze rzędy (gap 0,6 m, realny ciąg co 3. rząd zamiast po każdym). „Pokaż 3 propozycje układu" liczy 3 warianty (Kompakt — sort po wysokości/min. straty; Lean I — przepływ; Komórka U) i pokazuje dla każdego metrykę (głębokość rzędów, rezerwa, deficyt), oznacza NAJLEPSZĄ, każdy z „Zastosuj". Przycisk **„Optymalizuj obecny układ"** zagęszcza to, co już stoi (obrót + ciasne rzędy, bez programu stref), raportuje odzyskane m². `trialLayout` = dry-run metryk bez zatwierdzania. Pas mediów w auto-układzie zmniejszony (media na dachu wg dok.). Uwaga: naiwne pakowanie w rzędy nie mieści 23 urządzeń + pełnego programu stref w 156×72 tak dobrze jak ręczny plan referencyjny (Autorozmieszczenie) — auto-układ uczciwie pokazuje deficyt + monit powiększenia.

## Stan poprzedni (v0.8)

**Pełny katalog z dokumentacji klienta (rev. 07.2026):** DEVICES = 23 karty procesowe A1–A8, B1–B14, B8b (pełna podmiana — stare 10 W-draft usunięte). Każda karta ma pola plannera (kompaktowe: m², wymiary, tryb, przepustowość, strefa Pb/czysta/ATEX, badge wąskie-gardło/krytyczne, `ref` = pozycja w planie referencyjnym) oraz obiekt `karta` (opis, wejścia/wyjścia, urządzenia ze specyfikacją, media, odpady z kodami, BHP, dostawcy EU/PL + Chiny/Tajwan z **linkami** — zweryfikowane HTTP; Genox/TEKMAX bez linku). Moc (D-szac ⚠) i CAPEX (W-draft, skalowane do ~35 mln zł A+B z dok.) — do zastąpienia ofertami RFQ. Domyślna hala = **156×72 m** (11 232 m²).

**Autorozmieszczenie** (pomarańczowy przycisk obok Auto-układu) = wierne odtworzenie planu referencyjnego z dokumentacji: hala 156×72, wszystkie 23 urządzenia wg współrzędnych `ref`, strefy zaplecza, korytarz AGV. Auto-układ heurystyczny (lean/SLP) zostaje.

**Nowa zakładka w lewym pasku „Karty urządzeń"** (`renderKarty`) — pełne karty procesowe wszystkich pozycji z nawigacją chipami, dostawcami z linkami; katalog w plannerze pozostaje kompaktowy (karta po prawej ma przycisk „Pełna karta →"). **M3** = pierwsza realna wersja (PV 1 MWp, odzysk ciepła 250–450 kW, pompa ciepła COP 4–5, PCM/M-TES — z bilansu ciepła dok.). **M6** = cash cost zł/kWh (z 454/481 MWh/rok) + rozbicie CAPEX wg kategorii HRF (STEP/Cleantech: grunt/budowlane/środki trwałe/WNiP). Menu ⋮ → **eksport „Zadanie 3 · środki trwałe" (CSV w układzie HRF)**. Intro: dwa przyciski CTA „Zaimportuj dane"/„Zacznij od zera" (usunięto „Zacznij projektowanie"), stopka © 21 zmysłów LAB. Obiekt `DOK` = dane zbiorcze z dokumentacji (wsad 20 t/d, 1455 kWh/d, lead time 75/49 h, CAPEX itd.).

Dokumentacja klienta (5 PDF + XLSX kart procesowych) — wyciąg w `notatki-analiza-dokumentacji-07.2026.md`. XLSX „Harmonogram rzeczowo-finansowy STEP/Cleantech" = pusty szablon wniosku (struktura kosztów, ≥3 oferty/wydatek, deadline 31.12.2030).

## Stan poprzedni (v0.7)

Nowe w v0.7: **strona startowa** (intro w tym samym pliku: splash w akcencie, wielki napis KLAB, izometryczna hala z efektem „latarki" — kursor odsłania zapełnioną wersję; przyciski „Zaimportuj dane" (.json z eksportu) / „Zacznij od zera" / „Zacznij projektowanie"; stopka © 21 zmysłów LAB). Grafiki intro to generowane SVG-placeholdery — **sloty `INTRO_IMG_PUSTA` / `INTRO_IMG_PELNA`** w skrypcie przyjmą data-URI obrazów od Krzyśka (ten sam kadr, pusta vs zapełniona hala). Do tego: **monit automatycznego powiększania hali** (przy deficycie w Auto-układzie przycisk „Tak, powiększ" przelicza układ ponownie; przy urządzeniu poza obrysem chip kolizji dostaje „Powiększ halę…" z confirmem), **wymiar hali w pasku KPI** (karta „Hala (wymiar roboczy)"), wymiar hali zapisywany w localStorage/eksporcie/undo, przyklejony nagłówek panelu Auto-układu (krzyżyk widoczny przy scrollu). Bez Google Fonts i zewnętrznych zasobów — plik dalej offline.

## Stan poprzedni (v0.6)

Nowe w v0.6 — **program powierzchni (deliverable M5)**: Auto-układ generuje opcjonalnie **strefy** (przyjęcie+magazyn surowca z dokami przy lewej ścianie, magazyn wyrobów+wysyłka z dokami przy prawej, pas zaplecza przy górnej: socjal+śluzy Pb · QA/UR · technika · strefa kwasów) z metrażami edytowalnymi w panelu (domyślne = W-draft do kalibracji, np. surowiec z „20 t/d") oraz **pętlę obwodową ciągów** (pionowe odcinki główne przy strefach dokowych + dolny odcinek nad pasem mediów) zamiast samych pasów. Strefy są obiektami stanu: przesuwalne na planie, wymiary edytowalne w karcie po prawej, usuwalne (Del), w undo/eksporcie/localStorage, rysowane też w izometrii 3D jako plamy posadzki. Po rozstawieniu panel pokazuje **bilans powierzchni** (produkcja/strefy/komunikacja/rezerwa, z deficytem gdy program się nie mieści — wymiary hali 96×48 to założenie robocze, realne = pytanie do klienta). KPI „Zabudowa" pokazuje rozbicie prod/strefy/ciągi.

Nowe w v0.5: **Auto-układ** (przycisk na kanwie) — deterministyczna heurystyka lean/SLP rozstawiająca urządzenia z planu: 3 strategie (przepływ liniowy I wg łańcucha we→wy per linia, komórka U/serpentyna, grupowanie wg modułów), między rzędami generowane **ciągi komunikacyjne** (główny/boczny/pieszy, szerokości edytowalne, domyślnie 4/3/1,2 m wg ogólnych zasad BHP) rysowane na planie i zapisywane w stanie (undo działa), opcja uzupełnienia planu o brakujące urządzenia z katalogu, po rozstawieniu **lista zaleceń infrastrukturalnych wg norm** (PN-EN 15154 przy kwasach, czujniki O₂ przy N₂, rozdzielnia >1 MW itd.) — jako zalecenia, NIE fizyczne urządzenia (zasada „zero urządzeń spoza dokumentacji" trzyma). Do tego: podgląd obrysu urządzenia na kanwie podczas przeciągania z katalogu, licznik sztuk w prawym górnym rogu kafelka, podpięcia mediów z kryciem 25% (klasa `mconn`), zakładki dwuwierszowe (kod M nad nazwą).

Nawigacja v0.4 (decyzja Krzyśka 13.07): **górne taby = moduły oferty M1–M6** (M1 · Schemat ciągu → widok procesu, M2 · Bilans mediów, M3 · Źródła energii → placeholder, M4 · Zatrudnienie, M5 · Layout hali → kanwa 2D, M6 · Koszty), **lewy pasek ikon = 4 widoki robocze**: układ hali 2D, schemat procesu, zestawienie modułów (wszystkie użyte karty pogrupowane per moduł z sumami), widok 3D hali. Widok procesu (M1) budowany automatycznie z pól `we→wy` (strzałka ciągła = dopasowanie dokładne, przerywana = przybliżone; linia B łańcuchuje się w całość, linia A ma jawne przerwy — brak podziału frakcji breakera w danych). Widok 3D = **statyczna izometria SVG** (bez WebGL/zależności), wysokości brył = założenie robocze 3 m do czasu pola `wys` w schemacie.

Działa: layout drag&drop (siatka 0,25/0,5/1 m/wył., obrót R, kolizje, pan/zoom, undo/redo Ctrl+Z/Y, duplikowanie), KPI live (urządzenia, moc, media, personel, powierzchnia, CAPEX), magistrale mediów wewnątrz hali (chipy E/W/N/KW + punkty przyłączy), strefy serwisowe ze znacznikiem „S", widok M2 (bilans per moduł funkcjonalny), widok M4 Personel (model zmianowy 1/2/3 per urządzenie — założenie robocze, domyślnie 3 zmiany), widok M6 Koszty (OPEX z suwakami, stacked bar struktury), eksport/import JSON, localStorage (klucz `klab_sym_v02`, czyta też `klab_sym_v01`).

Layout v0.3 (wg makiety Krzyśka z 13.07): lewy pionowy pasek ikon = przełączanie widoków (zsynchronizowany z zakładkami u góry), pełnoekranowa kanwa hali z linijkami wymiarowymi i obrysem architektonicznym, **katalog urządzeń = pływający panel po lewej** (zwijany na X; karty wg referencji Movara: ID + pigułka „n× na planie", ilustracja w prawym górnym rogu, przepływ we→wy z paskiem obciążenia, stopka ⚡/▭/👤), **karta wybranego urządzenia = pływający panel po prawej** (podpis „Wybrana karta urządzenia"; akcje Obróć/Duplikuj/Usuń dla urządzeń z planu), pionowy pasek narzędzi kanwy przy prawej krawędzi (undo/redo, siatka, zoom, dopasuj, media, strefy), na dole pasek modułów funkcjonalnych z planu (bez strzałek przepływu — topologii procesu nie ma w danych; klik filtruje katalog) + legenda mediów, w top-barze „Zapisz projekt (JSON)" + menu ⋮ (import, otwieranie starszych wersji). Grafiki urządzeń renderują się też w kartach na planie (zagnieżdżone SVG — uwaga: selektor `#svg` w CSS musi zostać zawężony, globalne `svg{width:100%}` psuje zagnieżdżone grafiki).

Czego celowo NIE przeniesiono z makiety (wymagałoby wymyślenia danych): strzałki przepływu między urządzeniami i numeracja etapów (brak topologii procesu w schemacie), pola wys./ciepło odpadowe/tagi ATEX (schemat czeka na review Marcina), „Edytuj parametry" (dane W-draft zablokowane do review).

Grafiki: `GFX` = schematy kreskowe (używane na planie hali), `GFX2` = szare „rendery" wektorowe (karty katalogu + karta urządzenia). Oba komplety to placeholdery — zdjęcia realnych urządzeń NIE mogą być hotlinkowane (plik offline) ani brane z sieci bez licencji (repo publiczne; na Wikimedia Commons brak sensownych zdjęć tego typu urządzeń — sprawdzone 13.07). Docelowo: materiały od dostawców z pakietów RFQ Marcina, wgrane jako data-URI w miejsce `GFX2`.

## Backlog (kolejność wg roadmapy — iteracja 3)

1. **Widok M3 Energia:** scenariusze źródeł (sieć/PV/odzysk ciepła/kogeneracja/magazyn KLAB) — CZEKA na dane z modułu M3, nie wymyślaj założeń
2. Po review Marcina: podmiana katalogu DEVICES na pełny (~15 pakietów) + ewentualne nowe pola schematu
3. Strefy hali: rysowanie prostokątów „od zera" ręcznie (generator + edycja są od v0.6); eksport rzutu do PNG/PDF (deliverable M5)
4. M6: cash cost na jednostkę produkcji (czeka na wolumeny linii A i B — pytania do klienta) · M4: narzut urlopowo-chorobowy i personel pośredni (UR, logistyka, QA)

## Czego NIE robić

Pełne 3D (WebGL, silniki, obracanie kamerą) — widok 3D wyłącznie jako statyczna izometria SVG (złagodzenie zasady na polecenie Krzyśka 13.07) · animacje ozdobne · dark mode · live-collab · optymalizacja layoutu solverami/metaheurystykami — dozwolona wyłącznie deterministyczna heurystyka lean/SLP w Auto-układzie (złagodzenie na polecenie Krzyśka 13.07) · dodawanie urządzeń spoza dokumentacji klienta (potrzeby normowe tylko jako lista zaleceń/strefy) · rozszerzanie zakresu poza 6 modułów oferty (M1–M6).

## Po sesji

- Dopisz iterację do sekcji „Dziennik decyzji" w `../START — nowy projekt Cowork/Fabryka akumulatorów.md` (jedna linia: co się zmieniło).
- **Po każdym wgraniu (push) ZAWSZE podaj na końcu odpowiedzi link do podglądu:**
  `https://twentyonelab.github.io/klab/`
  GitHub Pages jest skonfigurowane w trybie „Deploy from a branch" (gałąź `claude/new-session-07diir`, root) — po pushu odświeża się samo w ~1 min. `index.html` w korzeniu przekierowuje do najnowszej wersji symulatora — **przy tworzeniu nowej wersji v0.X zaktualizuj URL w przekierowaniu.**
- Sloty grafik intro: w pliku v0.X szukaj `INTRO_IMG_PUSTA`/`INTRO_IMG_PELNA` — wklej data-URI (base64) obrazów wygenerowanych przez Krzyśka; dopóki `null`, rysują się wektorowe placeholdery.
- Zapasowy podgląd na claude.ai (artefakt, prywatny): `https://claude.ai/code/artifact/1e0579ce-3855-409d-a7d8-aae3b4d6fd21` — aktualizacja: z pliku v0.X usuń tagi `<!DOCTYPE>`, `<html>`, `<head>`, `<body>` i `<meta>` (zostaje `<title>` + `<style>` + treść body ze skryptem) i opublikuj narzędziem Artifact z parametrem `url` jak wyżej, żeby nie powstał nowy adres.
