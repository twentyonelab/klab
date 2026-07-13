# Symulator fabryki KLAB — instrukcje dla Claude Code

Pracujesz nad **`Symulator fabryki v0.1.html`** — narzędziem do symulacji fabryki akumulatorów KLAB (recykling LAB + produkcja ogniw w obiegu zamkniętym). Zespół: Krzysiek (design/strategia) i Marcin (inżynieria), firma 21 zmysłów. Mów po polsku.

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

## Stan obecny (v0.1)

Działa: layout drag&drop (siatka 0,5 m, obrót R, kolizje, pan/zoom), KPI live (moc, media, personel, powierzchnia, CAPEX), linie mediów do szyn, strefy serwisowe, kafelki kart urządzeń wg referencji Movara (przepływ we→wy, pasek obciążenia, schematyczne grafiki SVG w obiekcie `GFX`), widok M2 (bilans per moduł funkcjonalny), eksport/import JSON, localStorage.

## Backlog (kolejność wg roadmapy — iteracja 2 i 3)

1. **Widok M4 Personel:** obłożenie stanowisk per etap/zmianę z urządzeń na planie (model zmianowy 1/2/3-zmianowy per urządzenie)
2. **Widok M6 Koszty:** OPEX z suwakami założeń (cena energii zł/kWh, woda, N₂, stawka osobowa, UR % CAPEX), cash cost, wykres struktury
3. **Widok M3 Energia:** scenariusze źródeł (sieć/PV/odzysk ciepła/kogeneracja/magazyn KLAB) — CZEKA na dane z modułu M3, nie wymyślaj założeń
4. Po review Marcina: podmiana katalogu DEVICES na pełny (~15 pakietów) + ewentualne nowe pola schematu
5. Strefy hali (magazyny, kwasy, socjal) jako rysowalne prostokąty; eksport rzutu do PNG/PDF (deliverable M5)

## Czego NIE robić

3D · animacje ozdobne · dark mode · live-collab · optymalizacja layoutu algorytmem · dodawanie urządzeń spoza dokumentacji klienta · rozszerzanie zakresu poza 6 modułów oferty (M1–M6).

## Po sesji

Dopisz iterację do sekcji „Dziennik decyzji" w `../START — nowy projekt Cowork/Fabryka akumulatorów.md` (jedna linia: co się zmieniło).
