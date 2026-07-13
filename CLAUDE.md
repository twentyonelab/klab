# Symulator fabryki KLAB — instrukcje dla Claude Code

Pracujesz nad **`Symulator fabryki v0.4.html`** (starsze wersje zostają w repo jako archiwum) — narzędziem do symulacji fabryki akumulatorów KLAB (recykling LAB + produkcja ogniw w obiegu zamkniętym). Zespół: Krzysiek (design/strategia) i Marcin (inżynieria), firma 21 zmysłów. Mów po polsku.

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

## Stan obecny (v0.4)

Nawigacja v0.4 (decyzja Krzyśka 13.07): **górne taby = moduły oferty M1–M6** (M1 · Schemat ciągu → widok procesu, M2 · Bilans mediów, M3 · Źródła energii → placeholder, M4 · Zatrudnienie, M5 · Layout hali → kanwa 2D, M6 · Koszty), **lewy pasek ikon = 4 widoki robocze**: układ hali 2D, schemat procesu, zestawienie modułów (wszystkie użyte karty pogrupowane per moduł z sumami), widok 3D hali. Widok procesu (M1) budowany automatycznie z pól `we→wy` (strzałka ciągła = dopasowanie dokładne, przerywana = przybliżone; linia B łańcuchuje się w całość, linia A ma jawne przerwy — brak podziału frakcji breakera w danych). Widok 3D = **statyczna izometria SVG** (bez WebGL/zależności), wysokości brył = założenie robocze 3 m do czasu pola `wys` w schemacie.

Działa: layout drag&drop (siatka 0,25/0,5/1 m/wył., obrót R, kolizje, pan/zoom, undo/redo Ctrl+Z/Y, duplikowanie), KPI live (urządzenia, moc, media, personel, powierzchnia, CAPEX), magistrale mediów wewnątrz hali (chipy E/W/N/KW + punkty przyłączy), strefy serwisowe ze znacznikiem „S", widok M2 (bilans per moduł funkcjonalny), widok M4 Personel (model zmianowy 1/2/3 per urządzenie — założenie robocze, domyślnie 3 zmiany), widok M6 Koszty (OPEX z suwakami, stacked bar struktury), eksport/import JSON, localStorage (klucz `klab_sym_v02`, czyta też `klab_sym_v01`).

Layout v0.3 (wg makiety Krzyśka z 13.07): lewy pionowy pasek ikon = przełączanie widoków (zsynchronizowany z zakładkami u góry), pełnoekranowa kanwa hali z linijkami wymiarowymi i obrysem architektonicznym, **katalog urządzeń = pływający panel po lewej** (zwijany na X; karty wg referencji Movara: ID + pigułka „n× na planie", ilustracja w prawym górnym rogu, przepływ we→wy z paskiem obciążenia, stopka ⚡/▭/👤), **karta wybranego urządzenia = pływający panel po prawej** (podpis „Wybrana karta urządzenia"; akcje Obróć/Duplikuj/Usuń dla urządzeń z planu), pionowy pasek narzędzi kanwy przy prawej krawędzi (undo/redo, siatka, zoom, dopasuj, media, strefy), na dole pasek modułów funkcjonalnych z planu (bez strzałek przepływu — topologii procesu nie ma w danych; klik filtruje katalog) + legenda mediów, w top-barze „Zapisz projekt (JSON)" + menu ⋮ (import, otwieranie starszych wersji). Grafiki urządzeń renderują się też w kartach na planie (zagnieżdżone SVG — uwaga: selektor `#svg` w CSS musi zostać zawężony, globalne `svg{width:100%}` psuje zagnieżdżone grafiki).

Czego celowo NIE przeniesiono z makiety (wymagałoby wymyślenia danych): strzałki przepływu między urządzeniami i numeracja etapów (brak topologii procesu w schemacie), pola wys./ciepło odpadowe/tagi ATEX (schemat czeka na review Marcina), „Edytuj parametry" (dane W-draft zablokowane do review).

Grafiki: `GFX` = schematy kreskowe (używane na planie hali), `GFX2` = szare „rendery" wektorowe (karty katalogu + karta urządzenia). Oba komplety to placeholdery — zdjęcia realnych urządzeń NIE mogą być hotlinkowane (plik offline) ani brane z sieci bez licencji (repo publiczne; na Wikimedia Commons brak sensownych zdjęć tego typu urządzeń — sprawdzone 13.07). Docelowo: materiały od dostawców z pakietów RFQ Marcina, wgrane jako data-URI w miejsce `GFX2`.

## Backlog (kolejność wg roadmapy — iteracja 3)

1. **Widok M3 Energia:** scenariusze źródeł (sieć/PV/odzysk ciepła/kogeneracja/magazyn KLAB) — CZEKA na dane z modułu M3, nie wymyślaj założeń
2. Po review Marcina: podmiana katalogu DEVICES na pełny (~15 pakietów) + ewentualne nowe pola schematu
3. Strefy hali (magazyny, kwasy, socjal) jako rysowalne prostokąty; eksport rzutu do PNG/PDF (deliverable M5)
4. M6: cash cost na jednostkę produkcji (czeka na wolumeny linii A i B — pytania do klienta) · M4: narzut urlopowo-chorobowy i personel pośredni (UR, logistyka, QA)

## Czego NIE robić

Pełne 3D (WebGL, silniki, obracanie kamerą) — widok 3D wyłącznie jako statyczna izometria SVG (złagodzenie zasady na polecenie Krzyśka 13.07) · animacje ozdobne · dark mode · live-collab · optymalizacja layoutu algorytmem · dodawanie urządzeń spoza dokumentacji klienta · rozszerzanie zakresu poza 6 modułów oferty (M1–M6).

## Po sesji

- Dopisz iterację do sekcji „Dziennik decyzji" w `../START — nowy projekt Cowork/Fabryka akumulatorów.md` (jedna linia: co się zmieniło).
- **Po każdym wgraniu (push) ZAWSZE podaj na końcu odpowiedzi link do podglądu:**
  `https://twentyonelab.github.io/klab/`
  GitHub Pages jest skonfigurowane w trybie „Deploy from a branch" (gałąź `claude/new-session-07diir`, root) — po pushu odświeża się samo w ~1 min. `index.html` w korzeniu przekierowuje do najnowszej wersji symulatora — **przy tworzeniu nowej wersji v0.X zaktualizuj URL w przekierowaniu.**
- Zapasowy podgląd na claude.ai (artefakt, prywatny): `https://claude.ai/code/artifact/1e0579ce-3855-409d-a7d8-aae3b4d6fd21` — aktualizacja: z pliku v0.X usuń tagi `<!DOCTYPE>`, `<html>`, `<head>`, `<body>` i `<meta>` (zostaje `<title>` + `<style>` + treść body ze skryptem) i opublikuj narzędziem Artifact z parametrem `url` jak wyżej, żeby nie powstał nowy adres.
