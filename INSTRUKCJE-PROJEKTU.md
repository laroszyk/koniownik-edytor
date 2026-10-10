# Dosiad — kontekst projektu

Plik do wskazania przy zakładaniu projektu w Cowork. Opisuje, co budujemy,
co jest już ustalone i jak pracujemy, żeby nie zaczynać od zera w nowym zadaniu.

## Kto i co

Luiza — projektantka UX/UI z Poznania, instruktorka jazdy konnej od ośmiu lat,
prowadzi własną stajnię. Rozmawiamy po polsku.

Budujemy **Dosiad** — aplikację dla instruktorów jeździectwa. Dwie części:

1. **Edytor placu** — rysowanie schematów: drągi, kawaletki, stacjonaty, oksery,
   pachołki, opony, litery ujeżdżeniowe, tor przejazdu. Liczy odległości
   i fule dla wybranych wielkości koni. Prototyp gotowy: `edytor-placu.html`.
2. **Dziennik instruktora** — zapis treningów, profile jeźdźców i koni.
   Do przebudowy od nowa. Następny w kolejce.
3. **Grafik tygodnia** — po dzienniku. Układa tydzień z ograniczeń: dostępność
   jeźdźców, przypisanie jeźdźca do koni, limity pracy konia, brak dublowania.
   To algorytm spełniania ograniczeń, nie zadanie dla modelu językowego.

Pełne uzasadnienie, analiza konkurencji, zakres MVP, koszty, sprawy prawne
i lista rzeczy odrzuconych — w `plan-produktu.html`, sekcje 01–15.

## Stan na 2 września 2026

- Edytor placu skończony jako prototyp, opublikowany jako artefakt.
- Dane starego dziennika wyczyszczone — były w całości wygenerowane.
  Zostaje sam kształt rekordów w `dane/jezdzcy-i-konie.json`.
- Dziennik i grafik jeszcze nie istnieją.

## Rzeczy ustalone, do których nie wracamy

- **Konwencja pomiaru**: drągi od środka do środka; przeszkody od tyłu
  pierwszej do podstawy następnej, z wliczonym zapasem na lądowanie i odbicie.
- **Wzór, nie tabela**: `odległość = zapas + n × fula`, osobno dla trzech grup —
  konie duże (powyżej 156 cm), konie małe (149–156), kuce (do 148).
- **Grupa mieszana**: na jednym treningu bywa kilka wielkości naraz. Żadna
  liczba fuli nie pojawia się bez nazwy wielkości, której dotyczy.
- **Odległości nie są rysowane na placu** — plac zostaje rysunkiem, liczby idą
  do listy pod nim i do tabelki pod planem na kartce A4.
- **Wzrost konia** będzie podpowiadał grupę, gdy powstanie dziennik, ale ręczny
  wybór zostaje.
- **Tor wiązany po id przeszkody**: punkt trasy postawiony na drągu zapamiętuje
  jego id i jedzie wyłącznie z nim. Nie wracamy do wiązania po odległości —
  przy trzech drągach obok siebie ściągało trasę z sąsiadów.
- **Świeżo postawiona przeszkoda nie jest częścią trasy**, dopóki jej nie upuścisz.
  Inaczej element lądujący na środku placu od razu łapał tor i ciągnął go w wybrzuszenie.
- **Figury liczone, nie rysowane**: wolta, półwolta i serpentyna to kafelki w dolnym
  pasku, obok przeszkód. Koło 20 m odrzucone — wolta z wpisaną średnicą je zastępuje.
- **Figura wstawia się jednym kliknięciem**: kafelek, średnica, przycisk — i stoi na
  środku placu jako osobny odcinek toru. Nie wracamy do wybierania miejsca przed
  wstawieniem: dwuetapowe „wybierz figurę, potem dotknij punktu trasy" wyglądało
  dokładnie jak awaria, bo po zatwierdzeniu na placu nie pojawiało się nic.
- **Punkty figury jadą razem.** Przeciągnięcie dowolnego przesuwa całą figurę; obce
  kotwice nie wchodzą w jej środek. Inaczej kółko przestaje być kółkiem po pierwszym ruchu.
- **Figura zachowuje się jak przeszkoda, nie jak rysunek**: chwyta się ją i przesuwa
  bez wchodzenia w tryb Tor, a zaznaczona pokazuje w pasku swoją średnicę do zmiany
  (i liczbę łuków przy serpentynie) oraz Usuń. Powiększana wolta rośnie wokół swojego
  środka, serpentyna wokół swojej osi — zaczep zostaje przeliczony.
- **Uchwyt do rozciągania**: zaznaczona figura pokazuje kropkę po przeciwnej stronie
  niż jej zaczep, przerywaną linię średnicy i liczbę metrów. Ciągnięcie uchwytu zmienia
  średnicę od zaczepu — jeden koniec stoi, drugi jedzie za palcem, jak gumka.
  Pole w pasku i guziki − / + zostają dla dokładnych liczb.
- **Przerzuć i Obróć o 90°** w pasku zaznaczonej figury. Odbicie liczone jest
  względem środka figury, nie jej zaczepu — inaczej wolta przeskakiwałaby o własną
  średnicę, a dwa przerzucenia nie wracałyby do punktu wyjścia. Figura, która po
  obrocie wychodzi za bandę, jest wsuwana z powrotem, nie odrzucana.
- **Kropka z obwódką na początku linii toru to uchwyt całej tej linii** — ciągnięcie
  poza trybem Tor przesuwa cały odcinek razem z figurami, które w nim siedzą.
  W trybie Tor ta sama kropka dalej przesuwa pojedynczy punkt.
- **Podpowiedź startowa nie kasuje podpowiedzi z akcji.** Pokazywała się 700 ms po
  wczytaniu i nadpisywała to, co użytkownik zdążył w tym czasie zrobić.
- **Półwolta to jeden ciągły łuk, nie półkole doklejone do prostej.** Łuk jedzie
  216,87°, czyli tyle, żeby jego styczna w punkcie wyjścia wskazała punkt powrotu na
  linii; skos wychodzi z niego stycznie i w figurze nie ma ani jednego kąta. Przy
  powrocie L = 3 × promień kąt dojścia do ściany to 36,87°, niezależnie od średnicy.
  Odrzucone po drodze: równe półkole plus prosta (łamało się o 41°) i półkole plus
  krótki wybieg po stycznej (dalej 2,9 × mediana skrętu — Luiza zobaczyła to gołym okiem).
- **Punkty skosu mają kroki podobne do cięciwy łuku.** Jeden punkt w połowie skosu
  leżał tak daleko, że rozciągał styczną Catmull-Roma w ostatnim odcinku łuku i krzywa
  wybrzuszała się tuż przed wyjściem.
- **Miernik gładkości figury**: skręt narysowanej krzywej na każde 15 cm długości,
  próg względem MEDIANY tego skrętu, nie względem próbkowania. Kąt w figurze to
  pojedynczy skok ponad resztę krzywej; miernik liczący kąt między próbkami bez
  odniesienia przepuścił załamanie, które było widać na oko.
- **Serpentyna ścienna też ma uchwyt, z sufitem na ścianie.** Ciągnięcie do środka
  zmniejsza ją proporcjonalnie (te same proporcje, tylko mniejsza, już nie przy ścianach),
  ciągnięcie na zewnątrz zatrzymuje się na szerokości placu. To samo robi pole „szer. m"
  i guziki − / +. Obrót o 90° zachowuje „pełną szerokość" jako pełną w nowej orientacji,
  a węższej zostawia jej metry.
- **Serpentyna ma dwa tryby**: wpisana średnica (półkola o tej średnicy) albo
  „od ściany do ściany" — połówki elipsy dotykające obu długich ścian, na całą długość
  placu, wymiary liczone z placu, nie wpisywane. W tym trybie pasek daje tylko liczbę
  łuków, nie ma uchwytu rozciągania, a obrót o 90° przelicza wymiary na drugą stronę placu.
- **Linię toru zaznacza się dotknięciem i kasuje pojedynczo.** Pasek pokazuje jej długość
  i liczbę punktów, guzik Usuń zdejmuje tę jedną linię, a „Popraw punkty" wchodzi w tryb
  Tor. Wcześniej jedynym usuwaniem był „Usuń tor", czyli wszystko naraz.
- **Kolejność kafelków**: skoki, potem figury ujeżdżeniowe, potem pachołki i reszta.
- **Od końca figury da się rysować tor dalej** — dotknięcie skrajnego punktu w trybie
  Tor przełącza dorysowywanie na ten koniec zamiast na koniec listy punktów.
- **Długość serpentyny to liczba łuków razy średnica.** Okno pokazuje ją na bieżąco
  i porównuje z długością placu; odmowa nazywa prawdziwy powód, bo przy serpentynie
  problemem prawie zawsze jest liczba łuków, a nie średnica.
- **fmtL zamiast fmt w opisach figur**: „10 m", nie „10,00 m". Przecinek dziesiętny
  w input[type=number] daje pustą wartość — pola dostają liczbę wprost.
- **W oknie ma być jeden przycisk, który coś robi.** Generyczne „Gotowe" obok akcji
  właściwej zbiera kliknięcia i wygląda na zatwierdzenie — w oknach z własną akcją
  nazywa się „Anuluj".
- **Messenger skreślony** — Meta demontuje mechanizm, na którym miałby stać.
- **Predykcja kontuzji i wideo** — odrzucone, z powodem i warunkiem powrotu
  zapisanym w planie.

## Jak pracujemy

- Po polsku, bez owijania. Krytyka projektowa mile widziana — Luiza jest
  projektantką i ocenia jak projektantka.
- **Testować przed publikacją.** Każda zmiana w edytorze sprawdzana
  w przeglądarce (Playwright), zanim trafi do niej. Zdarzyło się publikować
  bez testu i za każdym razem kończyło się to wpadką.
- Nazwy funkcji i komentarze w kodzie po polsku — tak jest napisany edytor.
- Nie zgadywać za nią w rzeczach jeździeckich. Widełki odległości, wielkości
  koni, realia stajni — ona jest tu autorytetem, nie model.
- Nie usuwać jej danych bez pytania; oryginały zostawiać obok.

## Co dalej

Domknąć edytor po jej testach na telefonie, potem dziennik, potem grafik.

## Generator tras — ustalenia z 3 września 2026

**Uwaga na start: generator w dużej mierze JUŻ ISTNIEJE w edytorze.**
`oknoPrzyklady()` ma osiem przykładów (`PRZYKLADY`), inwentarz „Mój sprzęt",
funkcję `brakiDla()`, filtr „Tylko to, co ułożę ze swojego sprzętu"
i przycisk „🎲 Wylosuj, co dziś postawić". Nie budować drugiego obok —
rozbudowywać ten.

Czego temu, co jest, brakuje (ustalone w rozmowie 3 września):

- **Szablon to rozkład, nie odległości.** Obecne `PRZYKLADY` mają wpisane
  współrzędne bezwzględne i sztywne odstępy — stąd wskazówki w rodzaju
  „co 3,10 m dla dużego konia, sprawdź dla swojego". Docelowo rozmieszczenie
  opisane względem liter i ścian, a odległości liczone przy wczytaniu, dla
  zaznaczonych wielkości koni.
- **Bez sprawdzania, czy się mieści.** Fałszywe ostrzeżenie przy układzie, który
  spokojnie wejdzie, jest gorsze niż brak ostrzeżenia. Luiza rysuje na kartce A4
  bez wymiarów i dopasowuje na placu.
- **Tor opcjonalny i osobny od rozkładu.** Jeden rozkład, zero lub kilka wariantów
  przejazdu. Rozkład, po którym da się pojechać na kilka sposobów, obsługuje
  początkującego i zaawansowanego bez przestawiania drągów.
- **Dyscyplina jako wymiar filtra**: ujeżdżenie / skoki / western. Western to pusta
  półka — pole istnieje, biblioteki brak, bo Luiza się nie zna i nie zgaduje.
- **Odległość jest cechą roli, nie ćwiczenia**, i ma dwa tryby: z wzoru albo
  „bez reguły" + wskazówka własnymi słowami instruktorki. Slalom nie ma reguły:
  „rozstaw tak, żeby koń się mieścił; węziej się da, ale wtedy kłus zebrany".
- **Sprzęt bywa wymienny, a zamiana bywa progresją.** Pachołek ↔ opona to zamiana
  równoważna. Drąg → kawaletka → stacjonata → okser to oś trudności, i ponieważ
  drągi i przeszkody mierzy się inną konwencją, podniesienie drąga przelicza
  odległość.
- **Okser zjada dwa drągi** (`DRAGI_W`) — każdy przydział sprzętu musi to widzieć.
- **Odbicie w lustrze** daje drugi wariant z jednego szablonu: litery zamieniają się
  parami (K↔F, E↔B, H↔M), linia środkowa zostaje.

## Warto podpatrzeć u konkurencji (research 3 września)

- **Polework Patterns** (UK, £3.99 jednorazowo) — filtr po liczbie drągów, filtr po
  celu treningu w 15 kategoriach, oznaczenia chodu i trudności kolorem, ulubione,
  przycisk losowania. Nie liczy odległości, nie rozróżnia konia od kuca, nie da się
  dodać własnego ćwiczenia ani narysować placu.
- **Polework** (druga aplikacja) — 149 ćwiczeń w stępie, kłusie i galopie, ściąga
  z odległościami dla koni i kuców jako osobna strona, nie wpisana w układ.
- **Parkour.design, ObstacleStudio, Scopey** — narzędzia dla budowniczych parkurów,
  nie dla instruktorów. Liczą odległości i fule, ale niczego nie proponują.
  ObstacleStudio publikuje trasę jeźdźcom w 3D — potwierdzenie kierunku
  instruktor → jeździec.
- **Aplilab School** (PL, 30 zł/mies. instruktor, 200 zł szkoła) — grafik, jeźdźcy,
  konie z godzinami pracy, rozliczenia, konta uczestników, SMS. Zero placu i ćwiczeń.
  Bezpośrednia konkurencja dla dziennika i grafika; siedzi tuż nad planowaną ceną.

Do skopiowania: filtr po celu treningu, oznaczenia chodu i trudności, ulubione,
miniatura układu na liście. Do nieskopiowania: konfetti, przedziały zamiast
dokładnego inwentarza, osobna ściąga z odległościami.

## Poczekalnia — wrócić po domknięciu całości

Przypomnieć Luizie o tej sekcji, gdy edytor, generator, dziennik i grafik będą gotowe.

- **Audio-przewodnik w słuchawce.** Tylko nasza aplikacja może to zrobić, bo tylko ona
  ma narysowany plan. Pętla czasowa to zgadywanie tempa konia — lepiej przeczytać całe
  ćwiczenie raz na początku albo krok po kroku na polecenie jeźdźca. Audio generowane
  przy tworzeniu planu, na wifi; na ujeżdżalni zasięgu nie ma.
- **Ćwiczenia uzupełniające dla jeźdźca.** Mapowanie objawu konia na część ciała
  człowieka to udawana diagnoza. Wersja, która się broni: diagnozę stawia instruktorka
  i przypisuje zadanie domowe — czyli pole w dzienniku, nie osobny moduł fitness.

## Zrobione w edytorze 3 września (wieczór)

W `oknoPrzyklady()`: filtr po dyscyplinie i po celu (chipy), filtr „★ Ulubione"
zapisywany w `localStorage` pod `dosiad-ulubione`, miniatura każdego układu
(`miniaturaPrzykladu`, samodzielny SVG — nie przechodzi przez `rysuj()`),
znaczki chodu i poziomu, licznik „n z 8". Losowanie respektuje wszystkie filtry.
Osiem przykładów dostało pola `dysc`, `chod`, `poziom`.

W `brakiDla()`: pachołek i opona liczą się do wspólnej puli (`WYMIENNE`), więc
kto ma opony zamiast pachołków, dalej ułoży slalom. Komunikat mówi wtedy
„pachołków lub opon". Drąg i kawaletka celowo NIE są wymienne — podniesienie
drąga zmienia ćwiczenie i odległości.

Sprostowania do wcześniejszych notatek w tym pliku:
- **Odbicie w lustrze już było** — `oknoOdbij` / `odbij(os)`, w dwóch osiach,
  z poprawnym przeliczaniem kątów. Nie budować drugiego.
- **`brakiDla` od początku liczyło oksera jako dwa drągi** (`DRAGI_W`).
  Błąd był w demie generatora, nie w edytorze.

