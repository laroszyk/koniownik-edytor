# Koniownik — edytor schematów placu

Narzędzie dla instruktorów jazdy konnej: układanie schematu placu (przeszkody,
pachołki, opony, figury) i rysowanie trasy przejazdu, a potem wydruk albo obrazek
dla jeźdźca.

- **`edytor-placu.html`** — cała aplikacja. Jeden plik, ~8 900 linii, ~300 funkcji,
  bez bibliotek, bez builda, bez sieci. Otwiera się z pliku i działa.
- **`INSTRUKCJE-PROJEKTU.md`** — dziennik projektu po polsku: każda decyzja, każdy
  błąd i dlaczego został naprawiony tak, a nie inaczej. Czytaj to przed kodem.

## Jak to jest zbudowane

- Jedna funkcja opakowująca (IIFE), stan w `stan`, zapis w `localStorage`
  pod kluczem `koniownik-plac`.
- Rysunek to SVG **w metrach** — układ współrzędnych placu, nie piksele.
  Plac „w pionie" (`stan.pion`) to obrót całej sceny o 90°.
- Trasa przejazdu to **kolejność, nie rysunek**: `krokiTrasy()` → kroki posortowane
  po numerze, `torPrzejazdu()` → punkty kontrolne, `sciezkaToru()` → ścieżka SVG.
  Wygładzanie: jedna styczna na punkt (`stycznePunktow`), uchwyty Béziera
  (`uchwytyOdcinka`), a wokół pachołka mijanego kilka razy — łuki `A`
  (`ulozLuki` / `ulozLukiCiagu`).
- Całość po polsku: nazwy funkcji, zmiennych i komentarzy. **To jest celowe**
  i ma tak zostać.

## Czego NIE przerabiać

Te rzeczy wyglądają jak problemy, ale są decyzjami produktowymi:

1. **Jeden plik HTML.** Bez builda, bez npm, bez frameworka. Instruktorka ma
   otworzyć plik ze swojego pulpitu albo z telefonu i ma działać offline.
   Nie proponuj podziału na moduły ani bundlera.
2. **Polskie nazewnictwo** w kodzie i komentarzach.
3. **Komentarze z datami i cytatami** użytkowniczki — to pamięć projektu,
   nie szum. Nie kasować.
4. **Brak zależności zewnętrznych** — żadnego CDN-a, żadnego fetcha.

## Co warto sprawdzić

Przegląd ma sens zwłaszcza w tych miejscach:

- **Geometria trasy** (`torPrzejazdu`, `sciezkaToru`, `stycznePunktow`,
  `uchwytyOdcinka`, `ulozLukiCiagu`) — czy krzywa nigdzie nie wchodzi na sprzęt,
  czy łuki wokół jednego pachołka trzymają jeden kierunek obiegu, czy nie ma
  przypadków z dzieleniem przez zero i NaN-em w ścieżce.
- **Obsługa dotyku i myszy** (`pointerdown`/`move`/`up`, kolejność trafiania:
  kropka → sprzęt → linia/krzyżyk) — czy gest niszczący nie zadziała przez
  przypadek i czy nic nie „kradnie" dotknięć.
- **Stan i zapis** — migracje starych zapisów przy wczytaniu, spójność po
  cofnięciu (`zapiszHistorie`), czy da się doprowadzić stan do sprzeczności
  (numery przejść, `powroty`, `punkty`).
- **Eksport obrazka** (`zrobObraz`) — rasteryzacja SVG → PNG działa w dwóch
  trybach, bo ramka podglądu blokuje `blob:`; czy proporcje zgadzają się dla
  placu poziomego i pionowego.
- **Dostępność i wydajność** — `rysuj()` przerysowuje całą scenę; czy nie ma
  pętli przerysowań, czy etykiety i uchwyty czytają się przy dużym zoomie.
- **Martwy kod** — zostało kilka wyłączonych na stałe gałęzi (`trybTor`,
  `trybNumerowania`, `naFig`, `POLACZ_ZASIEG`, `PRZYKLADY`); można je wskazać
  do usunięcia.

## Czego tu nie ma

Testy (Playwright, ~1000 sprawdzeń) powstawały w środowisku tymczasowym i nie są
częścią tego repozytorium. Jeśli przegląd ma je uwzględnić, trzeba je odtworzyć.

## Licencja

Projekt prywatny, w trakcie budowy. Wszystkie prawa zastrzeżone.