Zostaje jedna rzecz z ustaleń, świadomie nieruszona: **odległości przeliczane
pod wielkość konia przy wczytaniu przykładu**. Obecne `PRZYKLADY` mają odstępy
zapisane w współrzędnych bezwzględnych, więc żeby je przeliczać, trzeba najpierw
zadeklarować relacje między elementami („te trzy drągi to linia co jedną fulę
kłusa"). Część układów — koło, przekątna — nie mieści się w modelu linii.
To decyzja o kształcie danych i wymaga rozmowy z Luizą, nie zgadywania.

## Struktura aplikacji — ustalona przez Luizę 3 września

Dosiad ma **cztery części**, generator jest osobną, nie funkcją edytora:

1. **Dziennik** — zapis treningów, profile jeźdźców i koni.
2. **Kalendarz** — grafik tygodnia.
3. **Generator pomysłów na treningi** — `generator.html`, własny ekran.
4. **Edytor schematów** — `edytor-placu.html`, działa też samodzielnie.

Powłoki (nawigacji między częściami) jeszcze nie ma — na razie osobne pliki HTML.

### Generator — stan
Zbudowany na prawdziwych danych: osiem ćwiczeń Luizy, prawdziwe fule i zapasy,
prawdziwe widełki rozstawu drągów. Wygląd i typografia wprost z edytora placu —
to jedna aplikacja, więc jeden język wizualny.

Co liczy sam, zamiast mieć wpisane:
- **kolejność przeszkód** — z rzutu na łamaną toru (`drogaWzdluzToru`), bez SVG
- **odległości** w obu konwencjach, z klamrowaniem równych odstępów
  („4 drągi co 1,35 m", nie trzy razy to samo)
- **ocena dla każdej wybranej wielkości** — port `ocen()` z edytora, żeby obie
  części mówiły o tej samej odległości to samo
- **chód** z rozstawu drągów; gdzie rozstawu nie ma — milczy
- **rodzaj** (naziemne / ze skokami) z typów elementów
- **braki sprzętu** z pulą pachołek+opona i okserem liczonym jako dwa drągi

Wspólne z edytorem klucze `localStorage`: `dosiad-sprzet`, `dosiad-ulubione`.

### Do rozstrzygnięcia
Biblioteka ćwiczeń stoi teraz w dwóch plikach naraz (`PRZYKLADY` w edytorze,
`CWICZENIA` w generatorze). Jeden plik dla obu nie wchodzi w grę, dopóki obie
części są publikowane jako osobne artefakty — artefakt musi być samodzielny.
Propozycja: skoro generator jest osobną częścią, edytor traci „Gotowe ustawienia"
i zostaje przy „Moich schematach"; biblioteka mieszka wtedy tylko w generatorze.
Czeka na decyzję Luizy.

## Edytor placu — zatrzymanie i łączenie odcinków (7 września)

### Kafelek „X" — zatrzymanie
Nowy typ `zatrzymanie` w `TYPY`, kafelek w dolnym pasku zaraz za serpentyną,
przed pachołkiem. Rysowany jako krzyżyk w kolorze toru (`var(--tor)`), nie jako
sprzęt leżący na piasku — bo to znak na trasie. Nie liczy się do puli sprzętu
i nie wpina się w tor (nie jest przejezdzalny), więc przesuwanie go nie rusza linii.

### Łączenie i rozłączanie końcówek
Po upuszczeniu linii albo figury edytor sprawdza, czy któraś jej końcówka nie
wylądowała bliżej niż `POLACZENIE` (1,2 m) od końcówki innej linii. Jeśli tak —
dosuwa przeciąganą linię dokładnie na styk i skleja obie w jeden odcinek.
Miejsce styku dostaje `zlaczenie: true` i widoczne kółeczko z krzyżykiem;
dotknięcie go rozłącza (`rozlacz()` przywraca `przerwa: true`).

**Nieaktualne od 9 września** (zostawione, żeby było widać, co się zmieniło):
„łączymy tylko koniec z początkiem, nigdy początek z początkiem". Dziś próbujemy
czterech ustawień i wolno odwrócić linię narysowaną ręką — patrz sekcja
„Trzy ograniczenia domknięte" na końcu. Figury nadal zostają w swoim kierunku,
bo trzymają parametry na pierwszym punkcie.

Przerwy między odcinkami trzyma teraz jedna funkcja `zapiszOdcinki()` —
`przerwa` siedzi wyłącznie na pierwszym punkcie każdego odcinka poza pierwszym.
Wcześniej ustawiało to kilka miejsc naraz i potrafiły się rozjechać.

### `zmiescPrzesuniecie()` — przycinanie całej grupy, nie punktu po punkcie
Przy przesuwaniu figury pod bandę każdy punkt był przycinany osobno, więc koło
przy ścianie robiło się spłaszczonym jajkiem. Teraz liczymy jedno wspólne
ograniczenie dla całego prostokąta figury i przesuwamy wszystkie punkty o tyle
samo. Dotyczy figur, całych linii i sklejania.

Testy: `testy13.mjs` (42 sprawdzenia) plus regresja `testy.mjs`…`testy12.mjs` —
razem 364 sprawdzenia, wszystkie zielone przed wgraniem.

## Ustalenia z 7 września — model ćwiczenia bez piętrzenia zależności

Luiza zgłosiła, że z każdą rozmową robi się coraz więcej zależności (chód,
wielkość konia, grupa mieszana, poziom) i przestaje to być do ogarnięcia.
Rozstrzygnięcie idzie w stronę **mniejszej** liczby pojęć, nie większej:

- **Chód nie jest częścią tożsamości ćwiczenia.** „Drągi w kłusie" to zła nazwa,
  bo zaszywa decyzję w nazwie i wymusza trzy ćwiczenia zamiast jednego. Ten sam
  rozkład czterech drągów obsługuje stęp, kłus i galop — zmienia się rozstaw.
- **Chód i wielkość konia są WYNIKIEM, nie wyborem.** Instruktor podaje trzy
  rzeczy: co ma na placu, po co dziś trenuje, jakie konie. Odległości spadają
  z tego same, w tabelce na dole karty: wiersz na chód, kolumna na każdą
  zaznaczoną wielkość. Nic więcej do klikania.
- **Ćwiczenie deklaruje, które chody mają sens** (`chody: ["klus","galop"]`) —
  jedno małe pole, które instruktorka wie od ręki. Reszta liczy się z wzoru.
- **Poziom trudności zostaje skreślony** i nie wraca. Trudność zależy od trasy,
  a trasy celowo nie narzucamy.
- **Wartość siedzi w trasach, nie w rozkładach.** Trzy drągi na ziemi wymyśli
  każdy — sensu dodaje pomysł, którędy przez nie pojechać. Stąd kierunek:
  mało rozkładów, dużo tras na każdym. Jeden rozkład × sześć tras to sześć
  treningów przy tym samym sprzęcie, a trasy Luiza wymyśla szybko.
- **„Daj inne" powinno losować trasę w obrębie rozkładu**, nie przeskakiwać
  między rozkładami.

Zmiany wprowadzone tego dnia w generatorze i przeglądzie:
pachołki i „inny" dostały wewnętrzny obrót znoszący obrót sceny (stały bokiem);
cele konia — dodane „Rozluźnienie", usunięta „Prostość"; cele jeźdźca — dodane
„Równowaga", „Rytm", „Przejścia". Ponieważ te trzy wyrazy stoją teraz na obu
osiach, zaznaczenie w filtrze niesie ze sobą oś (`kon|Rytm`, `jezdziec|Rytm`) —
inaczej filtr myliłby jedno z drugim. Sekcja „Wskazówka instruktorki"
przemianowana na **„Nasze wskazówki"**.

## Do poczekalni dopisane 7 września

**Samouczek.** Na sam koniec, gdy wszystkie cztery części będą gotowe — pierwsze
uruchomienie musi tłumaczyć, skąd biorą się liczby i czemu nie ma poziomu
trudności. Bez tego cała ta robota z konwencjami pomiaru jest niewidoczna.

## Edytor placu — sufit figur i węzeł zamiast plusika (7 września, po testach Luizy)

### Figura nie wyjeżdża już poza plac
Sufit średnicy był wpisany na sztywno jako 40 m (`maksFigury`, `uchwytFigury.maks`),
więc wolta i półwolta rosły, dopóki nie wystawały poza bandę o `WYSTAJE_MAX`.
Teraz sufit liczy `najwiekszaFigura()` — szuka krokiem 0,25 m największej średnicy,
która się mieści, budując kandydata dokładnie tak, jak zbuduje go przebudowa.
Stąd nowa `punktyPrzebudowy()`: jedno miejsce, w którym powstają punkty figury,
używane i przez przebudowę, i przez pytanie „ile jeszcze wejdzie". Gdyby to były
dwie formuły, pasek obiecywałby rozmiar, którego przebudowa i tak by nie przyjęła.

Sufit zależy od tego, jak figura rośnie: ciągnięta za uchwyt rośnie od zaczepu
(`odZaczepu: true`), zmieniana w pasku — wokół własnego środka. Dlatego
`najwiekszaFigura` dostaje ten sam przełącznik.

`WYSTAJE_MAX` z 0,75 m na 0,25 m — tyle, ile wynosi krok siatki. Trzy czwarte
metra za bandą widać gołym okiem.

Komunikat „X m to szerokość placu" został tylko przy serpentynie od ściany do
ściany, bo tylko tam jest prawdziwy. Przy wolcie i półwolcie: „to wszystko, co
mieści się w tym miejscu placu".

### Węzeł zamiast plusika
Kółko z plusem na styku dwóch linii było pułapką: plus czyta się jako „kliknij,
żeby połączyć", więc Luiza dotykała go, żeby scalić — a on rozłączał. Wygląda to
tak, jakby łączenie w ogóle nie działało.

Teraz:
- znak styku to **pełna kropka w obwódce** (supeł), nie plus;
- dotknięcie węzła **zaznacza** go i pokazuje w pasku „Złączone końcówki ·
  jedna trasa · N m" z przyciskiem **Rozłącz** — nic się nie dzieje samo;
- po udanym sklejeniu edytor **od razu zaznacza świeży węzeł**, więc pasek
  potwierdza, że to teraz jedna trasa. Wcześniej scalenie nie zmieniało obrazka
  (figury i tak się stykały), więc nie było po czym poznać, że zadziałało;
- w trybie Tor dotknięcie węzła nadal rozłącza od razu — tam grzebie się
  w pojedynczych punktach i pasek jest zajęty narzędziami toru.

Testy: `testy15.mjs` (30 sprawdzeń) plus poprawione `testy13.mjs` — razem
396 sprawdzeń w trzynastu plikach, wszystkie zielone przed wgraniem.

## Paleta marki Dosiad (7 września, ustalona przez Luizę)

Cztery kolory, w proporcji 60 / 30 / 8 / 2. To jest teraz marka, nie propozycja.

| Kolor | Hex | Udział | Do czego |
|---|---|---|---|
| Ciemny brąz obudowy | `#241C16` | 60% | paski, panele, tła okien |
| Piasek placu | `#DCCDB2` | 30% | powierzchnia ujeżdżalni — materiał, nie dekoracja |
| Patyna | `#269186` | 8% | logo, przyciski główne, tor przejazdu, zaznaczenie, linie wymiarowe |
| Owies | `#AFC42B` | 2% | werdykt „✓ pasuje", stan włączony, wylosowana propozycja — **nic poza tym** |

Zmiany w kodzie (edytor, generator, przegląd — wszystkie trzy pliki):
- `--accent` `#35A79C` → `#269186`, `--accent-ink` → `#05211E`
- `--tor` `#4F57A6` → `#269186` — tor przestał być granatowy, jest patyną
- `--ok` `#6FB36F` → `#AFC42B`, nowy `--ok-ink: #22280A` na napisy na owsie
- `button.on` (stan włączony) z patyny na owies; `button.chip.on` i inne
  zaznaczenia zostają na patynie — patyna to „wybrane", owies to „zgoda"
- ikonki w dolnym pasku z `#8E9BE8` na `var(--accent)`
- `.znak.rodzaj-naziemne` z `--ok` na `--ink-soft` — owies nie jest kolorem kategorii
- legenda odległości przestała nazywać kolory („✓ zielony" → samo „✓"), bo owies
  nie jest zielony i za rok kolor może się znowu zmienić

Skutek uboczny do sprawdzenia okiem: tor i zaznaczenie mają teraz ten sam kolor,
więc zaznaczona linia różni się **grubością i pełną siłą koloru**, nie odcieniem.
Patyna na piasku ma niższy kontrast niż dawny granat (2,5:1 wobec 3,9:1) — przy
tej grubości kreski czyta się dobrze, ale gdyby na słońcu na telefonie znikała,
trzeba będzie przyciemnić samą linię toru.

## Figury kończą się przed płotem (7 września)

`LUZ_BANDY = 0.5` — powiększana figura zatrzymuje się pół metra przed ścianą,
bo koń jedzie po śladzie, nie ociera się o bandę.

Ważne, czego ten luz **nie** robi, żeby nie popsuć prawdziwej jazdy:
- nie dotyczy przyjmowania figury, tylko rośnięcia. Serpentyna na cztery łuki po
  10 m zajmuje plac 40 m co do metra i dalej wchodzi — bo tak się ją jeździ;
- sufit nigdy nie kurczy tego, co już stoi (`naj` startuje od bieżącej średnicy),
  więc dotknięcie uchwytu nie zmniejsza figury wpisanej w plac;
- `wsunNaPlac` (po przerzuceniu i obrocie) bierze luz tylko wtedy, gdy figura ma
  na niego miejsce — inaczej wsuwa do samej ściany;
- serpentyna „od ściany do ściany" zachowuje swój dawny odstęp 0,25 m, bo jej
  sensem jest dotknięcie obu ścian.

Testy: `testy15.mjs` (35 sprawdzeń, w tym „kończy się przed płotem" mierzone
odległością do bandy) — razem 401 sprawdzeń w piętnastu plikach, wszystkie zielone.

## Poprawki po drugim przeglądzie Luizy (7 września)

### Tor ciemniejszy, zaznaczenie bez pogrubienia
`--tor: #1B665E` — patyna przyciemniona o 30%. Na piasku kontrast rośnie z 2,5:1
do 4,3:1, więc trasa czyta się mocniej niż czysta patyna.

Zaznaczona linia **rozjaśnia się do czystej patyny** (`--accent`), zamiast grubieć.
Skacząca grubość myliła — wyglądała jak zmiana kształtu, a nie stanu. Ta sama
zasada w przeglądzie i w generatorze (tam tor jest wpisany na sztywno w `#1B665E`).

### Nic nie wchodzi na bandę
Element był przycinany do ściany **swoim środkiem**, więc drąg trzymany za środek
kładł się połową na bandzie, a opona i „X" wjeżdżały na nią w całości.

Teraz `polElementu()` podaje, ile element naprawdę zajmuje od swojego środka —
wzdłuż własnej osi i w poprzek — a `zasiegElementu()` przelicza to na osie placu,
więc obrócony drąg zajmuje co innego wzdłuż, a co innego w poprzek.
`granicaElementu()` daje z tego widełki dla środka, `wsunElement()` wsuwa
z powrotem na piasek.

Zasięgi (metry od środka, do sprawdzenia gdyby kształty się zmieniły):
drąg 0,16 w poprzek · kawaletka 0,49 · stacjonata 0,38 · okser rozstaw/2 + 0,25 ·
pachołek 0,65 × 0,63 · opona 0,67 · zatrzymanie 0,71 · litera 0,44 · inny 0,48;
wzdłuż wszystkie drągowe: długość/2 + 0,03.

Wsuwanie odpala się przy: przeciąganiu, obrocie, zmianie długości i rozstawu,
zmianie rozmiaru placu, wczytaniu gotowego ustawienia, wczytaniu schematu
**i przy starcie edytora** — inaczej schemat zapisany starszą wersją dalej
pokazywałby sprzęt na bandzie i wyglądało to jak niedziałająca poprawka.

Świadomie **nie** liczymy do zasięgu podpisów: numerka przeszkody i wysokości
w centymetrach. To adnotacje interfejsu, nie sprzęt leżący na piasku.

Testy: `testy16.mjs` (32 sprawdzenia — sześć typów sprzętu ciągniętych za bandę,
obrót, wydłużenie, zmniejszenie placu, wczytanie starego schematu, kolory toru).
Razem 433 sprawdzenia w szesnastu plikach.

### Uwaga dla drugiej sesji
Paleta marki wpadła do `generator.html` i `przeglad.html` już dwa razy i dwa razy
została nadpisana przy przebudowie tych plików. Wartości są w tabeli wyżej —
przy każdej przebudowie trzeba je przenieść, bo inaczej wracają stare `#35A79C`,
`#6FB36F` i granatowy `#4F57A6`.

## Rysowanie po figurze zaczyna nową linię (7 września)

Punkty figury siedzą w `stan.tor` jak każde inne, więc punkt dopisywany na końcu
listy lądował w tym samym odcinku, co ostatnio wstawiona wolta — i trasa sama
wyjeżdżała z figury. Nikt o to nie prosił.

Teraz punkt postawiony zaraz po figurze dostaje `przerwa: true` i zaczyna własną
linię. Kolejne punkty dopisują się już normalnie do niej — przerwa wchodzi tylko
wtedy, gdy ostatni punkt toru należy do figury.

Wyjazd z figury został tam, gdzie był: dotknięcie jej końca w trybie Tor włącza
„Rysuję od figury" i dopiero wtedy trasa wychodzi z wolty. To ma być decyzja,
a nie skutek uboczny kolejności klikania.

Przy automatycznej przerwie edytor mówi wprost: „Nowa linia obok figury · żeby
jechać dalej od figury, dotknij jej końca" — bo inaczej wyglądałoby to jak
zgubiony punkt.

Testy: `testy17.mjs` (19 sprawdzeń) — razem 452 sprawdzenia w siedemnastu plikach.

## Wejście w tryb Tor mówi, że są dwie drogi (7 września)

Usunięte: ostrzeżenie „X m od bandy — mało miejsca na wyjście" wraz ze stałą
`MIN_OD_BANDY`. Decyzja Luizy — na małym placu odzywało się przy połowie ustawień
i przestało cokolwiek znaczyć. Pozostałe ostrzeżenia (sprzęt na sobie, trasa
przez przeszkodę, braki w sprzęcie) zostają.

Dodane: `podpowiedzTrybuToru()`. Przy włączeniu trybu Tor, jeśli na placu stoją
figury, edytor mówi wprost:

> Na placu są wolta i półwolta · dotknij placu tam, gdzie ma się zacząć trasa,
> albo dotknij końca figury, żeby jechać dalej od niej

Nazwy z `figuryNaPlacu()` (po jednej na rodzaj, w kolejności postawienia),
odmiana przez `wyliczenie()` i „jest / są". Ta podpowiedź wisi 9 sekund zamiast
3,8 — to instrukcja wyboru, nie potwierdzenie kliknięcia (`pokazPodpowiedz`
ma teraz trzeci parametr `dlugo`). Na pustym placu tekst zostaje stary.

Dodane też: w trybie Tor **oba końce każdej figury dostają obwódkę** — kreskowaną,
gdy tylko można je wybrać, i pełną w patynie, gdy z tego końca właśnie rysujemy.
Bez tego zdanie „dotknij końca figury" wysyłało w poszukiwania: końce wolty nie
różniły się niczym od reszty jej punktów. Przy okazji naprawione: wybranie końca
nie odrysowywało placu (`rysuj()` brakowało obok `odswiezKontekst()`), więc
podświetlenie pojawiało się dopiero przy następnej akcji.

Testy: `testy18.mjs` (18 sprawdzeń) — razem 470 sprawdzeń w osiemnastu plikach.

## Kasowanie toru nie rusza figur (7 września)

Błąd: „Cofnij punkt" robiło `stan.tor.pop()`, a punkty figury siedzą w tej samej
tablicy. Jeśli ostatnia w torze była wolta, przycisk zjadał ją kreska po kresce.
„Usuń tor" robiło `stan.tor = []`, czyli kasowało razem z figurami.

Zasada po poprawce: **figura jest elementem ustawienia placu, jak drąg — nie
kreską, którą się przy okazji rysowało.** Kasowanie toru jej nie dotyka. Figurę
usuwa się tak jak dotąd: dotknięcie i „Usuń" w pasku.

Trzy przyciski w pasku trybu Tor, wszystkie liczą tylko punkty spoza figur
(`indeksyNarysowane()`), wszystkie wygaszone, gdy nic narysowanego nie ma:

| Przycisk | Co robi |
|---|---|
| Cofnij punkt | zdejmuje ostatni postawiony punkt (`cofnijPunktToru`) |
| **Usuń linię** (nowy) | kasuje ostatnią narysowaną linię (`usunOstatniaLinie`) |
| Usuń tor | kasuje wszystko, co narysowane (`usunNarysowanyTor`) |

Wspólny `usunPunktyToru(indeksy)` wyjmuje punkty od końca i sprząta po nich:
punkt, który zostaje po skasowanym początku linii, przejmuje `przerwa`;
`kontynuacja.indeks` przesuwa się albo znika; pierwszy punkt toru nigdy nie
zostaje z `przerwa`.

Szczegół: „Usuń linię" na linii dorysowanej od figury („Rysuję od figury")
kasuje tylko dorysowane punkty, a figura zostaje cała — bo mieszkają w jednym
odcinku, ale to nie znaczy, że są jednym bytem.

Po „Usuń tor" zdejmujemy wszystkie `zlaczenie` i każda figura dostaje własną
linię: złączenia prowadziły przez skasowane odcinki, więc nie ma czego trzymać
razem, a zostawione znaczniki sklejałyby figury, które nigdy nie były sklejone.

Testy: `testy19.mjs` (31 sprawdzeń) — razem 501 sprawdzeń w dziewiętnastu plikach.

## Przeszkoda może stać obok trasy (7 września)

Dotąd o przynależności do przejazdu decydował sam rodzaj sprzętu: drąg, kawaletka,
stacjonata i okser zawsze były „na trasie". Wystarczyło postawić kawaletkę bliżej
niż 4 m od linii, żeby dostała numer i wpięła się w przejazd — a instruktorka
często rozstawia sprzęt obok: na następne ćwiczenie, na zapas, jako ograniczenie.

Nowa flaga `pozaTrasa` na elemencie i przełącznik **„Na trasie"** w pasku
(tylko dla tych czterech rodzajów — `moglbyByc()`). Wyłączony znaczy: bez numeru,
bez miejsca w kolejności przejazdu, bez wiązania z linią i bez ciągnięcia jej
za sobą przy przesuwaniu.

Cała decyzja siedzi w jednym miejscu — `przejezdzalny(e)` zwraca teraz
`!e.pozaTrasa && NA_TRASIE.indexOf(e.typ) >= 0`. Stąd bierze ją wiązanie punktu
(`przeszkodaPod`), samoczynne zakotwiczenie (`zakotwiczWTorze`), ciągnięcie toru
przy przesuwaniu (`torPrzyElemencie`) i numerowanie (`numeryZToru`, które
wcześniej sprawdzało `NA_TRASIE` na własną rękę — teraz woła `przejezdzalny`).

Wypięcie **nie rusza punktów toru** — zdejmuje samo wiązanie (`t.el`). Linia
zostaje dokładnie tam, gdzie ją narysowała ręka; gdybyśmy kasowali punkty,
znikałyby też te postawione świadomie. Wpięcie z powrotem woła `zakotwiczWTorze`.

Świeżo postawiona przeszkoda, która wpięła się sama, mówi teraz, jak to cofnąć:
„Wpięty w trasę — jeśli ma tylko stać obok, wyłącz «Na trasie» w pasku poniżej".

Świadomie **zostawione bez zmian**: „Rozstaw równo" i linia wymiarowa do sąsiada
działają też na sprzęcie obok trasy. To pomiar fizyczny między drągami, przydatny
niezależnie od tego, czy akurat się je przejeżdża.

Testy: `testy20.mjs` (25 sprawdzeń) — razem 526 sprawdzeń w dwudziestu plikach.

### Poprawka tego samego, po uwadze Luizy: „Oddziel od trasy"

Przełącznik „Na trasie" był do rozszyfrowania — świeci czy nie świeci, a co się
stanie po kliknięciu, trzeba było zgadnąć. Guzik nazywa teraz **czynność**:
„Oddziel od trasy", a po oddzieleniu „Wepnij w trasę". Stan stoi w opisie obok
nazwy elementu: „Kawaletka · obok trasy".

Druga rzecz: oddzielenie zdejmowało samo wiązanie, ale punkt, który edytor dołożył
przy wpinaniu, zostawał na linii — więc wyglądało to, jakby nic się nie stało.
Punkty wstawiane przez `zakotwiczWTorze` mają teraz `auto: true` i przy oddzielaniu
są wyjmowane. Punktów postawionych ręką (`t.el` bez `auto`) nie ruszamy — z tych
zdejmujemy tylko wiązanie, bo linia ma zostać tam, gdzie ją narysowano.

Testy: `testy20.mjs` urósł do 34 sprawdzeń — razem 535 w dwudziestu plikach.

## Okno figury: odmowa widoczna i odmieniona nazwa (7 września)

Dwa błędy z jednego zrzutu ekranu Luizy („po wpisaniu cyfry nie da się kliknąć
buttonu"):

1. **Martwy przycisk.** Przy średnicy mniejszej niż 3 m kliknięcie wołało
   `pokazPodpowiedz(...)`, a podpowiedź stoi na dole ekranu — **za zasłoną okna**.
   Z punktu widzenia użytkownika przycisk po prostu nie działał. Odmowa jest teraz
   czerwoną linijką w samym oknie, pod polem, i znika przy poprawianiu liczby.
   Osobny komunikat dla pustego pola („Wpisz średnicę…") i dla za małej liczby
   („2 m to za mało — najmniejsza figura ma 3 m średnicy").
   **Reguła na przyszłość: w oknie modalnym nie wolno tłumaczyć się podpowiedzią
   na dole ekranu.**

2. **„Wstaw wolta na plac".** Nazwa szła z `f.nazwa.toLowerCase()`, czyli mianownik.
   `FIGURY` ma teraz pole `biernik` (woltę, półwoltę, serpentynę) i guzik z niego
   korzysta. Serpentyna miała to dobrze tylko dlatego, że jej tekst był wpisany
   ręcznie przy przełączniku „od ściany do ściany".

Testy: `testy21.mjs` (20 sprawdzeń) — razem 555 w dwudziestu jeden plikach.

### Za duża figura też mówi w oknie (7 września)

Odmowa przy za dużej średnicy działała tak samo źle, jak przy za małej: leciała
do podpowiedzi na dole ekranu, schowanej za zasłoną okna. Teraz:

- **w trakcie wpisywania** — czerwona linijka w oknie: „30 m nie zmieści się
  na placu 40 × 20 m — największa wolta to 20 m";
- przy serpentynie sufit zależy od liczby łuków: „Przy 6 łukach na plac
  40 × 20 m wejdzie najwyżej 6,50 m — zmniejsz średnicę albo liczbę łuków";
- pole ma ustawiony `max`, więc strzałki zatrzymują się na suficie.

Sufit liczy `najwiekszaNaSrodku()` przez `wystanieFigury()` — a to buduje punkty
tą samą drogą, którą stawia je `postawFigure` (`zaczepDlaFigury` → `punktyFigury`).
Inaczej okno obiecywałoby rozmiar, którego postawienie by nie przyjęło.

Przy okazji: **`WYSTAJE_MAX` z 0,25 m na 0,05 m.** Ćwierć metra tolerancji dawało
sufit 20,5 m na placu szerokim na 20 m — czyli koło wystające za obie ściany.
Teraz tolerancja pokrywa tylko zaokrąglenia, a dwudziestometrowe koło na placu
20 m wchodzi dokładnie, dotykając obu ścian (to standardowa figura ujeżdżeniowa,
więc musi wchodzić). Odstęp od bandy przy **powiększaniu** to osobna sprawa
(`LUZ_BANDY` 0,5 m) — tam figura rośnie, tu wchodzi w gotowy wymiar.

Testy: `testy21.mjs` urósł do 35 sprawdzeń; `testy5.mjs` i `testy6.mjs` sprawdzają
teraz odmowę w oknie zamiast w podpowiedzi. Razem 570 w dwudziestu jeden plikach.

## Numery tylko tam, gdzie trasa naprawdę przechodzi (7 września)

`ZASIEG_TORU` wynosił **4 m** i mierzył od środka przeszkody do próbek linii.
Cztery metry to pół szerokości placu — wystarczyło postawić woltę, żeby dwa drągi
stojące zupełnie obok dostały numery 1 i 2, i nie było wiadomo, skąd się wzięły.

Teraz `ZASIEG_TORU = 0.9` (tyle samo co `PRZYLEGANIE`, którym linia zakotwicza się
sama) i mierzymy **do kształtu przeszkody** przez `odlegloscOd`, nie do jej środka.
Dzięki temu trzymetrowy drąg łapie się na całej długości, a nie tylko środkiem.

Wyjątek: przeszkoda, która ma **własny punkt toru** (`t.el === e.id`), dostaje numer
zawsze. Ktoś ją tam świadomie wpiął — miara jest wtedy nieistotna.

Skutki: znika też fałszywa odległość w pasku (lista przeszkód idzie z tej samej
funkcji `numeryZToru`). Drąg oddzielony przyciskiem „Oddziel od trasy" numeru nie
dostaje niezależnie od odległości — to sprawdza `przejezdzalny`.

Testy: `testy22.mjs` (14 sprawdzeń) — razem 584 w dwudziestu dwóch plikach.

## „Obróć plac" na wierzchu i łączenie linii z figurą (8 września)

### Obrót placu
Nowy guzik `#btnObrocGora` w nagłówku, obok Cofnij — ikona prostokąta ze strzałką.
Pozycja w menu zostaje, oba wołają wspólną `obrocPlac()`. Obracanie placu to ruch
robiony co chwilę, a nie ustawienie schowane na dnie menu.

### Dotknięcie figury w trybie Tor podłącza do niej trasę
Dotąd dotknięcie początku figury włączało `kontynuacja` z `przed: true`, czyli
„od teraz rysuję **przed** figurą". Kto miał już narysowaną linię, widział, że
edytor przeskakuje na woltę zamiast ją podłączyć.

Teraz `dolaczLinieDoFigury()`: dotknięcie **wjazdu w figurę** (jej pierwszego
punktu; przy wolcie oba końce leżą w tym samym miejscu, więc liczy się jako wjazd)
scala ostatnią narysowaną linię z figurą w jedną trasę. Nic się nie przesuwa —
między końcem linii a początkiem figury rysuje się dojazd. Styk dostaje
`zlaczenie`, więc później da się go rozłączyć jednym dotknięciem.

Po scaleniu `kontynuacja` wskazuje **drugi koniec figury**, więc kolejne dotknięcia
przedłużają trasę za woltą — trasa czyta się jak przejazd: dojazd, koło, wyjazd.

Bez narysowanej linii zostaje stare zachowanie (włącz „Rysuję od figury"), bo
nie ma czego do czego podłączać.

Testy: `testy23.mjs` (24 sprawdzenia) — razem 608 w dwudziestu trzech plikach.

## Pierwsze testy na telefonie Luizy (8 września)

### Wolta rozjechana po doczepieniu trasy — naprawione
`sciezkaToru` liczyła styczne Catmull-Roma z sąsiadów w tablicy. Punkt dojazdu
stojący przed woltą był takim sąsiadem i rozciągał jej pierwszy łuk — koło robiło
się jajkiem. Teraz punkty figury biorą sąsiadów **wyłącznie z własnej figury**
(granice liczone raz, w `odFig`/`doFig`), a figura zamknięta zawija się na swój
drugi koniec. **Zasada: kształt figury nie zależy od tego, co do niej doczepisz.**

### Reszta z listy
- **Litera** wyjeżdżała poza kwadracik: `dominant-baseline: central` bywa na
  telefonie ignorowane. Teraz `dy="0.34em"` i font 0,66 zamiast 0,72.
- **Okna** trzymają się góry ekranu (`align-items: flex-start`), bo wyśrodkowane
  uciekały spod palca, gdy chowała się klawiatura telefonu. To najpewniejsze
  wytłumaczenie „przycisk serpentyny nie działa" — w testach wstawia się poprawnie
  w każdym rozmiarze.
- **Wiersze w oknach**: pole liczbowe dostało `flex: 0 0 5rem`, podpis
  `white-space: nowrap`, a etykieta `flex: 1; min-width: 0`. Na wąskim ekranie
  ściskało się to, co ma stałą szerokość: pole robiło się węższe od pozostałych,
  a „w schemacie: 0" łamało się na trzy linijki.
- **Linijka**: „Linijka pomocnicza włączona" — bez tłumaczenia, że na prawdziwym
  placu jej nie ma. Luiza: „to jest logiczne".
- **„Ustawiaj co 25 cm" → „Przyciąganie co 25 cm"**, bo z poprzedniej nazwy nie
  dało się odczytać, o co chodzi.
- Z legendy odległości wypadło „Czasem właśnie o to chodzi".

### Do rozstrzygnięcia z Luizą (nie zgadywać)
1. **Przeszkoda wpięta w trasę ciągnie ją za sobą.** Luiza chce móc odsunąć drąg
   bez ruszania linii, ale to samo wiązanie sama zamawiała na początku projektu.
   Jest przycisk „Oddziel od trasy" — pytanie, czy to wystarczy, czy potrzebny
   inny domyślny ruch.
2. **Kolory odległości.** Zielony mówi „koń trafia bez zmiany tempa", a tempa
   aplikacja nie zna. Czerwony w reszcie interfejsu znaczy „usuń/anuluj", więc
   jako ocena odległości wprowadza w błąd. Do decyzji: ile kolorów, jakie nazwy,
   jakie progi — to wiedza jeździecka Luizy, nie moja.

Testy: `testy24.mjs` (18 sprawdzeń, w tym pomiar okrągłości wolty i układ okien
na ekranie 360 px) — razem 626 w dwudziestu czterech plikach.

### Decyzje Luizy z testu na telefonie — wykonane

**1. „Oddziel od trasy" na początku paska.** Nie zmieniamy zachowania (tor dalej
jedzie za wpiętą przeszkodą), zmieniamy widoczność: guzik stoi teraz zaraz przy
nazwie elementu, przed obrotami. Luiza: *„na telefonie po prostu go nie widziałam,
a to jest aplikacja na telefon, więc takie funkcje powinny być na początku"*.
**Zasada na przyszłość: kolejność w pasku układamy pod ekran 360 px — to, co
najważniejsze, ma być widoczne bez przewijania.**

**2. Dwa kolory ocen zamiast trzech.**
- **owies** — odległość wychodzi na pełną liczbę fuli (dawne `ok` i `blisko`);
  to, czy fula musi być wyciągnięta czy skrócona, mówi podpis, nie kolor;
- **pomarańczowy** — wypada pomiędzy dwiema liczbami fuli (dawne `zle`);
- **czerwony zniknął z ocen** i zostaje wyłącznie na „usuń / anuluj".

Znaki: `✓` i `–` (krzyżyk odpadł razem z czerwienią — to nie błąd, tylko inna
odległość). Klasy `ok/blisko/zle` w kodzie zostają, bo niosą treść podpisu;
zmieniło się tylko, na jaki kolor każda mapuje.

Legenda mówi teraz o pełnych fulach zamiast o tempie: aplikacja nie wie, jakim
tempem pojedzie koń, więc nie może twierdzić, że „trafia bez zmiany tempa".
To był mój błąd merytoryczny, wytknięty przez Luizę.

Testy: `testy24.mjs` (26 sprawdzeń) — razem 634 w dwudziestu czterech plikach.

## Druga tura uwag z telefonu (8 września)

- **Ikona obrotu placu** — kółko z dwiema strzałkami zamiast prostokąta ze strzałką.
- **Przyciąganie co 25 cm** dostało w menu drugą linijkę: „sprzęt staje na równych
  ćwiartkach metra". Sama nazwa nie tłumaczyła nic, a na telefonie nie ma dymka
  z podpowiedzią. Klasa `.wybor-lista button .pod` jest do wykorzystania wszędzie,
  gdzie pozycja menu wymaga zdania wyjaśnienia.
- **Okno „Odległości na trasie"** straciło pierwsze zdanie o długości trasy —
  ta sama liczba stoi w lewym górnym rogu ekranu.

### Łączenie figury z figurą
Dotknięcie końca serpentyny włączało „Rysuję od figury", a kolejne dotknięcie
wolty przestawiało rysowanie na woltę zamiast połączyć obie w przejazd.
`dolaczLinieDoFigury` szuka teraz źródła w kolejności: **grupa, od której właśnie
rysujemy** (`kontynuacja`), a dopiero potem ostatnia narysowana linia. Dzięki temu
serpentyna → wolta scala się tak samo jak linia → wolta.

### Odległość „pomiędzy" mówi, co z nią zrobić
Zamiast `2–3` i „2 długie albo 3 krótkie" jest **„2 fule wyciągnięte albo
3 skrócone"** (skrót w pasku: „2 albo 3"). Uwaga Luizy: skoro przy jednej ocenie
piszemy „wyciągnij albo skróć", to zapis `2–3` mówi to samo innym językiem
i tylko myli. Odmiana przez `odmianaFuli` i `rozciagniecie`, więc „1 fula
wyciągnięta albo 2 skrócone" też brzmi po polsku.

### Przesuwana linia zabiera wpięty sprzęt
Dotąd przeciągnięcie linii zostawiało drągi w miejscu, choć były w nią wpięte.
Teraz `przeszkodyOdcinka()` zbiera elementy związane z punktami tej linii, a
`zmiescOdcinekZeSprzetem()` przycina przesunięcie tak, żeby i linia, i sprzęt
zostały na piasku. **Zasada: wpięcie znaczy „to jedno" i działa w obie strony —
przeszkoda ciągnie trasę, trasa ciągnie przeszkodę.** Sprzęt oddzielony
(`pozaTrasa`, bez wiązania) zostaje na miejscu.

Testy: `testy25.mjs` (27 sprawdzeń) — razem 661 w dwudziestu pięciu plikach.

## Trzecia tura z telefonu (8 września)

### Nie dało się poprowadzić trasy między dwiema woltami
Zasięg chwytu punktu toru w trybie Tor wynosił 0,8 m dla **każdego** punktu.
Punkty figury stoją gęsto (na obwodzie wolty co ~2,5 m) i są rysowane drobno,
więc prześwit między dwiema woltami w całości mieścił się w zasięgu chwytu:
dotknięcie tam nie stawiało punktu trasy, tylko łapało figurę.

Zasięg zależy teraz od tego, co pod palcem:
- punkt postawiony ręką — 0,8 m (bez zmian),
- **koniec figury** — 0,5 m (ma narysowaną obwódkę o promieniu 0,4),
- **środkowy punkt figury** — 0,35 m (rysowany jako kropka 0,1).

Łączenie wolty z woltą działało już wcześniej — psuł je właśnie ten zasięg.

### Teksty
- „Przyciąganie co 25 cm" → **„Sprzęt staje co 25 cm"** (+ linijka „przy
  przesuwaniu trzyma się równych ćwiartek metra"). Luiza dwa razy powiedziała,
  że nazwa jest niezrozumiała — nazwa ma opisywać to, co widać, a nie mechanizm.
- **„wyciągnięte" → „wydłużone"** w całych ocenach: tak mówi Luiza, a to jej
  język jest tu wzorcem.
- Nagłówek okna odległości mówi teraz, **z czego liczymy**: „Ile fuli mieści się
  między przeszkodami. Liczone z fuli wzorcowej: koń duży 3,66 m · koń mały
  3,35 m · kuc 2,98 m." Aplikacja nie zna tempa konia i nie ma prawa udawać,
  że zna — podaje podstawę wyliczenia i tyle.
- Legenda: „mieści się tyle fuli" oraz „żadna pełna liczba fuli się nie mieści —
  2 albo 3 znaczy: dwie mocno wydłużone albo trzy mocno skrócone".

### Pomysł Luizy na MVP 2
Osobny **generator tempa**: gdyby aplikacja znała tempo, w jakim jedzie się
przejazd, liczba fuli zgadzałaby się jeden do jednego zamiast być szacunkiem
z fuli wzorcowej. Do rozważenia po MVP 1.

Testy: `testy26.mjs` (14 sprawdzeń, w tym prześwit między woltami) — razem
675 w dwudziestu sześciu plikach.

## Okno „Odległości na trasie" napisane od nowa (8 września, tekst Luizy)

Układ według jej własnego zapisu:

1. **Podstawa wyliczenia** — „Liczba fuli (skoków) między przeszkodami liczona
   jest ze standardowej długości fuli", a pod spodem lista: koń duży 3,66 m,
   koń mały 3,35 m, kuc 2,98 m. Potem jedno zdanie: „Ile wyjdzie naprawdę, zależy
   od tempa, jakim pojedziesz". Aplikacja nie zna tempa i mówi to wprost.
2. **Oznaczenia** — cztery, nie trzy: `✓` pasuje do standardowej fuli;
   `✓ wydłużone / skrócone` wyjdzie, ale fula musi być inna niż standardowa;
   `–` dystans niejednoznaczny („1 albo 2" = albo jedna mocno wydłużona, albo
   dwie mocno skrócone); `·` za blisko — na pełną fulę z lądowaniem i odbiciem
   jest za krótko. Ten czwarty wiersz był potrzebny: „za blisko" pojawiało się
   w ocenach, ale nigdzie nie było wyjaśnione.
3. **Przeszkody na trasie** — wiersze podpisane „przeszkoda 1 → 2", a nie samym
   „1→2".

Poprawiona też rzecz, której Luiza nie nazwała wprost, ale którą widać w jej
opisie: guziki z propozycjami odległości wzięła za „standardowe dystanse
referencyjne", bo wyglądały jak wiersze informacyjne. Mają teraz podpis
**„przesuń na"** — bo naprawdę przesuwają przeszkodę.

Nowe klasy: `.odl-wstep`, `.odl-fule`, `.odl-naglowek`, `.odl-legenda`,
`.odl-popraw`. Uwaga: znaki legendy mają klasę `.zn`, **nie** `.znak` —
globalne `.znak` to pastylka z obwódką z listy przykładów i psuła wygląd.

Testy: `testy26.mjs` urósł do 25 sprawdzeń — razem 686 w dwudziestu sześciu plikach.

### Oznaczenia słowami Luizy (8 września)

Legenda w oknie odległości jest teraz jej tekstem, przepisanym co do słowa:

- **✓ Pasuje** — idealny dystans, nic nie musisz robić.
- **✓ Wydłużone / Skrócone** — da się przejechać, ale musisz konia pogonić
  (wydłużyć krok) albo wyhamować (skrócić krok).
- **– Niejednoznaczny** — „1 albo 2" znaczy: albo mocno pogonić na jedną fulę,
  albo mocno wyhamować na dwie. *(dopisane przeze mnie — Luiza podała trzy
  pozycje, ale ten znak pojawia się w ocenach i bez wiersza w legendzie
  zostałby zagadką; do usunięcia, jeśli uzna inaczej)*
- **· Za blisko** — przeszkody stoją zbyt blisko siebie. Brak miejsca na
  lądowanie, pełny krok i ponowne odbicie.

Każde oznaczenie ma teraz nazwę przy znaku („✓ Pasuje", a nie samo „✓"), więc
da się o nim mówić. **Zasada: teksty jeździeckie piszemy jej językiem — „pogonić",
„wyhamować", „fula", „wydłużone" — a nie moim opisowym.**

Testy: `testy26.mjs` (27 sprawdzeń) — razem 688 w dwudziestu sześciu plikach.

### Hierarchia okna odległości (8 września)

Luiza: „popraw hierarchię wizualną, usuń niejednoznaczny". Zrobione:

- wiersz **„– Niejednoznaczny" wypadł z legendy** — zostają trzy oznaczenia,
  dokładnie te, które podała. Sam znak `–` przy ocenach zostaje (pomarańczowy
  chip „1 albo 2"); gdyby okazał się zagadką, trzeba będzie zmienić chipa, a nie
  dokładać wiersz z powrotem;
- **podstawa wyliczenia i oznaczenia siedzą w jednym cichym panelu**
  (`.odl-panel`, tło `--chrome-3`, font 0,78 rem). Czyta się je raz, więc nie mają
  konkurować z danymi. Długości fuli są w jednej linijce zamiast listy;
- **odległość jest największa w wierszu** (1,02 rem, półgruba), a podpis pary
  („przeszkoda 1 → 2") zszedł do drobnego wersalika. Wcześniej oba miały podobną
  wagę i oko nie wiedziało, co czytać najpierw;
- „Przeszkody na trasie" to teraz tytuł sekcji (`.odl-tytul`), a nie kolejny
  drobny wersalik jak „Oznaczenia" w panelu.

**Zasada: materiał pomocniczy (jak się liczy, co znaczą znaki) idzie w tło,
dane przejazdu na wierzch.**

Testy: `testy26.mjs` (31 sprawdzeń, w tym pomiar wielkości czcionek —
odległość musi być większa od podpisu pary i od legendy). Razem 692.

### Trzy stopnie oceny, trzy kolory (8 września)

Luiza: „chyba nie możemy zrobić takich samych kolorów, jeśli chodzi o to, że jest
ok i że się trzeba wydłużyć albo skrócić". Racja — dzień wcześniej sama zwinęłam
te dwa stopnie do jednego koloru i zgubiłam różnicę, która ma znaczenie w jeździe.

Skala bez czerwieni, bo ta znaczy w aplikacji „usuń":

| stopień | kolor | znaczy |
|---|---|---|
| `ok` | owies `#AFC42B` | pasuje, nic nie trzeba robić |
| `blisko` | neutralny `--ink-soft` | wyjdzie, ale pogoń albo wyhamuj |
| `zle` | pomarańczowy `--ostrzez` | niejednoznaczne albo za blisko |

Neutralny zamiast trzeciej barwy: „trzeba pogonić" to nie ostrzeżenie i nie
pochwała, więc nie zasługuje ani na owies, ani na pomarańcz. Patyny nie użyłam,
bo w palecie jest zarezerwowana na markę (tor, zaznaczenie, przyciski).

Odstępy: legenda `line-height` 1,7 i 0,55 rem między wierszami, panel 0,85 rem
paddingu, wiersze listy 0,7 rem. To tekst do czytania, nie tabelka.

Testy: `testy26.mjs` (38 sprawdzeń, w tym scenariusz z czterema odległościami
naraz — za blisko, pasuje, wydłużona, niejednoznaczna — i porównanie ich kolorów).
Razem 699 w dwudziestu sześciu plikach.

### Koniec ze ścianą tekstu (8 września)

Luiza: „za dużo tego tekstu, najważniejsze info ma być tylko, a teraz zrobiłeś
ścianę tekstu, którą trzeba przewinąć". Trzy tury poprawiania tekstów skończyły
się tym, że wyjaśnienia zajęły więcej miejsca niż dane.

Okno pokazuje teraz:
1. **jedną linijkę** podstawy — „fula wzorcowa · duże 3,66 m · małe 3,35 m",
   i tylko dla **wybranych** wielkości (`grupy()`), nie wszystkich trzech;
2. **odległości** — to jest treść okna;
3. na dole **zwinięte** „Co znaczą znaki?" (`<details class="odl-pomoc">`)
   z trzema oznaczeniami i zdaniem o tempie.

Zniknął panel `.odl-panel`, nagłówki „Oznaczenia" i „Przeszkody na trasie"
(w oknie o tytule „Odległości na trasie" ten drugi niczego nie wnosił).

**Zasada: wyjaśnienie czyta się raz, dane ogląda się za każdym razem. Wyjaśnienia
domyślnie zwinięte, nigdy nad danymi.** Dotyczy każdego okna, nie tylko tego.

Test pilnuje wysokości okna (`< 560 px` na scenariuszu z trzema przeszkodami),
więc kolejne dopisywanie tekstu od razu zapali się na czerwono.

Testy: `testy26.mjs` (37 sprawdzeń) — razem 698 w dwudziestu sześciu plikach.

## Szerokie okno, brakujący viewport i cztery kolory ocen (8–9 września)

### Powrót panelu, ale w szerokim oknie
Zwinięcie wyjaśnień pod „Co znaczą znaki?" Luizie się nie spodobało — wolała je
widzieć od razu. Wróciły, ale okno odległości otwiera się teraz jako
**`.okno.szerokie`** (720 px zamiast 460). Te same zdania przestały się łamać po
trzy razy: panel skurczył się z dziewięciu wierszy do pięciu, a guziki „przesuń na"
mieszczą się obok siebie. `pokazOkno(naglowek, budujTresc, szerokie)` ma trzeci
parametr; na telefonie działa media query `max-width: 560px`, która schodzi
o stopień z czcionkami panelu.

### Brakujący `<meta name="viewport">`
Plik zaczynał się od `<title>` — bez `<meta charset>` i **bez viewportu**.
Telefon układał więc stronę tak, jakby ekran miał 980 px, i skalował całość
w dół: stąd drobne napisy i to, że moje „telefonowe" testy tak naprawdę mierzyły
układ desktopowy (`window.innerWidth` = 980 przy viewporcie 390). Po dodaniu
`<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">`
telefon rysuje aplikację w prawdziwej skali. Dodany też `<meta charset="utf-8">`,
bo plik otwierany z dysku nie ma nagłówka HTTP z kodowaniem.

**Zasada: każdy plik aplikacji zaczyna się od charset i viewportu.** Do sprawdzenia
w `generator.html` i `przeglad.html`.

### Cztery stopnie oceny, cztery kolory
Decyzja Luizy z 9 września — i świadome odejście od wcześniejszej reguły
„czerwień tylko na usuń":

| stopień | klasa | kolor | znaczy |
|---|---|---|---|
| Pasuje | `ok` | owies `#AFC42B` | idealny dystans |
| Wydłużone / Skrócone | `blisko` | **patyna `#269186`** | da się, ale pogoń albo wyhamuj |
| Niejednoznaczny | `zle` | pomarańcz `#DE9A3C` | „1 albo 2" |
| Za blisko | `stop` | **czerwień `#D75E4C`** | fizycznie się nie da |

„Za blisko" dostało własną klasę `stop` (wcześniej pusty string), więc da się ją
pomalować i policzyć. W `najlepszaOdleglosc` kara za `stop` to 200 punktów —
propozycja poprawki nigdy nie zaproponuje odległości, której nie da się przejechać.

Czerwień na „za blisko" jest uzasadniona: to jedyna ocena, która mówi „nie tutaj",
a nie „jak to przejechać". Pozostałe czerwienie w aplikacji dalej znaczą „usuń".

Testy: `testy26.mjs` (41 sprawdzeń, w tym scenariusz z czterema odległościami
naraz i porównanie kolorów chipów z kolorami legendy). Razem 702 w 26 plikach.

## Sprzęt postawiony na sprzęcie (9 września)

Pachołek postawiony na stacjonacie leżał na niej na rysunku, ale dotknięcie łapało
stacjonatę. Przyczyna w `trafiony()`: brało **najbliższy** element, a `odlegloscOd`
liczy do kształtu — do trzymetrowego drąga jest zero centymetrów wszędzie wzdłuż
jego długości, do pachołka tyle, ile dzieli palec od jego środka. Drąg wygrywał
zawsze, choćby pachołek leżał dokładnie na nim.

Teraz `trafiony()` pyta dwa razy:
1. **od góry stosu** (`stan.elementy` od końca — to jest kolejność rysowania):
   czy palec stoi naprawdę na tym elemencie, czyli bliżej niż `promienDotyku(e)`
   (`polElementu().wpoprzek`, bo długość jest już w `odlegloscOd`);
2. jeśli nic — najbliższy w `ZASIEG` (0,85 m), tak jak dotąd.

Dzięki temu to, co leży na wierzchu, łapie się pierwsze, a delikatne dotknięcie
obok dalej trafia w najbliższy sprzęt. Żeby złapać przeszkodę pod pachołkiem,
dotyka się jej poza pachołkiem — trzymetrowy drąg daje na to sporo miejsca.

**Zasada: kolejność łapania idzie od wierzchu stosu, nie od odległości.**

Tekst oceny „Wydłużone / Skrócone" — wersja Luizy: „da się przejechać, ale musisz
wydłużyć lub skrócić krok konia".

Testy: `testy27.mjs` (16 sprawdzeń: pachołek na stacjonacie, opona na drągu,
łapanie przeszkody obok pachołka, dwa pachołki blisko siebie). Razem 718 w 27 plikach.

---

## Trzy ograniczenia domknięte (9 września)

### 1. Łączenie linii działa w obie strony

Dotąd `sprobujPolaczyc` sklejała wyłącznie **koniec pierwszej linii z początkiem
drugiej**. Jeśli obie linie zaczynały się w tym samym miejscu i rozchodziły
(albo obie tam kończyły), złączenia po prostu nie było — a z ekranu nic tego nie
tłumaczyło.

Teraz próbujemy czterech ustawień po kolei:

```
koniec mój → początek jego
początek mój → koniec jego
mnie odwrócić, potem koniec → początek
jego odwrócić, potem koniec → początek
```

Odwrócić wolno **tylko linię narysowaną ręką** (`daSieOdwrocic` = żaden punkt
grupy nie ma `figId`). Figury zostają w swojej kolejności zawsze: wolta,
półwolta i serpentyna trzymają swoje parametry na PIERWSZYM punkcie przebiegu
(`figTyp`, `figD`, `figLuki`, `figTx/figTy`, `figNx/figNy`), więc odwrócona
figura straciłaby możliwość przeliczenia się przy powiększaniu.

Testy: `testy28.mjs`.

### 2. Przeciąganie przeskakiwało o metr — naprawione

Znalezione przy okazji, poważniejsze od tego, czego szukałam. Dotknięcie linii
albo przeszkody otwiera dolny pasek kontekstowy, plac robi się o kilkadziesiąt
pikseli niższy i **ten sam punkt ekranu oznacza już inne miejsce placu**. Między
`pointerdown` a pierwszym `pointermove` wszystko skakało pod palcem o około metr.

Poprawka: przy PIERWSZYM ruchu (`if (!ruszyl)`) zapamiętujemy punkt zaczepienia
jeszcze raz, po zmianie układu — dla linii, figury, punktu toru i przeszkody.
Cztery miejsca, ta sama zasada.

**Uwaga na przyszłość:** wcześniej rozluźniłam tolerancje w testach zamiast
zbadać, skąd bierze się rozjazd. To był błąd — test, który przestaje trafiać,
zwykle ma rację.

### 3. Ósemka: ta sama przeszkoda mijana dwa razy to dwa skoki

Dotąd `numeryZToru` szukała dla każdej przeszkody **jednego** najbliższego punktu
toru, a `zakotwiczWTorze` odpuszczała, gdy przeszkoda miała już swój punkt
(`maPunktToru`). Efekt: w ósemce drąg w środku dostawał jeden numer, druga
odległość wokół niego w ogóle się nie liczyła, a przy przesuwaniu drąga jedna
gałąź trasy zostawała w tyle.

Teraz:

- `numeryZToru` szuka **wszystkich zbliżeń** — idzie po próbkach toru i otwiera
  nowe „przejście" za każdym razem, gdy odległość spada poniżej `ZASIEG_TORU`
  (0,9 m) po tym, jak wcześniej z niego wyszła. Przerwa w drodze większa niż 3 m
  też zaczyna nowe przejście, żeby dwa odcinki toru się nie zlały.
- Numery idą po przejazdach, nie po przeszkodach. Przeszkoda mijana dwa razy nosi
  oba: **„2·3"** — plakietka robi się wtedy pigułką zamiast kółka.
- `policzOdleglosci` dostaje listę PRZEJAZDÓW (`{e, nr}`), nie listę przeszkód,
  więc obie odległości wokół dwa razy mijanego drąga trafiają na listę. Para
  „ta sama przeszkoda pod rząd" jest pomijana — nie ma między nią a nią odstępu.
- Klamra „cztery drągi co 1,35" nie ma prawa przeskoczyć powrotu: sąsiednie
  przerwy scalają się tylko wtedy, gdy naprawdę się stykają (`przerwy[j].b ===
  przerwy[j+1].a`).
- `zakotwiczWTorze` kotwiczy KAŻDE przejście (`mijaniaToru` zamiast
  `mijanieToru`), idąc od końca, żeby wstawiony punkt nie przesuwał indeksów
  pozostałych. To samo przejście drugiej kotwicy nie dostanie: sprawdzamy, czy
  któryś z końców odcinka nie jest już wpięty w tę przeszkodę.

Testy: `testy29.mjs` (14 sprawdzeń). Razem 744 w 29 plikach.

### Czego NIE poprawiłam — bo się nie potwierdziło

W notatkach stało, że na rzadko klikanym łuku kotwica ściąga krzywą o 20–40 cm.
Zmierzyłam to (`pomiar2.mjs`, łuk `[{4,17},{20,3},{36,17}]`, drąg postawiony
dokładnie na krzywej): odchylenie kształtu **0,000 m**, a po zakotwiczeniu krzywa
mija drąg o **0,006 m**. Dla trzech różnych położeń wzdłuż łuku tak samo.

Ten „przesuw", który widać na ekranie, bierze się skądinąd: **przyciąganie co
25 cm** potrafi postawić drąg do pół metra od krzywej, a kotwica ląduje wtedy w
najbliższym punkcie KRZYWEJ, nie w środku drąga. Krzywa nie drgnęła — drąg nigdy
na niej nie stał. Notatka o tym ograniczeniu została skasowana jako nieprawdziwa.

---

## Najmniejsza figura i ósemka (10 września)

### Metrowe kółka są dozwolone

Luiza chciała wstawić woltę o średnicy 2 m i dostała odmowę; od 3 m w górę
działało. Jej argument: „może ktoś będzie chciał zrobić sobie takie ładne
kółeczko do czegoś". Ma rację — plac ma być kartką, a nie tylko planem przejazdu.
Trzy metry były progiem „wolty, którą da się przejechać", a to nie to samo,
co „figury, którą wolno narysować".

Nowy próg: **`MIN_FIG = 1` metr**, jedna stała zamiast rozsypanych po kodzie
trójek (okno, pasek kontekstowy, `srednicaZUchwytu`, `najwiekszaFigura`,
`najwiekszaNaSrodku`, atrybuty `min` w polach). Pół metra to już za mało — sama
tak powiedziała i tak zostało.

Testy: `testy30.mjs` 167–168.

### Ósemka to DWIE wolty, nie nowy typ figury

Luiza chciała połączyć dwie wolty w ósemkę i się nie dało. Jej własne
rozstrzygnięcie: „może po prostu wstawimy ósemkę jako osobny element… wtedy
składa się z dwóch wolt, a każdą z tych wolt można osobno zwiększyć lub
zmniejszyć, bo często też tak jest" — i od razu dopisała warunek: „jeśli to
spowoduje, że będzie mniej błędów, to lepiej".

Dlatego ósemka **nie jest nowym typem punktu toru**. Kafelek „Ósemka" wstawia dwa
zwykłe koła, każde ze swoim `figId`:

- wspólny zaczep w środku placu — to punkt styku,
- wspólna styczna `t` w poprzek osi placu,
- przeciwne normalne `n` — koła siadają po dwóch stronach styku i są obiegane
  w przeciwne strony,
- oba dostają `figOsemka: true`.

Dzięki temu cała istniejąca maszyneria (chwytanie, przesuwanie, przebudowa,
przerzucanie, cofanie) działa bez jednej nowej gałęzi — a każde koło zmienia się
osobno, dokładnie tak, jak chciała.

`figOsemka` robi jedną rzecz: przy przebudowie koło rośnie **od punktu styku**
(`odZaczepu`), a nie wokół własnego środka. Bez tego powiększenie jednej połowy
rozjeżdżałoby ósemkę. W pasku kontekstowym takie koło podpisuje się
„Koło ósemki", nie „Wolta" — żeby było wiadomo, co się właśnie zmienia.

Sufit: `najwiekszaOsemka()` — dwa koła po `d` zajmują `2d` wzdłuż placu, więc na
40 × 20 wejdą koła po 20 m. Odmowa mówi to wprost: „Dwa koła po 24 m to 48 m —
na plac 40 × 20 m wejdą koła po 20 m".

**Zasada, którą warto zapamiętać:** jeśli nowy kształt da się złożyć z tych,
które już są, składamy — zamiast dokładać typ, który trzeba obsłużyć w dziesięciu
miejscach.

Testy: `testy30.mjs` 169–171 (29 sprawdzeń). Razem 773 w 30 plikach.

---

## Ósemka jeździ w całości · widoczny zapas na lądowanie (10 września)

### Ósemka przesuwa się jako jedno

Pierwsza wersja rozjeżdżała się przy pierwszym przeciągnięciu: dwa koła to były
dwie niezależne figury, więc jedno jechało, drugie zostawało. Luiza: „docelowo
się tego nie nadusi, no to ono powinno przesuwać się w całości".

`figOsemka` przestało być `true`, a stało się **wspólnym numerem obu kół**
(kolejny `stan.nastFig`). Po nim:

- `drugieKoloOsemki(figId)` znajduje partnera (tylko gdy jest dokładnie jeden —
  stare zapisy z `figOsemka: true` nie sparują się przypadkiem w trójkę),
- `punktyDoPrzeciagniecia(figId)` zwraca punkty obu kół, więc `zmiescPrzesuniecie`
  przycina całą ósemkę razem i oba koła jadą o ten sam wektor,
- w pasku kontekstowym pojawia się **„Rozdziel ósemkę"** (`rozdzielOsemke`):
  koła zostają na miejscu, tracą `figOsemka` i od tej pory są zwykłymi woltami —
  jeżdżą osobno i rosną wokół własnego środka.

Średnicę dalej zmienia się **osobno dla każdego koła**, tak jak było ustalone.

**Ważne przy okazji:** koła jednej ósemki nie mogą się sklejać przez
`sprobujPolaczyc`. Stykają się z definicji, więc każde przesunięcie zawiązywało
na styku węzeł „Rozłącz" — czyli drugi, konkurencyjny sposób rozdzielania tej
samej figury. Jeden warunek w pętli (`taSamaOsemka`) to wycina; ósemkę można
nadal doczepić do narysowanej linii.

### Skąd się bierze „2 fule skrócone" — teraz widać w oknie

Luiza zakwestionowała ocenę: 9,94 m przy fuli 3,66 m wyglądało na dystans
z zapasem, a aplikacja pisała „duże 2 fule skrócone". Rachunek był poprawny,
tylko **niewidoczny**:

```
odległość = lądowanie + n × fula + odbicie
9,94 = 3,60 (zapas) + 2 × 3,17     ← koń musi skrócić fulę o pół metra
wzorzec dla 2 fuli: 3,60 + 2 × 3,66 = 10,92 m
różnica: −0,98 m  →  |roz| > LUZ_OK (0,5), ≤ LUZ_BLISKO (1,0)  →  „skrócone"
```

Te 3,60 m (`KONIE.duzy.zapas`; mały 3,30; kuc 3,00) to **nie jest zapas
bezpieczeństwa** — to droga zjedzona przez lądowanie po pierwszej przeszkodzie
i odbicie przed drugą. Nie da się jej wykorzystać na fule, więc do przeliczenia
zostaje 6,34 m na dwa skoki, czyli po 3,17 m zamiast 3,66 m.

Nagłówek okna mówi to teraz wprost:

> Odległość między przeszkodami to lądowanie + fule + odbicie. Liczę z wartości
> standardowych:
> koń duży: fula 3,66 m · lądowanie i odbicie 3,60 m

**Zasada:** jeśli instruktorka nie może odtworzyć naszej liczby na kartce, to nie
jest jej błąd — to znaczy, że schowaliśmy jedną ze składowych.

Testy: `testy31.mjs` (19 sprawdzeń: ósemka w całości, rozdzielanie, zwykła wolta
bez tego przycisku, nagłówek okna i rachunek 9,94 m). Razem 792 w 31 plikach.

---

## Rachunek fuli po jej myśli · zapas do ustawienia (10 września)

### Co było źle w moim rozumowaniu

Wytłumaczyłam, że odejmuję 3,60 m na lądowanie i odbicie — i Luiza od razu
pokazała, że to jest **idealizacja podana jak fakt**:

> „to wszystko zależy od konia, od temperamentu, od tego jaki ma wykrok
> i ciężko czasami stwierdzić, jakie będzie to wybicie"

I opisała, jak liczy naprawdę:

> „wiem, że mam 10 m, wiem, że mój koń robi jedną fulę na 3 m, czyli trzy fule
> się w tych 10 metrach zmieszczą i metr mi zostaje — i to jest to, co mam
> na wybicie i odbicie na tę drugą przeszkodę"

**Matematycznie to jest dokładnie to samo równanie.** `n = round((d − zapas)/fula)`
i „ile fuli się mieści, ile metrów zostaje" różnią się tylko tym, co jest daną,
a co resztą. Ale jej wersja jest **sprawdzalna w głowie**, a moja wymagała
zaufania do liczby, której nigdzie nie było widać.

### Rachunek pod każdą odległością

Każdy wiersz skoków dostał małą szarą linijkę (`rachunekWiersza`, klasa
`.rachunek`) w jej kolejności:

```
duże: 2 fule × 3,66 = 7,32 m · zostaje 2,62 m, potrzeba ~3,60
kuce: 2 fule × 2,98 = 5,96 m · zostaje 3,98 m, potrzeba ~3,00
```

Przy dwóch wielkościach są dwie linijki. **Drągi na ziemi nie dostają jej wcale**
— tam nie ma ani lądowania, ani odbicia, więc nie ma czego rozpisywać
(`w.naziemne`).

Uwaga techniczna: `.okno p` ma `max-width: 58ch`, przez co rachunek łamał się
w połowie. Stąd `max-width: none` w `.rachunek`.

### Zapas jest teraz ustawieniem, nie prawdą objawioną

`zapasKonia(k)` czyta `stan.zapasy[k]`, a w braku wpisu wraca do naszych
3,60 / 3,30 / 3,00. Liczy z niego **wszystko**: `ocen`, `najlepszaOdleglosc`
(propozycje „przesuń na") i rachunek w wierszu.

- okno **„Lądowanie i odbicie…"** — w menu „…" oraz pod linkiem **„Ustaw swoje"**
  w oknie odległości, dokładnie tam, gdzie człowiek kwestionuje liczbę,
- pole na każdą wybraną wielkość (1–8 m), przycisk „Przywróć standardowe",
- gdy wartość jest własna, nagłówek dopisuje „(twoje)".

Decyzja Luizy: domyślnie zostają nasze liczby, ale ma być widać, że da się je
zmienić — „nie wydaje mi się, że każdy będzie zadowolony".

### Okno znowu mieści się bez przewijania

Rachunki dołożyły wysokości i okno urosło do 712 px przy limicie 680
(`testy26.mjs`, wymóg z 9 września). Zamiast podnosić próg: link „Ustaw swoje"
wszedł do zdania o wartościach orientacyjnych zamiast stać osobno, skróciłam
oba akapity nagłówka, `.rzad` ma padding 0,62 rem zamiast 0,70, a `.rachunek`
margines 0,1 rem. Wynik: 674 px.

**Zasada:** kiedy dokładam informację do tego okna, muszę zabrać tyle samo
gdzie indziej — inaczej wraca ściana tekstu, którą Luiza już raz odrzuciła.

Testy: `testy32.mjs` (29 sprawdzeń: rachunek przy skokach, brak przy drągach,
nagłówek bez udawania pewności, własny zapas zmienia ocenę i rachunek,
„Przywróć standardowe", wejście z menu, dwie wielkości). Razem 821 w 32 plikach.

---

## Przeszkoda wpięta w figurę (10 września)

Luiza postawiła kawaletkę na kole ósemki i nie dało się jej wpiąć — ani przez
upuszczenie, ani przez „Oddziel od trasy" i „Wepnij w trasę".

Przyczyna siedziała w `zakotwiczPrzejscie`:

```js
// wnętrze gotowej figury zostawiamy w spokoju
if (a.figId && b.figId && a.figId === b.figId) return;
```

To była **słuszna zasada z błędnym wnioskiem**. Punktu toru w środku wolty
faktycznie dokładać nie wolno (Catmull-Rom robi z koła jajko). Ale z tego wynika
tylko tyle, że nie da się wpiąć przeszkody TAK JAK w zwykłą linię — nie, że nie
da się jej wpiąć w ogóle.

### Odwrócone wiązanie

Przy zwykłej linii to **tor trzyma przeszkodę** (punkt z `t.el = e.id`).
W figurze jest odwrotnie: **przeszkoda trzyma się figury**.

```js
e.naFig = { fig: figId, i: <odcinek w figurze>, t: <0..1>, dkat: <kąt wzgl. jazdy> }
```

- `naFigurze(pktFig, i, t)` liczy punkt i kierunek jazdy tą samą matematyką,
  którą rysuje `sciezkaToru` — z sąsiadami wyłącznie z tej figury i z zawinięciem
  na drugi koniec przy figurze zamkniętej. Bez tego przeszkoda leżałaby obok linii,
  a nie na niej.
- `wepnijWFigure` dosuwa przeszkodę dokładnie na linię i zapamiętuje kąt
  **względem kierunku jazdy** — dzięki temu po obrocie figury drąg dalej leży
  w poprzek trasy, a nie sztywno na północ.
- `odswiezWpiete()` odtwarza pozycję po każdej zmianie figury: przeciąganie
  (na żywo, w `pointermove`), przebudowa średnicy, przerzucenie, obrót.
- `(i, t)` jest stabilne przy zmianie średnicy, bo figura o tym samym typie ma
  zawsze tyle samo odcinków — dlatego powiększone koło zabiera przeszkodę ze sobą.

Wpięta przeszkoda liczy się do przejazdu (`wpieta` w `numeryZToru` uwzględnia
`naFig`), a „Oddziel od trasy" zdejmuje wpięcie razem z resztą.

### Dwie rzeczy przy okazji

**Przestawiona przeszkoda też się wpina.** Dotąd `zakotwiczWTorze` po upuszczeniu
wołało się TYLKO dla świeżo postawionego sprzętu (`ciagniety.swiezy`). Przeciągnięcie
istniejącej kawaletki na trasę nie dawało nic — to była druga połowa jej problemu.
Teraz wpina się każde upuszczenie po ruchu.

**Kolejność w `pointermove` ma znaczenie.** `delete ciagniety.naFig` musi stać
PO `zakotwiczWszystkie()`, nie przed. Postawione przed — a tak to najpierw
napisałam — kasowało wpięcie, po czym `zakotwiczWszystkie()` w tej samej klatce
wpinało przeszkodę z powrotem, bo ta wciąż stała na linii figury. Objaw:
przeciągnięta przeszkoda zostawała ze starym, nieaktualnym `naFig`.

Testy: `testy33.mjs` (25 sprawdzeń: wpięcie, jazda z figurą, powiększenie koła,
oddziel/wepnij, odciągnięcie, zwykła linia dalej po staremu).
Razem 846 w 33 plikach.

---

## „Połącz" zamiast celowania w linię (10 września)

Luiza: „w momencie kiedy mamy przeszkodę i wkładamy ją na któreś z tego, to myślę,
że powinna być opcja połącz, a nie dopiero jak się naduśnie trasę — trzeba ułatwiać
użytkownikom jak najbardziej się da".

Poprzednia poprawka wpinała przeszkodę **tylko wtedy, gdy trafiła w linię na 0,9 m**
(`PRZYLEGANIE`). Ten próg jest słuszny dla wpinania SAMEGO Z SIEBIE — inaczej
aplikacja wciągałaby w przejazd wszystko, co stoi obok. Ale kiedy człowiek sam
mówi „połącz", nie ma powodu wymagać od palca celności do dziesięciu centymetrów.

### Guzik pierwszy z lewej

Kiedy zaznaczona przeszkoda **nie trzyma się niczego**, a w promieniu
`POLACZ_ZASIEG` (4 m) jest jakaś linia albo figura, pierwszy guzik w pasku brzmi
**„Połącz z woltą / półwoltą / serpentyną / ósemką / trasą"** — nazwany po tym, co
naprawdę jest obok. „Oddziel od trasy" schodzi wtedy na drugi plan (`mini`), bo
w tym stanie nie jest tym, czego się chce.

`polaczZTrasa(e)` dosuwa przeszkodę na najbliższy punkt krzywej i wiąże:
w figurze przez `naFig`, na zwykłej linii przez punkt toru. `Cofnij` zdejmuje to
w całości — i przesunięcie, i wiązanie.

`Wepnij w trasę` (dla oddzielonych) dostało tę samą logikę: jeśli po zdjęciu
`pozaTrasa` kotwica sama nie złapała, przeszkoda jest dosuwana na najbliższą linię.
Wcześniej ten guzik potrafił tylko zdjąć etykietkę i nic więcej nie robił.

### Wpięta przeszkoda staje W POPRZEK trasy

Przez przeszkodę na trasie się PRZEJEŻDŻA, więc kiedy to my przesuwamy ją na linię,
ustawiamy ją prostopadle do kierunku jazdy (`dkat: 90`). Kąt jest liczony
**względem trasy**, więc po obrocie figury drąg dalej leży w poprzek.

Obrót ręką (`obroc`) przesuwa `naFig.dkat`, a nie tylko `e.kat` — bez tego pierwszy
ruch figury cofałby to, co przed chwilą zrobiła ręka.

Zwykła linia, na której przeszkoda już stoi, dalej działa po staremu: to tor
przychodzi do przeszkody (dokładany punkt), więc nic jej nie obracamy.

Testy: `testy34.mjs` (34 sprawdzenia). Razem 880 w 34 plikach.

---

## Warstwa wizualna: hover i strzałki (10 września)

### Hover w kolorze marki

Guziki miały tylko rozjaśnienie ramki — czyli praktycznie nic. Teraz pod kursorem
ramka i napis stają się **patyną** (`--accent`), a tło ciemnieje do `--chrome-2`.
Doszła stała `--accent-jasny: #2FAFA1` — jaśniejsza patyna pod kursorem na
wypełnionych guzikach.

Trzy rzeczy, o które trzeba było zadbać:

- **`@media (hover: hover)`** — bez tego na telefonie `:hover` przykleja się po
  dotknięciu i guzik zostaje podświetlony, choć palca dawno nie ma.
- **Czerwień „Usuń" nie ustępuje patynie.** Kolor niesie znaczenie („to kasuje"),
  więc hover nie ma prawa go zabrać. To samo dotyczy stanu włączonego (owies).
- **`:active { transform: translateY(1px) }`** — guzik ma się wcisnąć.

### Główny guzik okna wypełniony

„Wstaw woltę na plac" to jedyna rzecz, którą się w tym oknie robi — więc wygląda
jak guzik, a nie jak ramka: pełna patyna, napis w `--accent-ink`, waga 700.

### Własne strzałki przy polach liczbowych

Systemowy spinner wyglądał jak z panelu sterowania Windows. Zdjęty
(`::-webkit-inner-spin-button { -webkit-appearance: none }` + `appearance: textfield`),
a w jego miejsce `ozdobLiczby(korzen)` owija każde `input[type=number]` w `.liczba`
i dokłada dwa guziki z **szewronami rysowanymi tą samą kreską co reszta ikon**
(stroke 1.6, zaokrąglone końce).

Szczegóły, które trzeba było domknąć:

- guziki wysyłają `input` **i** `change` — inaczej walidacja w oknie („za duża
  średnica") nie zauważyłaby zmiany zrobionej strzałką;
- puste pole przy pierwszym kliknięciu w górę ustawia `min`, a nie zero;
- `tabIndex = -1`, bo klawiatura ma strzałki w samym polu i nie ma po co
  zatrzymywać się na guzikach;
- `ozdobLiczby` woła się z `pokazOkno` **i** z `odswiezKontekst` (stąd rozdzielenie
  na `odswiezKontekst` / `odswiezKontekstWewn`), bo oba budują pola od nowa;
- pola ukryte (wymiary placu w nagłówku) są pomijane.

Testy: `testy35.mjs` (21 sprawdzeń). Razem 901 w 35 plikach.

---

## Z komentarzy na telefonie (14 września)

Nowy tryb pracy: Luiza komentuje bezpośrednio w opublikowanym artefakcie, ja
odczytuję wątki i poprawiam. Sześć komentarzy, z tego trzy do zrobienia od ręki,
jeden duży i jeden odłożony.

### Menu rozwijane wychodziło poza telefon

Pierwszy komentarz brzmiał „wyświetla się za duże względem telefonu", ale rzecz
była gorsza od rozmiaru: `.wybor-lista` rosła do szerokości najdłuższej pozycji
(468 px na ekranie 390 px), bo `white-space: nowrap` nie pozwalał niczemu się
złamać, a `.pod` siedział jako element flexa OBOK nazwy, nie pod nią. Lewa połowa
napisów uciekała poza krawędź — „Wyczyść plac" było nieosiągalne.

```css
width: max-content;                                  /* bez tego zwija się do trzech liter */
max-width: min(calc(100vw - 0.8rem), 24rem);
max-height: min(72vh, 34rem); overflow-y: auto;
.wybor-lista button { flex-wrap: wrap; white-space: normal; gap: 0.15rem 0.9rem; }
.wybor-lista button .pod { flex: 1 0 100%; }         /* druga linijka ZAWSZE pod nazwą */
```

### Trzy pozycje z menu

- **„Obróć plac" — usunięte.** Ikona w nagłówku robi to samo; dublowanie tylko myli.
- **„Gotowe ustawienia…" — usunięte.** To jest materiał dla GENERATORA POMYSŁÓW,
  osobnej części aplikacji. `PRZYKLADY` i okno zostają w pliku, żeby było co
  przenieść, ale nic ich stąd nie otwiera.
- **„Sprzęt staje co 25 cm" → „Równe ustawianie".** Luiza: „totalnie nie rozumiem,
  co tutaj jest napisane, a nasze opisy mają być jak dla debila". Nowy podpis mówi
  o SKUTKU, nie o mechanice: „sprzęt sam trafia na równe odległości — łatwiej
  ustawić drągi w rzędzie. Wyłącz, żeby stawiać dokładnie tam, gdzie palec."

### Tryb „Numeruj" — numerować można wszystko

Luiza: „jak ja sobie tworzę tor na Hubertusa, to robię, że to przeszkoda trzy,
to przeniesienie pachołka cztery, to wolta pięć". Dotąd numer dostawała wyłącznie
przeszkoda, przez którą przechodzi tor, a kolejność brała się z przebiegu linii.

Nowy kafelek **Numeruj** obok „Zmierz" i „Tor". Dotykasz po kolei tego, co koń
robi — przeszkody, pachołka, wolty, zatrzymania — każde dostaje następny numer.

- Numer siada na elemencie (`e.nr`, pole już istniało) albo na PIERWSZYM punkcie
  figury (`t.figNr`); figura dostaje plakietkę w środku swojego prostokąta.
- Ponowne dotknięcie zdejmuje numer, a **wszystko powyżej zjeżdża o jeden** —
  „przeszkoda 4" po skasowaniu trójki naprawdę jest trójką.
- **Ręka wyłącza automat.** Gdy istnieje choć jeden ręczny numer, `numeryZToru`
  nie nadaje swoich. Inaczej na jednym placu stałyby dwie sprzeczne numeracje.
  „Wyczyść numery" w pasku wraca do automatu.
- Tryb wyklucza się z „Tor" i „Zmierz", a wzięcie sprzętu z paska z niego wychodzi.

Testy: `testy36.mjs` (29 sprawdzeń). Razem 930 w 36 plikach.

### Odłożone: konie po imieniu

Komentarz o wielkościach koni („co w sytuacji, jak mamy dużego, małego, średniego
konia… może zrobić to do imienia konia, ale jak będzie 30 koni, to za dużo").
Pytanie zostało bez rozstrzygnięcia — **przyjęte założenie: zostają trzy grupy,
a docelowo ich liczby (fula + lądowanie i odbicie) będą do wpisania, tak jak dziś
jest już samo lądowanie.** Lista „Moje konie…" na wzór „Moje place…" i „Mój
sprzęt…" czeka na moment, w którym imiona naprawdę będą potrzebne — kształt danych
jest ten sam, więc to dołożenie, nie przebudowa.

---

## Zapisywanie obrazka — dwie drogi, jeden guzik (14 września)

W oknie „Obraz do zapisania" stała prośba, żeby przytrzymać obrazek palcem,
i obietnica, że „w gotowej aplikacji będzie tu zwykły przycisk pobierania".
Obietnica była niepotrzebna — przycisk da się zrobić teraz, tylko musi wiedzieć,
gdzie stoi.

Strona chodzi w dwóch miejscach i każde zapisuje inaczej:

1. **Otwarta wprost z pliku** — zwykły `<a download>` z data-URL, koniec tematu.
2. **Strona podglądowa (artefakt)** — strona siedzi w ramce, w której przeglądarka
   blokuje pobieranie zaczęte przez samą stronę. Plik podaje tam platforma:
   `claude.use("downloads")` → `save({filename, data})`, a oglądający potwierdza.

```js
let zapisPlatformy = null;                       // namespace „downloads", gdy jest
function pobieranieMozliwe() {                   // czy jesteśmy poza ramką
  try { return window.self === window.top; } catch (e) { return false; }
}
// guzik pokazujemy, gdy działa KTÓRAŚ droga — martwy guzik jest gorszy od braku
if (!pobieranieMozliwe() && !zapisPlatformy) { a.hidden = true; /* instrukcja palcem */ }
```

Szczegóły, które musiały zagrać:

- platforma odpowiada **później** niż otwiera się okno, więc po jej odpowiedzi
  odświeżamy okno, jeśli już stoi otwarte;
- do `save()` idzie **Blob** (`canvas.toBlob`), nie data-URL — obrazek nie musi
  przechodzić przez tekst;
- `declined` to nie błąd, tylko „nie" — nic nie pokazujemy, nie ponawiamy;
- każdy inny błąd chowa guzik i wraca do instrukcji palcem, zamiast udawać,
  że coś się stało;
- nazwa pliku mówi, co to jest: `dosiad-40x20-z-odleglosciami-2026-09-14.png`,
  `-dla-jezdzca-`, `-kartka-`.

Artefakt deklaruje `capabilities: {downloads: true}`.

Testy: `testy37.mjs` (21 sprawdzeń: pobieranie z pliku, nazwy trzech wersji,
ramka bez platformy, ramka z platformą). Razem 967 w 37 plikach.

---

# PRZEBUDOWA TRASY (14 września) — najważniejsza zmiana w projekcie

Luiza po trzech tygodniach walki: „cały czas ten tor się rozjeżdża, ja dodaję
przeszkodę, one się albo dodają, albo nie dodają". Spojrzała, jak to robi
konkurencja (parkour.design) i miała rację — problem nie był w dwudziestu błędach,
tylko w jednej decyzji projektowej pod nimi.

## Co było źle

**Trasa była odręcznym rysunkiem, a przeszkody musiały się do niego przykleić.**
Przy takim modelu „wpięło się czy nie" zależy od tego, czy palec trafił w linię
z dokładnością do 90 cm (`PRZYLEGANIE`). Wszystko, co budowaliśmy przez trzy
tygodnie — kotwice, `naFig`, „Połącz z woltą", „Oddziel od trasy", sklejanie
i rozłączanie linii, podwójne przejazdy — było leczeniem objawów tej jednej
decyzji. Każdy z osobna dawało się naprawić i za każdym razem wyglądało to na
ostatni raz.

**Mój błąd:** widziałam objawy, nie przyczynę. Luiza zobaczyła ją, patrząc na
konkurencję.

## Co jest teraz

**Trasa to KOLEJNOŚĆ rzeczy, przez które koń jedzie. Linia jest jej skutkiem.**

- kolejność siedzi w `e.nr` (przeszkody) i `t.figNr` (figury),
- `krokiTrasy()` → kroki posortowane po numerze,
- `torPrzejazdu()` → punkty linii: przeszkoda daje jeden punkt, figura cały swój
  przebieg (żeby linia naprawdę przez nią szła, a nie ucinała rogi),
- `rysuj()` liczy tę linię od nowa przy każdym rysowaniu.

**Przeszkoda nie może wypaść z toru, bo nie ma czego wpinać.** Przesunięcie
przeszkody przesuwa linię, bo linia nie jest osobnym bytem. Cała klasa błędów
zniknęła — nie została naprawiona, tylko przestała być możliwa.

**Rozstawienie ≠ trasa.** Postawienie przeszkody NIE dopisuje jej do przejazdu.
Plac rozstawiasz, kiedy chcesz; kolejność układasz osobno, w trybie „Trasa"
(dawne „Numeruj" — to było to samo pod inną nazwą, brakowało tylko linii).

**stan.tor trzyma dziś już tylko geometrię figur.** Wolta, półwolta, serpentyna
i ósemka to kształty na placu, nie przejazd.

## Szczegóły, które musiały zagrać

- **Wolta jest kołem**, więc wolno ją obrócić tak, żeby koń wjeżdżał od strony,
  z której nadjeżdża. Bez tego linia przecinała koło na pół, żeby dojść do jego
  zaczepu (trasa 58 m → 52 m na tym samym układzie).
- **Strzałki kierunku** na każdej przeszkodzie w trasie, liczone ze stycznej
  linii. Rysowane w osiach placu, nie przeszkody — mówią o kierunku jazdy,
  a nie o tym, jak leży drąg.
- **Znacznik startu**: kropka i strzałka przed pierwszym punktem.
- **Odległości tylko między kolejnymi przeszkodami.** Figura w środku przerywa
  liczenie (`przerwij`), bo przez łuk nie liczy się fuli.
- **Długość trasy** to długość PRZEJAZDU, nie suma wszystkiego, co narysowane —
  figura stojąca obok trasy nie jest drogą, którą koń przejedzie.
- **Gałka obrotu, nie duże koło.** Luiza: duże koło na tym placu znaczy woltę,
  dwa okrągłe kształty o różnych znaczeniach to gotowa pomyłka. Więc mała gałka
  na krótkim ramieniu. „Obróć o 90°" zostaje, bo jest precyzyjny.
- **Łuk albo kąt** (`e.ostryTor`) na każdej przeszkodzie trasy — Luiza chciała to
  zachować przy przejściu na kolejność.
- **Przyciąganie „od centymetra" odrzucone.** Drobniejsza siatka nie robi linii
  równiejszych, tylko trudniejszymi do trafienia. Równość bierze się stąd, że
  linia przechodzi przez środki przeszkód.

## Co wypadło

Rysowanie toru ręką, sklejanie i rozłączanie linii, kotwiczenie, `naFig`,
„Połącz z trasą", automatyczne numerowanie z przebiegu linii, „Popraw punkty",
„Usuń linię / Usuń tor". `trybTor` został jako stała `false` — kilkadziesiąt
gałęzi pyta o niego i są dziś nieosiągalne; do sprzątnięcia przy okazji.

## Testy

Model się zmienił, więc jedna trzecia zestawów opisywała nieistniejące
zachowanie. Nie da się ich „naprawić" — zostały wycofane do `stare/`
(z notatką, po co tam leżą), a w kilkunastu innych zasiewy przerobiono
z `tor:[{el:N}]` na `nr:` przy elementach.

- wycofane w całości: testy, testy9, testy11, testy13, testy17, testy19,
  testy22, testy28, testy29, testy33, testy34
- następca: **`testy38.mjs`** — trasa z kolejności (37 sprawdzeń)
- razem: 26 zestawów, 615 sprawdzeń, wszystkie zielone

## Poprawki zaraz po przebudowie (14 września, wieczór)

Luiza zobaczyła nową trasę na telefonie i zgłosiła trzy rzeczy:

**„W jaki sposób mogę zacząć trasę, bo nie widzę żadnego startu?"** — to było
najważniejsze. Model jest prostszy, ale niewidoczny: nic nie mówiło, że trzeba
wcisnąć „Trasa". Podpowiedź startowa mówi to teraz wprost i zmienia się zależnie
od tego, co stoi na placu (`podpowiedzNaStart`): pusty plac → „postaw sprzęt";
sprzęt bez przejazdu → „dotknij Trasa i dotykaj przeszkód po kolei"; gotowy
przejazd → „dotknij Trasa, żeby zmienić kolejność". Postawienie sprzętu też mówi
od razu: „stoi na placu · przejazd ułożysz osobno".

**Zasada:** kiedy upraszczamy model, rośnie dług na wyjaśnieniu. Rzecz, której
nie widać, trzeba powiedzieć.

**Gałka obrotu była czarną plamą** — `--chrome-2` na piasku wyglądał jak dziura.
Teraz jasna (`--drag-jasny`) z patynową obwódką i strzałką obrotu.

**Dwa guziki po 15° wypadły.** Obrót palcem robi to samo dokładniej, a pasek
kontekstowy na telefonie ma 390 px widoku i przewijał się na 1299 px. Po zmianie
1165 px — dalej za dużo, do skrócenia przy następnej okazji. „Obróć o 90°"
zostaje, bo jest precyzyjny.

Testy: `testy20` 117 przepisany na nowy model. Razem 26 zestawów, 613 sprawdzeń.

## „Start" jako element placu (14 września)

Luiza: „musimy po prostu mieć taki sam start, jak jest w konkurencji — robisz
start, a od tego startu robisz trasę". I miała rację: zastępczy znacznik na
początku linii to nie to samo, co rzecz, którą się stawia i obraca.

**Start jest teraz osobnym typem elementu** (`TYPY.start`), z własnym kafelkiem
na samym początku paska — przed sprzętem, bo od niego się zaczyna. Stawiasz,
przeciągasz, obracasz gałką w stronę jazdy. Rysowany jak u konkurencji: kropka,
strzałka i podpis „Start".

- **nie jest przeszkodą** (nie ma go w `NA_TRASIE`): bez numeru, bez odległości,
  bez „Oddziel od trasy". Od startu do pierwszej przeszkody nie liczymy fuli —
  `przejazdyDoOdleglosci` przerywa szereg (`k.start`).
- **otwiera przejazd**: `krokiTrasy()` wkłada go przed wszystko z numerem
  (`nr: 0`). Linia biegnie od niego i jedzie za nim, gdy go przesuniesz.
- **jest jeden**: drugi się nie dokłada — zamiast tego zaznacza się ten, który
  stoi, żeby dało się go od razu przesunąć. „Powiel" przy starcie nie istnieje.
- **zastępczy znacznik zostaje** dla przejazdu bez postawionego startu, ale
  przygaszony (`opacity: 0.55`) i znika, gdy prawdziwy Start stanie na placu.
  Dwa starty na jednym placu to gotowa pomyłka.

Podpowiedź startowa prowadzi teraz przez całą drogę: „Postaw «Start» tam, skąd
ruszasz, a potem dotknij «Trasa» i przeszkód po kolei".

Testy: `testy39.mjs` (25 sprawdzeń). Razem 27 zestawów, 638 sprawdzeń.

## Start stawiany palcem, nie na środku (14 września, wieczorem)

Luiza po pierwszej próbie: „miałam przeszkodę na środku i wszedł na to start.
Przykleił się do tej przeszkody i musiałam ją odkleić. Czy nie lepiej zrobić
to tak, że po prostu jest informacja, że postaw start w miejscu, w którym
chcesz?".

**Kafelek „Start" nie stawia od razu.** Dotknięcie kafelka włącza czekanie
(`czekaNaPostawienie`) i mówi: „Dotknij placu tam, gdzie koń zaczyna przejazd".
Następne dotknięcie placu stawia start dokładnie tam. `dodaj(typ, {gdzie})`
kładzie element we wskazanym punkcie i nie ustawia `swiezy`, więc nic się do
niczego nie przykleja.

**Zasada:** element, który określa miejsce (start, miarka, zatrzymanie), ma się
pytać o miejsce. Środek placu to dobry domyślny punkt dla sprzętu, który się
potem rozstawia — dla startu jest po prostu zły, bo środek placu zwykle jest
zajęty.

**Dotknięcie startu w trybie „Trasa" tłumaczy, zamiast milczeć.** Wcześniej
przydzielało startowi numer, którego nikt nie widział, i wyglądało na zepsute.
Teraz: „Start jest zawsze pierwszy — teraz dotknij przeszkody, do której koń
jedzie". Pierwsza ponumerowana przeszkoda przedstawia się jako „pierwsza
przeszkoda po starcie".

## Pasek kontekstowy przestał się przewijać w bok

1165 px treści przy 390 px ekranu znaczyło, że połowy guzików nikt nigdy nie
zobaczył. Trzy zmiany:

- na wąskim ekranie (`max-width: 720px`) pasek się **zawija** zamiast przewijać
  (`flex-wrap: wrap`, `overflow-x: visible`), a kreski `.pion` znikają — przy
  zawijaniu rozdzielały nie to, co trzeba;
- podpis i pole liczbowe siedzą w jednej ramce `.para`, żeby zawijanie nie
  rozerwało „wys. cm" od jego pola;
- rzeczy, po które sięga się rzadko (**Powiel, Rozstaw równo, Zapisz jako moją**)
  chowają się pod guzik **„Więcej"** (`.rzadko`). Na dużym ekranie ramka jest
  przezroczysta (`display: contents`) i pasek wygląda dokładnie jak wcześniej.

Zmierzone na 390 × 844: pasek 142 px zamiast przewijanych 1165 px, plac urósł
z 454 na 498 px, nic nie wystaje poza ekran. Na 1400 px: jeden rząd, 55 px,
bez guzika „Więcej".

Testy: `testy39` urósł do 38 sprawdzeń (225 — start nie ląduje na sprzęcie,
226 — od startu da się poprowadzić trasę). Razem 27 zestawów, **664 sprawdzenia,
0 błędów**.

**Do zrobienia dalej:** martwy kod po przebudowie trasy (`trybTor` to dziś
`const false`, a `naFig`, `zakotwiczWTorze`, `sprobujPolaczyc`, `rozlacz`,
`usunOdcinek`, `polaczZTrasa`, `PRZYKLADY` są nieużywane) — do wycięcia, ale
osobnym podejściem, z pełną regresją po.

## Kropki wpięcia — trasę ciągnie się od kropki do kropki (14 września, noc)

Luiza przysłała zrzut z konkurencji i powiedziała wprost: „zrób takie kropki
dosłownie, że każda przeszkoda ma mieć kropkę, powiedzmy, z dwóch stron, volta
też, i że my możemy połączyć się do tej kropki, i wiadomo, że to przechodzi
przez sam środek przeszkody. W rzeczywistości te dwie kropki są zielone, dlatego
że są blisko siebie i jakby automatycznie się one chcą połączyć".

Numerowanie dotknięciem zostało, ale **głównym sposobem układania trasy jest
teraz ciągnięcie od kropki do kropki**.

**Kropka to miejsce, którym trasa wchodzi w element i którym z niego wychodzi.**

- **przeszkoda** (drąg, kawaletka, stacjonata, okser) — dwie kropki, po obu
  stronach drąga. Przez drąg przechodzi się na wylot, więc wybór kropki mówi
  tylko, z której strony koń nadjeżdża: wjazd jedną stroną znaczy wyjazd drugą.
- **pachołek, opona, litera, zatrzymanie** — cztery kropki dookoła, bo mija się
  je z każdej strony; wejście i wyjście są tu niezależne.
- **start** — jedna kropka, na grocie strzałki. Ze startu się tylko wyjeżdża;
  próba dociągnięcia DO startu mówi „Start jest zawsze pierwszy".
- **wolta** — cztery kropki na obwodzie. **Półwolta i serpentyna** — swoje dwa
  końce; wjazd od drugiego końca odwraca przebieg figury.

**Jak to działa pod palcem.** W trybie „Trasa" kropki są widoczne. Ciągniesz od
jednej do drugiej: za palcem idzie gumowa linia, a kropka w zasięgu **zielenieje
po obu stronach** — dokładnie to, co Luiza opisała. Puszczenie łączy: cel staje
zaraz za źródłem w kolejności (`nr`), a obie strony zapamiętują, którą kropką koń
wchodzi (`e.we` / `e.wy`, dla figur `t.figWe`).

**Linia idzie przez sam środek.** `torPrzejazdu()` daje dla przeszkody z wybraną
stroną TRZY punkty — kropka wjazdu, środek, kropka wyjazdu — więc najazd jest
prosty i przechodzi przez przeszkodę, zamiast ścinać jej róg.

### Trzy rzeczy, które się przy tym wydały

**Styczna Catmull-Roma nie mieściła się w swoim odcinku.** Liczona jako
`(p2 − p0) / 6` rozjeżdżała się, gdy sąsiedzi stali nierówno — a kropki wpięcia
stoją 90 cm od przeszkody, przy sąsiedzie oddalonym o 10 m. Linia strzelała w bok,
zanim wróciła. Teraz styczna jest ucinana do 0,4 długości odcinka. Przy równych
odstępach (punkty figur) wychodzi 0,31 — czyli nic się nie zmienia, wolty
zostają woltami.

**Ostrzeżenie „trasa przechodzi przez pachołek" nie odzywało się od przebudowy.**
Pytało `if (stan.tor.length > 1)`, bo tam kiedyś mieszkał cały przejazd. Po
14 września siedzą tam tylko figury, więc plac bez figur miał to sprawdzenie
wyłączone. Teraz pyta o `krokiTrasy()`. Przy okazji: pachołek WPIĘTY w przejazd
nie jest już zgłaszany — on ma być na linii („4 przeniesienie pachołka").

**Kropka musi być równo duża pod palcem, a nie równo duża w metrach.** 40 m placu
mieści się na telefonie w 370 px, więc kropka „30 cm" miała tam trzy piksele.
`metrNaPiksel()` przelicza promień i zasięg chwytu z pikseli — 7 px na komputerze,
6 px na telefonie, zasięg 20 px wszędzie.

### Czego celowo NIE zrobiłem

- **Kropki widać tylko w trybie „Trasa".** Plac ma zostać rysunkiem; na kartce
  dla jeźdźca kropki nie mają czego szukać (są usuwane z eksportu razem z gumą).
- **Dotknięcie kropki bez pociągnięcia numeruje to, co pod spodem.** Kropki wolty
  leżą na jej obwodzie — bez tego nie dałoby się już dotknąć samej wolty.
- **Wolta ma jeden punkt styku, nie dwa.** Pełne koło wraca tam, skąd wyszło, więc
  wjazd i wyjazd to fizycznie ta sama kropka: wygrywa ta dotknięta ostatnio.

Testy: nowy `testy40.mjs` — widoczność kropek, zielenienie w trakcie ciągnięcia,
budowanie trasy ze startu, linia przez środek, wolta, odmowa łączenia do startu
i samego ze sobą, kropki poza obrazkiem, rozmiar pod palcem.

## Kropki bez trybu „Trasa" (15 września)

Luiza: „jest ok, ale zróbmy to bez klikania w «trasa»". Słusznie — narzędzie,
do którego trzeba najpierw wejść, to narzędzie, którego się nie znajduje.

**Kropki są teraz na placu cały czas** i ciągnie się od nich wprost, bez żadnego
trybu. Tryb „Trasa" zostaje wyłącznie dla numerowania dotknięciem — komu tak
szybciej. Miarka chowa kropki, bo tam się mierzy, a nie układa przejazd.

To wywołało trzy kolizje, bo kropki weszły na teren, który już był zajęty:

**Gałka obrotu stała dokładnie na kropce.** Obie leżą „prosto w górę" własnych
osi przeszkody — i tamtędy przechodzi jeszcze linia przejazdu. Gałka poszła na
ukos (`SKOS_GALKI = -42°`) i na dłuższe ramię (`wpoprzek + ODSTEP_KROPKI + 0,95`).
Zmierzone: od najbliższej własnej kropki dzieli ją teraz 1,4–2,1 m, a łapie się
ją z 0,75 m — palec nie musi wybierać. Przy okazji gałka zeszła z linii przejazdu.

**Kropki figury leżały na jej linii**, a za tę linię przesuwa się całą woltę.
Teraz kropka jest odsunięta o `ODSTEP_KROPKI` na zewnątrz (koło — promieniście,
figura otwarta — w przedłużeniu swojego końca). Wjazd nadal liczy się do
prawdziwego punktu na obwodzie; kropka to tylko rączka.

**Samo odsunięcie nie wystarczyło**, bo zasięg chwytu kropki jest duży (palec,
nie kursor) i na telefonie sięga dalej niż odstęp. Dlatego `kropkaBlizejNizReszta()`:
na placu kropka wygrywa tylko wtedy, gdy palec jest NAPRAWDĘ bliżej niej niż
linii figury i niż kształtu jakiejkolwiek przeszkody. Zasada ogólna: przy dwóch
uchwytach w zasięgu wygrywa bliższy, a nie ten sprawdzany wcześniej.

Dotknięcie kropki bez pociągnięcia zaznacza jej element (w trybie „Trasa" —
numeruje, jak dawniej).

Razem 28 zestawów, **713 sprawdzeń, 0 błędów**.

## Dlaczego drąg nie miał kropek — koniec z „pozaTrasa" (15 września)

Luiza przysłała zrzut: start, przeszkoda 1, a dalej dwa drągi **bez kropek**.
„Dlaczego nie mogę do draga dołączyć trasy? Nie wyświetlają się te kuleczki."

Odtworzyłem to jednym stanem i wyszło od razu: te drągi miały `pozaTrasa: true`,
czyli były kiedyś „oddzielone od trasy". A `wszystkieKropki()` pomijało taki
sprzęt — więc kropek nie było, a kropki to dziś JEDYNY sposób wpięcia czegoś
w przejazd. Schemat robił się nie do naprawienia inaczej niż przez guzik w pasku,
którego trzeba było najpierw znaleźć i zrozumieć.

**`pozaTrasa` wypadło z aplikacji.** To był relikt po odręcznym torze, gdzie
znaczyło „nie przyklejaj się do narysowanej linii". W modelu z kolejnością
o byciu w przejeździe mówi **numer i tylko numer** — druga, niewidoczna flaga
mogła się z nim już tylko kłócić.

- stary zapis leczy się przy wczytaniu: `pozaTrasa` znika razem z numerem, więc
  sprzęt zostaje na placu, ale poza przejazdem — dokładnie tam, gdzie był;
- guzik w pasku nazywa się teraz po tym, co robi z numerem: **„Wyjmij z trasy"**
  (zdejmuje numer, reszta przesuwa się o jeden) albo **„Dopnij na końcu trasy"**
  (dokłada jako ostatni; w środek wpina się, ciągnąc od kropki);
- zniknął dopisek „· obok trasy" przy nazwie i guzik „Połącz z…" — ten ostatni
  rozwiązywał problem, którego już nie ma (trafianie w linię co do 10 cm).

**Zasada:** jeśli o czymś decyduje jedna rzecz, nie wolno trzymać obok drugiej,
która decyduje o tym samym po cichu. Numer mówi, czy sprzęt jest w przejeździe —
i nic więcej nie ma prawa tego zmieniać.

Martwe po tej zmianie: `przelaczNaTrasie`, `polaczZTrasa`, `trzymaSieTrasy`,
`najblizszaKrzywa`, `nazwaCelu`, `POLACZ_ZASIEG` — do wycięcia razem z resztą
reliktów odręcznego toru.

Testy: `testy20` 118 przepisany na nowy model (stary zapis leczy się, kropki są,
oba guziki działają), `testy24` 145 na nową nazwę guzika. Razem 28 zestawów,
**716 sprawdzeń, 0 błędów**.

## Pięć poprawek po pierwszym prawdziwym użyciu kropek (15 września)

**Ikona gałki obrotu była kleksem.** Składała się z dwóch ścieżek o źle dobranych
promieniach — łuk miał `A 0.19 0.19 … large-arc=1` przy cięciwie 0,335, czyli
prawie pełne koło, a „grot" był osobną łamaną, która się w nie wtapiała. Teraz
jedna funkcja `rysujStrzalkeObrotu()` rysuje łuk 300° od godziny trzeciej
i trójkątny grot na jego końcu, wszystko z jednego promienia — w każdym rozmiarze
wygląda tak samo. Gałka ma też stały rozmiar pod palcem (`promienGalki()`,
`chwytGalki()` liczone w pikselach): 11 px i na komputerze, i na telefonie,
zamiast 11 px i 3,6 px.

**Start miał dwie strzałki.** Rysował swoją (kropka + kreska + grot) i dostawał
drugą, od kierunku przejazdu — razem wychodził podwójny szewron. Strzałka
kierunku pomija dziś start: on jest kierunkiem sam z siebie.

**Kafelek „Trasa" wypadł.** Luiza: „usunąć trasa, bo trasa będzie robić się
automatycznie". Tryb do dotykania po kolei nie ma czego robić obok ciągnięcia od
kropki do kropki. `trybNumerowania` zostaje jako `const false` (jak `trybTor`),
żeby nie przepisywać kilkudziesięciu warunków. „Wyczyść numery" przeniosło się
do menu „…" jako **„Rozwiąż trasę"** — samo pojęcie było potrzebne, tryb nie.

**Wrócił wybór łuk / kąt** — nie zniknął, tylko chował się razem z paskiem
kontekstu, którego w trybie „Trasa" nie było wcale. Przy okazji dostaje go teraz
**wszystko, co ma numer**, a nie tylko drągi: na pachołku zawraca się najczęściej.
Tak samo guzik „Wyjmij z trasy" / „Dopnij na końcu trasy" — poza startem, bo
start jest początkiem z definicji.

**Dotknięcie poza piaskiem odkłada przybornik.** Luiza: „ktoś zrobi trasę, kliknie
na tło poza placem i znikają mu te kropki". Kropki to narzędzie, a plac ma dać
się obejrzeć tak, jak wyjdzie na kartce. Dotknięcie marginesu wokół bandy chowa
kropki i odznacza wszystko; dotknięcie piasku przywraca je.

**Zasada:** narzędzie, którego nie da się odłożyć, przestaje być narzędziem
i staje się częścią rysunku.

Testy: `testy36` wycofany do `stare/` (cały był o trybie „Trasa"), `testy38`
i `testy39` przepisane na ciągnięcie od kropki do kropki, `testy20` 116 odwrócony
(pachołek i opona MAJĄ guzik trasy), `testy40` urósł do 68 sprawdzeń — nowe 236
(chowanie kropek), 237 (jedna strzałka startu), 238 (ikona i rozmiar gałki),
239 (łuk/kąt dla wszystkiego z numerem). Razem 27 zestawów, **714 sprawdzeń,
0 błędów**.

## Kreska pod strzałką startu (15 września, wieczorem)

Luiza: „pod strzałką jest jakaś kreska pod tym kółkiem obok startu, usuń to".

To nie była ozdoba — to była **linia przejazdu prowadzona przez ŚRODEK startu**.
`torPrzejazdu()` dawała dla startu dwa punkty: środek i kropkę wyjścia. Start
rysuje na tym samym odcinku własną strzałkę, więc linia ją dublowała — a że
Catmull-Rom wygina pierwszy odcinek w stronę dalszego sąsiada, dublująca kreska
wychodziła spod grotu krzywo i widać ją było jako obcy ogonek.

**Przejazd zaczyna się teraz w kropce startu**, nie w jego środku. Start pokazuje
strzałką, dokąd koń rusza, i to jest początek linii — jedna kreska zamiast dwóch,
i linia rusza dokładnie tam, gdzie strzałka pokazuje.

**Zasada:** jeśli dwa różne rysunki mówią to samo o tym samym miejscu, jeden
z nich jest zbędny — i to ten wyliczony, nie ten narysowany świadomie.

Przy okazji start **schudł**: kółko 0,42 → 0,31 m, strzałka krótsza i cieńsza,
podpis 0,8 → 0,72. Nie jest przeszkodą, więc nie ma przeciągać na siebie uwagi.
Kropka wpięcia przesunęła się z 1,9 na 1,62 m, tuż za grot.

**Wyjście z układania trasy trzeba było powiedzieć.** Chowanie kropek dotknięciem
poza placem działało od rana, ale nikt nie miał skąd o tym wiedzieć. Teraz mówi
o tym podpowiedź po każdym połączeniu („ciągnij dalej albo dotknij poza placem,
gdy gotowe”) i podpowiedź przy gotowym przejeździe.

### Przegląd reszty — co sprawdziłem i co jest dobrze

Puszczony osobny przelot (`przeglad.mjs`) przez: cofanie po połączeniu kropek,
zapis obrazu, listę odległości, pusty plac, wstawianie wszystkich czterech figur,
proporcje na telefonie i przeciąganie przeszkody wpiętej w trasę. **Bez znalezisk.**
Dwa fałszywe alarmy z pierwszego przebiegu warto zapamiętać, żeby nie ścigać ich
drugi raz: okno figury wymaga wpisania średnicy (pusta daje komunikat, nie cichy
brak reakcji), a „Zapisz obraz" najpierw pyta o wersję (dla siebie / dla jeźdźca /
na ścianę) i dopiero potem generuje.

Plac zajmuje na telefonie 76% ekranu (640 z 844 px) — wcześniejsza obawa o „jedną
trzecią" była z innego zrzutu, przy otwartym pasku kontekstu.

Razem 27 zestawów, **721 sprawdzeń, 0 błędów**. Nowe w `testy40`: 240 (brak
drugiej kreski przy starcie, odchudzony start), 241 (podpowiedź o wyjściu).

## Linia przestała się łamać na przeszkodzie (15 września, noc)

Luiza: „linia się załamuje względem przeszkody, popraw, żeby łączyła się ze
strzałką, albo żeby to wyglądało estetycznie".

Trzy rzeczy naraz, wszystkie z jednej przyczyny — **krzywa nie miała gdzie się
wyprostować**:

**1. Styczna liczona osobno dla każdego odcinka.** `sciezkaToru()` liczyła styczne
per odcinek (`(p2 − p0)/6`) i dokładała im ucinanie do 0,4 długości odcinka.
W punkcie, gdzie spotykają się odcinek długi (dojazd zza placu) i krótki (metr do
przeszkody), wychodziły dwa różne wektory — i stąd dziób. Teraz styczna liczy się
**raz na punkt**, a długość bierze z cięciwy (1/3), więc oba odcinki wychodzą
z punktu w tę samą stronę. Przy okazji koło z równo rozstawionych punktów wychodzi
dokładniej niż przedtem (4% spłaszczenia zamiast 11%).

**2. Punkt może narzucić swoją styczną** (`p.styczna`). Najazd na przeszkodę
korzysta z tego: wjazd, środek i lądowanie mają wpisaną oś skoku, więc przez
przeszkodę linia jest **matematycznie prosta** (zmierzone odchylenie: 0,000 m),
a krzywa wchodzi w nią gładko. Start też — jego punkt ma styczną wzdłuż strzałki,
więc linia wychodzi dokładnie w jej kierunku.

**3. Metr najazdu to za mało.** Trasa szła przez kropki wpięcia, 0,85 m od
kształtu. Na taki odcinek krzywa nie ma jak się wyprostować. Wprowadzony
`BIEG_PRZEJAZDU = 2,4 m` — tyle prostego przed przeszkodą i za nią — przycinany
do 38% odległości od sąsiada, żeby w szeregu co trzy metry najazd nie wyszedł zza
poprzedniej przeszkody. Kropki wpięcia zostają tam, gdzie były (to uchwyty),
najazd jest osobnym, dłuższym punktem trasy.

**Strzałka kierunku straciła trzon.** Leżała dokładnie na linii przejazdu i przy
gładkiej linii wyglądała jak druga, gruba kreska obok niej. Został sam grot,
posadzony w wolnym miejscu między przeszkodą a jej kropką wyjścia (`.grot-kierunku`,
mierzone 0,019 m od linii). Kropki odsunęły się przy okazji z 0,85 na 1,1 m, żeby
grot miał gdzie usiąść.

**Wolta wybiera wjazd mądrzej.** Pełne koło wraca tam, skąd wyszło, więc wjazd
i wyjazd to jeden punkt. Dotąd wybierany był najbliższy temu, SKĄD się nadjeżdża —
i linia potrafiła przeciąć koło na pół, żeby dojść do wyjazdu. Teraz minimalizuje
sumę „skądś do wolty" + „z wolty dokądś". Uwaga na przyszłość: wolta postawiona
dokładnie NA linii między dwiema przeszkodami zawsze zostanie przecięta — to nie
błąd rysowania, tylko konsekwencja tego, że koło wraca do punktu wyjścia.

**Zasada:** krzywa potrzebuje miejsca. Jeśli dwa sąsiednie punkty stoją metr od
siebie, a trzeci dziesięć metrów dalej, żadne sztuczki ze stycznymi tego nie
uratują — trzeba dać jej dłuższy odcinek.

Zmierzone na V-ce (dojazd z góry z lewej, odjazd w górę w prawo): najostrzejszy
skręt na krok próbkowania **2,2°** — przy załamaniu byłoby kilkadziesiąt.

Testy: `testy40` 229 przeliczony na nowy najazd + sprawdzenie, że jest naprawdę
prosty; nowe 242 (krzywa nigdzie się nie łamie — mierzone kątem między krokami)
i 243 (grot siedzi na linii). `testy38` 216 przepisany na klasę `.grot-kierunku`
zamiast dopasowania po treści ścieżki. Razem 27 zestawów, **729 sprawdzeń,
0 błędów**.

## Strzałka skoku jak u konkurencji · łuki albo kąty na całej trasie (16 września)

Luiza przysłała zrzut konkurencji obok naszego: „proponuję zrobić takie strzałki
na górze jak ma konkurencja, dzięki temu też odejdzie nam problem tego, że przy
drugiej przeszkodzie są jakieś trójkąty zamiast najazdu".

**Skok rysujemy teraz GRUBĄ STRZAŁKĄ na przeszkodzie**, wzdłuż osi skoku, a
kropkowana linia jest tym, czym jest u konkurencji — drogą MIĘDZY przeszkodami.
Wcześniej wszystko było jedną kreskowaną linią z małym grotem, więc miejsce,
w którym krzywa przechodziła w prosty najazd, było widać jako dziwny dziób.
Teraz chowa się pod strzałką, a kierunek skoku czyta się z drugiego końca stajni.

- trzon o grubości zależnej od szerokości przeszkody (0,2–0,34 m), wypełniony grot;
- strzałka nigdy nie wychodzi poza swój prosty najazd — przy przeszkodach co trzy
  metry skraca się razem z nim (zmierzone: 1,88 / 1,70 / 1,88 m zamiast pełnych 3,4);
- kropki wpięcia zostają po jej obu stronach, więc dalej widać, gdzie się wpina.

**Łuki albo kąty dotyczą CAŁEJ trasy.** Luiza: „skoro mamy zaznaczone łuki, no to
niech łuki będą wszędzie". Ustawienie siedziało na każdej przeszkodzie osobno
(`e.ostryTor`) i schemat wychodził pomieszany — część zakrętów okrągła, część
ostra, mimo że guzik mówił „Łuk". Teraz jedno `stan.ostreKaty`:

- guzik w pasku nazywa się **„Łuki" / „Kąty"** (liczba mnoga nie przez przypadek)
  i przełącza wszystko naraz;
- to samo siedzi w menu „…" jako **„Ostre kąty na trasie"**, żeby dało się tam
  sięgnąć bez zaznaczania czegokolwiek;
- stary zapis leczy się przy wczytaniu: jeśli cokolwiek miało `ostryTor`, cała
  trasa staje się kanciasta, a flagi znikają. **Wyleczony schemat zapisuje się od
  razu** — wcześniej migracja żyła tylko w pamięci i stara flaga wracała przy
  każdym otwarciu, aż do pierwszej zmiany na placu (to samo dotyczy `pozaTrasa`).

Przy kątach prosty najazd zostaje — koń i tak wchodzi w przeszkodę prosto, a łamie
się dopiero na wyjściu z najazdu. To wygląda na schemacie sensowniej niż załamanie
dokładnie na drągu.

**Zasada:** ustawienie, które da się włączyć osobno w dziesięciu miejscach, prędzej
czy później będzie włączone w połowie z nich. Jeśli rzecz dotyczy całego rysunku,
ma być jednym przełącznikiem.

Testy: `testy38` 216 liczy przeszkody ze strzałką (strzałka to dziś dwie ścieżki),
218 przepisany na ustawienie całej trasy, nowe 218b (stary zapis się leczy);
`testy40` 239 i 243 dostosowane, nowe 244 (strzałka gruba i przez całą przeszkodę)
i 245 (w szeregu co trzy metry nie wychodzi poza najazd). Razem 27 zestawów,
**743 sprawdzenia, 0 błędów**.

### Przy okazji: scalenie z przemianowaniem na „Koniownik"

Publikacja odbiła się: w międzyczasie ktoś opublikował wersję z **nazwą zmienioną
z „Dosiad" na „Koniownik"** (tytuł, marka w nagłówku, podpis na obrazku
i — najważniejsze — klucze `localStorage` z `dosiad-*` na `koniownik-*`, ze
starymi czytanymi jako zapasowe, żeby nikomu nie zniknął plac z telefonu).

Scaliłem w jedną stronę: **wziąłem opublikowaną wersję za bazę** i nałożyłem na
nią swoje zmiany, zamiast publikować swoją i zgubić przemianowanie. Testy
przestawione z `dosiad-plac` na `koniownik-plac` (`czytajKlucz` czyta stary klucz
zapasowo, więc zasiewy działałyby i tak, ale odczyt po zapisie już nie).

**Zasada na przyszłość:** jeśli publikacja mówi, że jest nowsza wersja, nigdy nie
publikować swojej „na siłę" — przeczytać tamtą w całości i nałożyć na nią swoje
zmiany. Odwrotna kolejność cicho kasuje cudzą pracę.

---

## Menu „…" bez powtórzeń (16 września)

Prośba: *„usuń to z menu, bo mamy to już przy trasie, jeśli są jeszcze jakieś
inne opcje, które się powtarzają w dwóch miejscach, też je usuń"*.

Przejrzałem całe menu pozycja po pozycji. Wyleciały dwie:

- **„Ostre kąty na trasie"** (`btnKaty`) — dokładnie ten sam przełącznik co guzik
  **„Łuki" / „Kąty"** w pasku przy zaznaczonej trasie. Poszedł `<button>`, jego
  obsługa kliknięcia, funkcja `odswiezKaty()` i oba jej wywołania
  (w `przelaczKatyTrasy` i w bloku startowym). Ustawienie żyje dalej w
  `stan.ostreKaty`, tylko wejście jest jedno.
- **„Numery na trasie"** (`btnNumery`) — przełączał `numeryWlaczone`, a to pilnuje
  wyłącznie `numeryZToru()`, której dziś nikt nie woła: numery biorą się z `e.nr`,
  czyli z kolejności ustawionej na placu. Guzik nie robił nic widocznego.
  `let numeryWlaczone = true` jest teraz `const` z komentarzem — zostaje jako
  stała, bo warunek w rysowaniu wciąż ją czyta.

Przy okazji poprawiony nieaktualny komentarz, który twierdził, że obrót placu jest
i w nagłówku, i w menu (jest tylko w nagłówku).

Menu po sprzątaniu: *Litery ujeżdżeniowe ✓ · Równe ustawianie ✓ · Linijka co 5 m ·
Rozwiąż trasę · Lądowanie i odbicie… · Odbij układ… · Moje schematy… · Moje place… ·
Mój sprzęt… · Zapisz obraz · Wyczyść plac*.

**Zostawione świadomie:** „Lądowanie i odbicie…" otwiera się i z menu, i z odnośnika
„Ustaw swoje" w okienku odległości. To nie jest ten sam przełącznik w dwóch
miejscach, tylko jedno okienko z dwoma wejściami — do potwierdzenia z Luizą.

Testy: `testy38` 194 odwrócony (sprawdza, że `#btnKaty` **nie ma**), `probakaty.mjs`
bez odczytów `#btnKaty`, nowy **`testy40` 246** („Menu nie powtarza tego, co jest
przy trasie"): brak pozycji o kątach, brak pozycji o numerach, żadna etykieta w menu
nie występuje dwa razy, a pasek przy trasie nadal daje Łuki/Kąty. Przy pisaniu tego
testu wyszła usterka w zasiewie — `trzy()` nie nadawał `nr`, więc guzik Łuki w ogóle
się nie pokazywał; dodane `nr` i `we`.

**Regresja: 27 zestawów, 745 sprawdzeń, 0 błędów.** Opublikowane jako wersja 20.

**Zasada:** jedno ustawienie — jedno wejście. Drugie wejście „dla wygody" oznacza
dwa miejsca do utrzymania i pierwszy podejrzany, gdy coś się rozjedzie.

---

## Slalom, wybór numeracji, spłaszczony obraz (16 września)

Cztery rzeczy z jednej wiadomości Luizy, plus dwie znalezione po drodze.

### Pachołek i opona: mija się je, a nie przejeżdża

*„Z pachołkami musimy zrobić tak, żeby nie można było przez nie przechodzić —
slalom to sama nazwa mówi. A tutaj mamy tak, że przez pachołek można przejść.
Po najechaniu na te kropki robią się takie łuki jak do zrobienia slalomu."*

Dotąd KAŻDY element w trasie dostawał najazd przez własny środek — dla drąga to
prawda (przeskakuje się go na wylot), dla pachołka nie. Nowy podział:

- `OMIJANE = ["pacholek", "opona"]` — te nie mają najazdu ani grubej strzałki;
- `osOminiecia(e, skad, dokad)` daje JEDEN punkt obok pachołka, oddalony o
  `max(wzdłuż, w poprzek) + 0.8 m` (w praktyce 1,45 m od środka), ze styczną
  równoległą do kierunku jazdy;
- punkt nosi `omija: id`, a nie `el: id` — dzięki temu pętla rysująca strzałki
  (`if (!q.el) return`) w ogóle go nie widzi. Grubą strzałkę rysuje się na tym,
  co się skacze.

**Odsuwamy się PROSTOPADLE DO JAZDY, a nie w stronę samej kropki.** Kropka mówi
tylko, z której strony mijamy (rzut na prostopadłą decyduje o znaku). Gdyby punkt
brać wprost z kropki, a kropka wypadłaby na wprost nadjeżdżającego konia, linia
robiłaby hak zamiast łuku. Bez wskazanej kropki (np. po „Dopnij na końcu trasy")
mijamy lewą stroną — byle nie przez środek.

Wychodząc z pachołka zapisujemy `we`, a nie `wy`: pachołek ma JEDNĄ stronę
mijania, nie wjazd i wyjazd.

**Przy okazji:** slalom co 4 m dostawał czerwone „duże za blisko". Między dwoma
pachołkami nie ma ani lądowania, ani odbicia, więc `zbudujWiersz` przy elemencie
omijanym zwraca `oceny: []` — odstęp dalej widać, wyroku o fulach nie ma.
**To dotyczy też pary skok → pachołek** (jest lądowanie, nie ma odbicia); jeśli
Luiza uzna, że tam liczba fuli jednak ma sens, to jedna linijka do zmiany.

Numer pachołka siada teraz po PRZECIWNEJ stronie niż linia (`kierunek.omija`
niesie punkt mijania) — wcześniej plakietka lądowała na trasie.

### Numeracja: automatyczna albo własna

*„Mamy dać wybór naszemu odbiorcy, czy numeracja ma być automatyczna, to znaczy
równoznaczna z tym, co ustawimy jako pierwsze na planszy, czy będzie robić sobie
tę numerację indywidualnie."*

Kluczowe rozdzielenie: **kolejność jazdy to `e.nr` / `t.figNr` i nikt jej nie
rusza.** Własny numer to osobne pole `e.numer` / `t.figNumer` — to, co widać na
rysunku. Dzięki temu „przeszkoda 7, a jedzie się ją trzecią" jest do zapisania,
a trasa dalej wie, którędy biegnie.

- `stan.numeracja` — `"auto"` (domyślnie) albo `"wlasna"`;
- pozycja w menu **„Numeracja: automatyczna / własna"** z wyjaśnieniem pod spodem;
- `numerPokazywany(e)` i `numerFiguryPokazywany(t)` — jedyne miejsca, przez które
  numer trafia na rysunek i do tabeli odległości;
- pole „nr" w pasku pokazuje się TYLKO przy numeracji własnej, podpowiedź
  (`placeholder`) to numer z trasy, puste = wróć do automatycznego.

**Stare pole „nr" pisało wprost do `e.nr`, czyli przestawiało kolejność przejazdu.**
Wyglądało jak „mój numer", a było „mój numer i przy okazji inna trasa". Wypadło.

`Rozwiąż trasę` i zdejmowanie numeru kasują też `numer` / `figNumer` — inaczej
własny numer wracałby jak duch przy ponownym wpięciu.

### „Więcej" rozwijało pustą ramkę

*„Nie wiem, po co mamy tutaj opcję więcej, po naciśnięciu tego nic się nie dodaje."*

Przy zaznaczonym Starcie ramka „rzadkich" była pusta: Powiel jest tylko dla
nie-Startu, Rozstaw i Zapisz jako moją tylko dla skoków. Guzik stał i rozwijał
nic. Teraz na końcu budowania paska: `if (!rzadko.childElementCount) { wiecej.remove(); rzadko.remove(); }`.

Przy okazji strzałki przy polach liczbowych nazywały się „Więcej"/„Mniej" — czytnik
ekranu mówił to samo, co guzik obok. Teraz „Zwiększ"/„Zmniejsz".

### Obraz do zapisania był spłaszczony

*„Podgląd spłaszczony… spłaszczony podgląd też się pobiera."*

`zrobObraz` liczył płótno z `stan.dl × stan.sz`. **Plac postawiony w pionie rysuje
się obrócony i jego `viewBox` ma zamienione boki** — rysunek 0,555 wchodził więc
na siłę w kadr 1,645 i koła robiły się elipsami (zmierzone: rozciągnięcie ~3×).
Teraz proporcje biorą się z `stan.pion`, czyli dokładnie z tego, co jest w `viewBox`.

Dwie rzeczy przy okazji, obie widoczne dopiero po naprawie:

- **podpis skalował się z WYSOKOŚCIĄ obrazka.** Plac w pionie jest wąski i wysoki,
  więc napis rósł w poprzek kadru. Teraz skaluje się z szerokością i zwęża się,
  gdy i tak się nie mieści;
- **szerokość słowa „Koniownik" mierzono PO ustawieniu czcionki monospace**, czyli
  nie tą, którą jest napisane — podpis wchodził na markę. Każda mierzy się swoją.

### Rączki figur znikają z kartki

Punkty wolty i obwódka-uchwyt nie miały klasy, więc zostawały na obrazku: wolta
wychodziła jak sznur korali z kółkiem u dołu. Dostały `class="uchwyt"` i lecą
razem z `.kropka, .guma` przy eksporcie. Na ekranie zostają — bez nich nie da się
chwycić figury.

**Zasada:** to, czym się rysuje, nie jest tym, co się rysuje. Każda rączka, kropka
i gałka ma mieć klasę, po której da się ją zdjąć z kartki.

Testy: nowe `testy40` 247–255 (slalom mija bokiem i po właściwej stronie; pachołek
bez wskazanej kropki też omijany; opona tak samo; slalom bez wyroku o fulach;
pełny obieg numeracji z przeładowaniem; „Rozwiąż trasę" zabiera własne numery;
„Więcej" tylko gdy coś chowa; obraz w pionie i w poziomie trzyma proporcje;
rączki znikają z eksportu). Razem **27 zestawów, 791 sprawdzeń, 0 błędów.**
Opublikowane jako wersja 21.

---

## Po filmie Luizy: zapis obrazka, widoczny wybór numeracji, wyrównany „Więcej" (16 września, wieczorem)

Luiza nagrała film z artefaktu i wypisała sześć rzeczy. Po kolei.

### Zapis obrazka: „wyskakuje komunikat, że nie da się zapisać, nie ma podglądu"

U mnie te same wersje obrazka składały się bez mrugnięcia — więc problem nie był
w rysunku, tylko w tym, JAK go podajemy przeglądarce.

Rysunek szedł do `<img>` jako **adres blob:**. W okienku artefaktu strona chodzi
w piaskownicy i tam polityka bezpieczeństwa potrafi zablokować `blob:` w `img-src`;
`<img>` odpala wtedy `onerror`, czyli dokładnie ten komunikat bez podglądu.

Odtworzone: `page.route` z nagłówkiem `Content-Security-Policy: img-src 'self' data:`
(bez `blob:`) — **komunikat pojawia się co do słowa.** Teraz:

- rysunek idzie **najpierw jako `data:`**, a `blob:` został zapasowym wyjściem —
  przy błędzie `onerror` przełącza się na następne źródło;
- klon SVG dostaje własną `width`/`height` (24 px na metr) i `preserveAspectRatio`,
  żeby rasteryzacja nie zależała od tego, co przeglądarka sobie dopowie;
- środek `onload` jest w `try/catch`: jeśli płótno odmówi zapisu (w piaskownicy
  potrafi to być SecurityError), komunikat **mówi, co się stało**, zamiast udawać,
  że rysunek się nie złożył;
- gdy odpadną oba źródła, komunikat nazywa oba po imieniu.

Sprawdzone pod trzema politykami: bez `blob:` — działa; bez `data:` — składa się
przez `blob:`; bez obu — jeden zrozumiały komunikat.

**Zasada:** „u mnie działa" przy cudzym błędzie znaczy tylko tyle, że różnica jest
w środowisku. Trzeba odtworzyć środowisko, a nie powtarzać test.

### Numeracja: wybór ma być widoczny, a własna ma znaczyć puste

*„Nie każdy wie, że jak naduszy na numerację, to jest taka opcja jak własna
i opcja automatyczna. To, że dla nas coś jest logiczne, nie znaczy, że dla drugiej
strony też."*

Jeden guzik, który po kliknięciu zmienia napis, pokazuje **stan**, a nie
**możliwości** — kto nie wie, że są dwie, ten nie ma skąd się dowiedzieć. Teraz
w menu stoi nagłówek „NUMERACJA PRZESZKÓD" i pod nim dwie pozycje,
`role="menuitemradio"`, z ptaszkiem przy włączonej.

I druga rzecz: *„skoro jest to własne, no to te cyfry powinny zniknąć"*. Miała
rację — przy numeracji własnej rysunek jest **pusty**, dopóki człowiek nie wpisze
numeru. Wcześniej numer z trasy świecił dalej jako podkład, więc przełącznik
wyglądał, jakby nic nie robił. Pole w pasku ma teraz podpowiedź „—", a nie numer
z kolejności, bo tamten wyglądał jak wartość, którą pole przyjmie.

Tabela odległości idzie za rysunkiem: nie ma numeru na placu — nie ma go i w tabeli.

### „Więcej": strzałka nad tekstem

`display: inline-flex` bez `align-items` rozciągało strzałkę na całą wysokość
guzika i sadzało ją nad napisem. Dołożone `align-items: center`, `gap`, `line-height: 1`
i `svg { flex: none; display: block }`, a napis trafił do `<span>`, żeby były dwa
równorzędne dzieci flexa, a nie goły tekst i element. Test mierzy różnicę środków
napisu i strzałki — wychodzi 0 px.

### Ostrzeżenie o pachołku: numer przestał być usprawiedliwieniem

Na filmie widać to najlepiej: po przeciągnięciu z górnego pachołka na środkowy
kolejność zrobiła się 1 → 2 → 3 (na górze) → 4 (w środku), trasa pojechała prosto
w górę i **przeszła tuż obok pachołka nr 4 bez słowa**. Bo `sprawdzPlac` miało
wyjątek: `if (e.nr) return` — „skoro stoi w przejeździe, to linia ma przez niego iść".

To było prawdą, dopóki trasa jechała przez środek. Od dziś trasa **omija** każdy
pachołek w przejeździe, więc jeśli linia go dotyka, to znaczy, że mija go
**przy okazji, jadąc gdzie indziej** — i o tym trzeba powiedzieć. Wyjątek wypadł.

**Sama prosta jest poprawna:** przy kolejności 2 → 3, gdzie 3 stoi za 4, koń
naprawdę jedzie w górę obok 4 i wraca. Aplikacja nie ma prawa wymyślać objazdu —
ma pokazać, co wyszło, i zwrócić uwagę. Do decyzji Luizy, czy linia ma sama
omijać pachołki, których nie jedzie (wtedy schemat ukrywa konflikt zamiast go
pokazać — dlatego nie robię tego z własnej ręki).

Testy: `testy40` 234 rozszerzone o pachołek mijany przy okazji, 251 przepisane na
dwie pozycje w menu (z ptaszkiem, z pustym rysunkiem przy własnej, z powrotem
i z powtórnym wyborem tego samego), 253 dokłada pomiar wyrównania strzałki na
telefonie. Razem **27 zestawów, 798 sprawdzeń, 0 błędów.**

---

## Slalom z powrotem i napis „Start" (16 września, późno)

Luiza przysłała **odręczny rysunek** jako wzór: trzy pachołki, a wokół każdego
zamknięta pętla, pętle stykają się między pachołkami. To jest slalom przejechany
**tam i z powrotem** — w górę mija się pachołek jedną stroną, wracając drugą,
i dopiero te dwa przejścia razem robią pętlę. *„Bardzo często wraca się po tych
slalomach, więc to nie może nam się rozjeżdżać."*

### Przeszkoda może stać na trasie dwa razy

To była najgłębsza zmiana od przebudowy trasy. Dotąd **element = jedno miejsce
w kolejce** (`e.nr`). Teraz dochodzi drugie: `e.nr2` i `e.we2`.

- `krokiTrasy()` wypuszcza dla takiego elementu **dwa kroki**, drugi z `drugi:true`;
- `torPrzejazdu()` bierze stronę z `k.drugi ? drugieWe(e) : e.we`;
- `drugieWe(e)` domyślnie daje kropkę **naprzeciwko** pierwszej (`+ połowa liczby
  kropek`), bo powrót idzie drugą stroną — i to właśnie zamyka pętlę;
- `osOminiecia` i `osPrzejazdu` przyjmują numer kropki z zewnątrz, zamiast czytać
  `e.we` na sztywno;
- plakietka pokazuje **oba numery naraz**: „2·5". Obsługa dłuższego napisu
  (prostokąt zamiast kółka) siedziała w kodzie od dawna, nieużywana.

**Zawrót na końcu.** Dwa przejścia tego samego pachołka jedno po drugim znaczą
„objedź go i wracaj". Sam spline ścinał wtedy przez jego czubek — zmierzone
**0,73 m od środka**, czyli po pachołku. Dołożony punkt ZA pachołkiem, w kierunku
nadjazdu: teraz 1,39 m.

**Plakietka musiała się odsunąć.** Przy powrocie pętla otacza pachołek z obu stron
i „obok" przestaje istnieć — numer stoi teraz poza pętlą i poza kropkami
(`promień omijania + 1,5 m`).

### Ciągnięcie do czegoś, co już jest na trasie, znaczy POWRÓT

To jest ta zmiana, o którą naprawdę chodziło. Dotąd `polaczKropki` robiło
`wyjmijZKolejnosci(kb)` i wstawiało cel za źródłem — czyli **przestawiało go
w kolejce**. Na filmie Luizy widać skutek: przeciągnęła z górnego pachołka na
środkowy, numery się zamieniły, trasa pojechała prosto w górę i slalom się rozjechał.

Nowa reguła: **jeśli cel już ma numer, to ciągnięcie do niego dokłada mu drugie
przejście**, a nie przestawia. Tak się jeździ slalom tam i z powrotem i tak wraca
się na przeszkodę w parkurze. Przestawić dalej można — przez „Wyjmij z trasy"
i wpięcie od nowa.

Kropka, od której ciągniemy, mówi też, **które** przejście kontynuujemy: jeśli
trafia w `drugieWe`, to numerujemy od powrotu, a nie od pierwszego przejścia.

Dalej poszły za tym: `nastepnyWolnyNumer`, `zrobMiejsceOd`, nowe `zwolnijNumer`
(jedno miejsce w kolejce), `wyjmijZKolejnosci` (kasuje cały wpis, oba przejścia),
`zdejmijNumer` (kasuje **najpierw powrót** — inaczej nie dałoby się cofnąć samego
powrotu), `reczneNumery`, `saReczneNumery`, `wyczyscNumery`, `kropkiWTrasie`
(świecą obie strony) i guzik w pasku: **„Zdejmij powrót"**, gdy przejścia są dwa.

### Napis „Start" zawsze po przeciwnej stronie grotu

*„Po obróceniu strzałki napis Start na nią wchodzi."* Napis był kotwiczony na
sztywno końcem (`text-anchor: end`) w punkcie za grotem. Przy strzałce w lewo ten
punkt jest **po prawej** od kropki, a napis dalej rósł w lewo — czyli przez kropkę
i grot.

Teraz liczymy kierunek grotu **na ekranie** (plac w pionie jest rysowany obrócony
o 90°, o czym napis wcześniej nie wiedział i czytał się bokiem) i z niego wybieramy
i stronę, i kotwicę: grot w prawo → napis kotwiczony końcem, grot w lewo → początkiem,
grot prawie pionowo → środkiem, nad albo pod. Test sprawdza wszystkie osiem kątów
w obu ustawieniach placu: **zero kolizji**, napis zawsze czyta się poziomo.

Testy: `testy40` 256–261. Razem **27 zestawów, 828 sprawdzeń, 0 błędów.**

**Zasada:** kiedy człowiek rysuje ręką, jak coś ma wyglądać, to nie jest sugestia
stylu — to jest brakujący wymóg modelu. Tu rysunek mówił „ta sama przeszkoda dwa
razy", czego model po prostu nie umiał zapisać.

---

## Okrążanie po kropkach (16 września, noc)

Luiza zadała pytanie odpowiedzią: zamiast wybrać wariant, **narysowała w Paincie
na zrzucie z aplikacji**, jak ma wyglądać. Trzy przeszkody, wokół każdej zamknięte,
okrągłe kółko przechodzące przez **wszystkie cztery kropki**, kółka stykają się
i krzyżują między przeszkodami. Do tego jedno zdanie: *„przy opcji pachołków
i opony powinna być opcja zrobienia trasy po każdej kulce, żeby to wyszło okrągłe"*.

### Co z tego wynikało

Poprzednia wersja robiła przy powrocie **soczewkę**: dwa punkty boczne po 1,45 m
i krzyżówki daleko między pachołkami. Jej rysunek ma **koło**: linia wchodzi dolną
kropką, mija boczną i wychodzi górną — a dwa takie półobroty składają się na pełne
kółko przez cztery kulki. Krzyżówki wypadają wtedy tuż przy kropkach, nie w połowie
drogi.

Więc: przy przeszkodzie z **dwoma przejściami** każde przejście to nie jeden punkt,
tylko **trzy**: kropka poprzeczna od strony nadjazdu → kropka boczna → kropka
poprzeczna od strony wyjazdu. Punkty leżą dokładnie na kropkach (1,75 m), a nie
1,45 m jak przy zwykłym mijaniu.

**Przy jednym przejściu nic się nie zmieniło.** Luiza zatwierdziła pojedynczy
slalom (*„jak jest pojedynczy slalom, to jest ok"*), a wciąganie go na kropki
robiłoby haki tam, gdzie kropka wypada na wprost konia. Okrążanie włącza się
dopiero wtedy, gdy przeszkoda faktycznie jest mijana w obie strony.

### Ćwiartka koła potrzebuje dłuższej stycznej

Pierwsze podejście dało nie kółko, tylko **czterolistną koniczynę**: linia
przechodziła przez kropki na 1,75 m, ale między nimi siadała na 1,25 m.

Powód jest czysto matematyczny. `sciezkaToru` bierze długość stycznej jako
**jedną trzecią cięciwy** — to dobra wartość dla łagodnego łuku, ale dla ćwiartki
koła za mała. Dla łuku 90° właściwa długość uchwytu Béziera to `0,5523·r`,
a cięciwa ćwiartki to `r√2`, czyli **0,39 cięciwy**, nie 0,333. Punkty okrążania
niosą znacznik `okrag`, a odcinek między dwoma takimi punktami dostaje 0,3905.
Zmierzone po poprawce: odchyłka od promienia kropek w cztery strony świata
**0–2 cm**.

### Plakietka musiała wyjść poza kółko

Numer stał „obok" pachołka — a przy okrążaniu „obok" przestaje istnieć, bo linia
otacza go dookoła. Teraz odsuwa się o `promień kropek + 1,1 m`, a kierunku szuka
po punkcie BOCZNYM przejazdu (jedynym, który niesie styczną) — wcześniej brał
pierwszy lepszy punkt okrążania i lądował na pachołku.

Testy: `testy40` 262 (linia przechodzi obok KAŻDEJ z dwunastu kropek; kółko trzyma
promień we wszystkie strony świata) i 263 (pojedynczy slalom dalej mija bokiem po
1,45 m i nie okrąża). Razem **27 zestawów, 835 sprawdzeń, 0 błędów.**

**Zasada:** kiedy pytanie ma cztery sensowne odpowiedzi, rysunek użytkownika jest
szybszy niż ankieta. Ale rysunek trzeba przeczytać **geometrycznie**, a nie „mniej
więcej tak" — tu całą różnicę robiło to, że linia ma iść PRZEZ kropki, a nie obok.

---

## Każda kropka z każdą (17 września)

*„Przy pachołkach i oponach daj opcję, żeby każdą kropkę można było połączyć
z każdą. Z tej zielonej kropki nie mogę połączyć się z tą, która jest nad nią
po lewej stronie, a powinna być taka opcja — ale tylko przy pachołkach i przy
oponach."*

To jest ta sama prośba co poprzednio, tylko od strony **gestu**, a nie rysunku.
Poprzednio obchodzenie robiło się samo, gdy przejścia były dwa. Teraz ma być
sterowane ręką: idziesz kropka po kropce dookoła, ile razy trzeba.

### Model: lista przejść, nie „pierwsze i drugie"

`nr2`/`we2` wystarczały na powrót, ale nie na obejście — kropek są cztery, więc
przejść może być do czterech. Nowa postać:

- pierwsze przejście zostaje tam, gdzie było: `e.nr` / `e.we` (czyta to pół
  aplikacji i nie ma powodu tego ruszać);
- każde następne ląduje w `e.powroty = [{nr, we}, …]`;
- `przejsciaElementu(e)` zwraca wszystkie po kolei i jest jedynym wejściem do tej
  listy; `porzadkujPrzejscia(e)` pilnuje, żeby `e.nr` był zawsze **najmniejszym**
  numerem, bo inaczej „e.nr" przestałoby znaczyć „kiedy koń tu jest pierwszy raz".

Za tym poszły wszystkie miejsca liczące numery: `nastepnyWolnyNumer`,
`zwolnijNumer`, `zrobMiejsceOd`, `wyjmijZKolejnosci` (kasuje wszystkie przejścia,
od największego numeru, żeby przesuwanie nie uciekało), `zdejmijNumer` (zdejmuje
tylko OSTATNIE przejście), `reczneNumery`, `saReczneNumery`, `wyczyscNumery`,
`kropkiWTrasie` i plakietka („1·2·3·4").

Stary zapis z `nr2` leczy się przy wczytaniu i od razu zapisuje.

### Gest był zablokowany o jedną linijkę wcześniej, niż myślałem

Szukałem w `polaczKropki`, a blokada siedziała w **wyborze celu przy ciągnięciu**:
kropki tego samego elementu były po prostu pomijane jako kandydaci. Słusznie przy
drągu — przez drąg skacze się na wylot, w kółko się go nie objeżdża — więc warunek
został, ale tylko dla nie-omijanych. Przy pachołku i oponie wolno celować we własne
kropki, poza tą, z której ciągniemy, i poza tymi, którymi koń już jedzie.

### Geometria: chodzenie po okręgu kropek

Kolejne kroki tego samego pachołka to kolejne kropki na jego okręgu. Linia idzie
od jednej do drugiej, dokładając po drodze kropki pośrednie, plus jedna kropka
**przed** i jedna **za** (dojazd i odjazd po stycznej). Cztery kropki = pełne koło,
domknięte bez dojazdu.

Trzy rzeczy, które wyszły dopiero przy mierzeniu:

- **styczna musi być narzucona każdej kropce** — styczna okręgu, prostopadła do
  promienia. Bez tego pierwsza kropka brała styczną z dojazdu spoza pachołka
  i pełne koło wchodziło do środka: **1,20 m zamiast 1,75 m**. Przy okazji koń
  dojeżdża do koła stycznie, a nie pod kątem;
- **kropki naprzeciw siebie to remis** — obie drogi wokół pachołka są tak samo
  długie, więc kierunek musi wybrać jazda. Bez tego zawrót na końcu slalomu wracał
  tą stroną, z której koń przyjechał, i linia przechodziła **0,44 m od pachołka**,
  czyli po nim. Teraz objeżdża dalszą stroną: 1,75 m;
- **plakietka „1·2·3·4" jest szeroka** i połową swojej szerokości wchodziła
  z powrotem na koło. Odsuwa się teraz o połowę własnej szerokości.

Zmierzone po wszystkim: odchyłka od promienia kropek w cztery strony świata
**0,00 m**, i przy pełnym obejściu, i przy slalomie z powrotem.

Testy: `testy40` 264 (lewa kropka → górna działa; cztery kropki = cztery przejścia;
zajętej nie da się użyć drugi raz; koło trzyma promień; plakietka poza kołem),
265 (drąg dalej nie da się objechać w kółko), 266 (stary `nr2` leczy się i zapisuje).
Razem **27 zestawów, 851 sprawdzeń, 0 błędów.**

**Zasada:** kiedy gest „nie działa", szukaj najpierw tam, gdzie się WYBIERA cel,
a nie tam, gdzie się go OBSŁUGUJE. Blokada była trzy ekrany wyżej niż logika,
którą czytałem.

---

## Kasowanie kawałka trasy (17 września)

*„Nadal jest źle. Ale widzę następny problem — jak usunąć tylko kawałek trasy?"*

Na pytanie „co jest źle" nie odpowiedziała, ale przysłała film. Widać na nim
plątaninę: opona **1·5**, pachołek **2·4**, pachołek **3·6** — czyli kolejność
1(dół) → 2 → 3(góra) → 4 → 5(dół) → **6(góra)**, z ostatnim skokiem przez cały
plac na 13 m. Linia jest narysowana poprawnie; to **kolejność** jest zła.

I tu leży prawdziwy problem, ten sam, o który sama zapytała: **jeden zły ruch nie
dał się cofnąć osobno.** Zostawało cofnięcie albo „Wyjmij z trasy", czyli kasowanie
przeszkody razem ze wszystkimi jej przejazdami, albo „Rozwiąż trasę" — wszystko.
Przy trasie budowanej kilkunastoma ciągnięciami to znaczy: zaczynasz od zera.

### Zielona kropka znaczy „tędy koń jedzie" — więc jej dotknięcie to zdejmuje

Wybrała ten wariant z trzech. Ciągnięcie dokłada przejazd, dotknięcie go zdejmuje —
to samo miejsce, odwrotny gest, nie trzeba niczego szukać w menu.

- `przejscieNaKropce(e, i)` mówi, które przejście „należy" do tej kropki. Przy drągu
  kropki są dwie i obie opisują ten sam przejazd (wjazd i wyjazd na wylot), więc
  dotknięcie którejkolwiek zdejmuje go w całości;
- `zdejmijPrzejscie(e, nr)` kasuje **wskazany** przejazd, nie ostatni — reszta trasy
  zjeżdża o jeden;
- gdy zejdzie ostatni, przeszkoda wypada z trasy, ale **zostaje na placu**;
- biała kropka dalej tylko zaznacza i podpowiada, co z nią zrobić;
- kropka startu jest nietykalna — start nie jest przejazdem;
- wszystko idzie przez `zapiszHistorie()`, więc cofnięcie przywraca przejazd.

Podpowiedź po każdym połączeniu mówi teraz o obu gestach naraz: *„ciągnij dalej,
dotknij zielonej kropki, żeby zdjąć, albo poza placem, gdy gotowe"*. I liczy
przejazdy po polsku — „mijany drugi/trzeci/czwarty raz", a nie „drugi raz" przy
czwartym.

Testy: `testy40` 267 (plątanina z filmu rozbierana kropka po kropce; biała nic nie
kasuje; po zdjęciu wszystkich przejazdów pachołek wypada z trasy, ale zostaje na
placu; numery bez dziur), 268 (kropka startu nietykalna), 269 (cofnięcie przywraca).
Razem **27 zestawów, 867 sprawdzeń, 0 błędów.**

**Zasada:** każdy gest, który coś dokłada, musi mieć brata, który to samo zdejmuje —
w tym samym miejscu i bez szukania. Inaczej „cofnij" zostaje jedynym wyjściem,
a przy dłuższej pracy to za mało.

---

## Punkt na trasie (17 września, po filmach z konkurencji)

Luiza przysłała dwa filmy bez słowa komentarza: **parkour.design** i naszą wersję.
W tym pierwszym zatrzymała się na prawym panelu, w którym stoi:

> **Punkt na trasie** · Pozycja 24, 11 · Sekcja 1 · **Moc wygięcia łuku** [1] · **Usuń**

To jest rzecz, której u nas w ogóle nie było. Nasza linia brała się **wyłącznie
z kolejności przeszkód** — więc kiedy wychodziła brzydko, nie było za co jej
złapać. U konkurencji między przeszkodami można dołożyć wolny punkt, przeciągnąć
go i tym wygiąć łuk.

(Na naszym filmie widać zresztą tę samą przyczynę co wczoraj: opona **1**,
opona **2·4**, opona **3·5** — kolejność chodzi w górę, w dół i znowu w górę,
więc linia musi robić hairpiny. Rysowane wiernie, tylko trasa jest taka.)

### Wolny punkt to krok przejazdu bez sprzętu

`stan.punkty = [{id, nr, x, y, moc}]`. Wchodzi do `krokiTrasy()` jak wszystko inne,
więc numerowanie, przesuwanie numerów i kasowanie działa bez osobnych ścieżek.

- **Dodanie:** ciągniesz od kropki i puszczasz **na pustym piasku**. Punkt wpina się
  zaraz za tym, od czego ciągnęłaś, reszta trasy zjeżdża o jeden.
- **Przesunięcie:** przeciągasz punkt — linia idzie za nim.
- **Zdjęcie:** dotykasz punktu (tak samo jak zielonej kropki), albo „Usuń" w pasku.
- **Wygięcie:** pole w pasku, 0,2–3. To mnożnik długości stycznej Béziera w tym
  punkcie: 1 to zwykła krzywa, więcej — pełniejszy łuk. Wierzchołek zostaje na
  punkcie, rozchodzą się boki, więc w testach mierzy się DŁUGOŚĆ łuku, nie wysokość.

Dwie rzeczy, które trzeba było dołożyć, żeby to nie przeszkadzało:

- **puszczenie tuż przy jakiejkolwiek kropce nie robi punktu** — to znaczy
  „celowałam w nią i chybiłam". Bez tego stary test „ta sama przeszkoda się nie
  łączy sama ze sobą" zaczął produkować punkt, bo druga kropka drąga jest
  niedozwolonym celem, więc `cel` wychodził pusty;
- **punkt nie rozcina liczenia odległości** — figura rozcina (koń jedzie łukiem),
  ale wygięcie tej samej prostej nie zmienia dystansu między przeszkodami. Na
  kartce dla jeźdźca punkt znika razem z resztą rączek (`class="uchwyt"`).

Przy okazji wyszła kolizja nazw: `ciagnietyPunkt` był już zajęty przez punkt
figury w `stan.tor`. Nowy nazywa się `ciagnietyPunktTrasy`.

Testy: `testy40` 270 (dodanie, numer zaraz za źródłem, wygięcie, przesunięcie,
zdjęcie), 271 (nie rozcina odległości, znika z eksportu), 272 („Rozwiąż trasę"
zabiera też punkty). Razem **27 zestawów, 890 sprawdzeń, 0 błędów.**

**Zasada:** jeśli coś jest wyliczane, musi dać się poprawić ręką. Automat, którego
nie da się nadpisać, prędzej czy później narysuje coś, czego człowiek nie chce —
i wtedy nie ma wyjścia poza „zrób to inaczej od nowa".

---

## Fule tylko przy przeszkodach (17 września)

*„Mam slalom zrobiony z opon, a pomimo tego wyświetlają mi się odległości i mam
informację, czy ilość fuli się zmieści. Ilość fuli ma być tylko przy przeszkodach."*

Wczoraj wyciąłem wyroki o fulach z **paska** odległości — i tam faktycznie było
czysto. Zostały w **oknie „szczegóły"**, gdzie cała preambuła dalej tłumaczyła,
że „odległość między przeszkodami to lądowanie + fule + odbicie", wypisywała fulę
konia i pokazywała legendę „Za blisko". Przy czterech oponach to jest wywód nie
na temat.

Teraz okno patrzy, czy na trasie JEST co skakać (`odleglosciTrasy.some(w => w.oceny.length)`):

- jest → wszystko po staremu;
- nie ma → jedno zdanie zamiast wykładu: *„Na tej trasie nie ma nic do skakania —
  pachołki i opony się mija, więc zostaje sama odległość, bez fuli"*, bez listy fuli,
  bez legendy, a nagłówek listy brzmi „Kolejność i odległości", nie „Przeszkody na trasie".

Przy okazji wiersz przestał nazywać oponę przeszkodą: teraz jest „opony 1 → 4",
a nie „przeszkoda 1 → 4". Do `MNOGA` doszły pachołki i opony.

**Zasada:** wyciąć liczbę to za mało — trzeba wyciąć też zdanie, które ją tłumaczy.
Wyjaśnienie bez rzeczy, którą wyjaśnia, jest gorsze niż sama rzecz.

Testy: `testy40` 273 (slalom z opon: ani wyroku, ani wykładu, ani legendy, a odległość
zostaje) i 274 (przy stacjonatach fule dalej są). Razem **27 zestawów, 902 sprawdzenia,
0 błędów.**

---

## Ćwiczenia i nożyczki do trasy (17 września, wieczorem)

*„Mam 3 opony, dodałam 2 przeszkody i chciałam połączyć trasą TYLKO te dwie
przeszkody, a automatycznie połączyło mnie z tymi oponami, a tego nie chciałam.
(…) Obok tego kółka ustawić też małe iks, żeby można było tę trasę usunąć
pomiędzy na przykład drągami."*

Zapytana wprost, wybrała: dwa **osobne ćwiczenia, każde liczone od 1**, i krzyżyk
**na środku każdego odcinka**. To jest zresztą to, co konkurencja nazywa „Sekcja 1",
„Sekcja 2" — i to, jak wygląda trening: najpierw rozgrzewka na slalomie, potem
zadanie na przeszkodach.

### Model: nie ruszamy kolejności, tylko stawiamy znacznik

Kusiło, żeby dać każdemu ćwiczeniu własną numerację w danych. Byłoby to przepisanie
wszystkiego, co liczy numery. Zamiast tego **globalna kolejność zostaje jedna**,
a krok może nieść znacznik `nowe` — „tu zaczyna się następne ćwiczenie":

- `krokiTrasy()` po posortowaniu nadaje każdemu krokowi `cw` (numer ćwiczenia)
  i `lok` (numer W RAMACH ćwiczenia) — i to `lok` trafia na rysunek;
- `sciezkaToru` przy `nowe` zaczyna nową podścieżkę (`M` zamiast `C`), więc jeden
  `<path>` rysuje obie linie i nie trzeba ruszać niczego, co po nim czyta;
- odległości nie przechodzą przez przerwę, a wiersze dostały `cw` i przy kilku
  ćwiczeniach mówią „ćw.1 · 1→3";
- przecięcie jest **odwracalne**: cofnięciem albo guzikiem „Połącz z poprzednim"
  przy pierwszej przeszkodzie następnego ćwiczenia.

Przejścia elementu niosą teraz `zrodlo` — obiekt, w którym naprawdę siedzą ich
pola. Dzięki temu znacznik czyta się i pisze bez liczenia indeksów, które
`porzadkujPrzejscia` i tak przestawia.

### Krzyżyki liczone z tej samej krzywej, którą widać

`sciezkaToru` rozpadło się na trzy kawałki: `stycznePunktow` (styczna na punkt),
`uchwytyOdcinka` (uchwyty Béziera jednego odcinka) i samo składanie ścieżki.
Dzięki temu `srodkiOdcinkow` liczy środek odcinka **tym samym wzorem**
(`(p1 + 3c1 + 3c2 + p2) / 8`), a nie „mniej więcej w połowie prostej" — krzyżyk
siada dokładnie na linii, także na łuku.

Punkty przejazdu dostały `krok` (do którego kroku należą), więc wiadomo, który
odcinek jest MIĘDZY krokami, a który wewnątrz jednego (najazd, okrążanie).
**Figury kopiujemy przy wkładaniu do przejazdu** — bez tego znacznik `krok`
lądował w `stan.tor` i jechał do zapisu.

Dwie pułapki po drodze:

- **przed pierwszym krokiem nie ma czego ciąć** — krzyżyk tam nie powstaje,
  a `krokiTrasy` ignoruje znacznik na pierwszym kroku;
- **krzyżyk zjadał dotknięcia kropek.** Długi odcinek mijający pachołek ma swój
  środek tuż obok niego, a cięcie sprawdzałem PRZED kropkami. Kropka jest
  ważniejsza (nią buduje się trasę), więc cięcie sprawdza się dopiero po niej.
  Złapał to test 267 — ten od zdejmowania przejazdów.

Testy: `testy40` 275 (cztery krzyżyki, cięcie robi 1-2-3 i 1-2, dwa kawałki linii,
odległości z „ćw.1"/„ćw.2", cofnięcie skleja), 276 („Połącz z poprzednim"),
277 (krzyżyki znikają z kartki i spod schowanych kropek). Razem **27 zestawów,
924 sprawdzenia, 0 błędów.**

**Zasada:** nowa warstwa nie musi znaczyć nowego modelu. Ćwiczenia to jeden
znacznik na kroku — i wszystko, co liczyło numery, liczy dalej po staremu.

---

## Krzyżyk kradł dotknięcia — i linia, którą wreszcie da się złapać (17 września)

*„Cały mechanizm tworzenia trasy nie działa — próbuję podłączyć cokolwiek do tej
trasy i się nie da. (…) Zrobiłeś usuń, ale nie zrobiłeś, że można tą trasę sobie
trochę przesuwać."*

### Prawdziwa przyczyna „nie da się nic podłączyć"

**To był mój błąd z poprzedniej wersji.** Krzyżyk do cięcia ciął już na
**naciśnięcie**, nie na puszczenie, a jego zasięg chwytu (0,6 m / 22 px) był
prawie taki sam jak kropki. Krzyżyk leży na środku odcinka, a odcinek biegnie
tuż obok sprzętu — więc w slalomie z oponami co 5 m krzyżyk wypada ok. 0,7 m od
kropki sąsiedniej opony.

Skutek: każde chybione o kilka pikseli sięgnięcie po kropkę **rozcinało trasę**
zamiast zacząć ciągnięcie. Z zewnątrz wyglądało to dokładnie tak, jak to opisała:
trasa zmienia się sama, połączenie nie powstaje, „się nie da".

Trzy poprawki naraz:

- **kolejność trafiania:** kropka → sprzęt → linia. Wcześniej linia łapała przed
  sprzętem;
- **zasięg krzyżyka zmniejszony** do 0,42 m / 15 px — mniej niż kropki;
- **cięcie dopiero na PUSZCZENIE i tylko bez ruchu.** Naciśnięcie, które zamieni
  się w ciągnięcie, nie tnie już niczego.

W testach nie było tego widać, bo testy klikają w **dokładne** współrzędne kropek.
Złapał to dopiero test 279, napisany tak, jak człowiek sięga ręką.

### Linia daje się złapać wprost

Punkt trasy z wersji 27 był dobry, ale dojście do niego wymagało ciągnięcia od
kropki na pusty piasek — czyli trzeba było o tym wiedzieć. U konkurencji po prostu
łapie się linię i ciągnie. Teraz u nas tak samo:

- **pociągnięcie linii** w dowolnym miejscu wygina ją tam — punkt powstaje sam,
  dokładnie pod palcem, i od razu jedzie za ręką;
- **puszczenie bez ruchu na krzyżyku** przecina trasę;
- **dotknięcie samej linii** nic nie robi i mówi o obu gestach.

`punktNaTrasie` szuka najbliższego punktu na **narysowanej krzywej** — tym samym
wzorem Béziera, którym ją rysujemy — i zwraca krok, PRZED którym trzeba wstawić
punkt. Dzięki temu wygięcie ląduje w tym odcinku, za który się pociągnęło, a nie
„gdzieś w kolejce".

Testy: `testy40` 278 (pociągnięcie linii wygina ją i stawia punkt pod palcem;
dotknięcie linii nic nie psuje), 279 (ciągnięcie z kropki do nowej przeszkody
dopina ją i NIC nie rozcina; dotknięcie opony obok odcinka tylko ją zaznacza).
Razem **27 zestawów, 936 sprawdzeń, 0 błędów.**

**Zasada:** gest niszczący nie ma prawa działać na naciśnięcie. Naciśnięcie to
dopiero zamiar — dopiero puszczenie bez ruchu jest decyzją. I: jeśli test klika
w idealny punkt, to nie sprawdza tego, co robi ręka.
