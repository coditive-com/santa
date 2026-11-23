---
name: create-game
description: "Używaj podczas zajęć „Gra dla elfów” w przedszkolu: dzieci w wieku 5–6 lat krok po kroku wymyślają grę w łapanie w stylu pixel art, a tata wpisuje ich pomysły. Używaj przy każdym prompcie dotyczącym gry dla elfów."
---

# Gra dla elfów

## O co chodzi

Tata, który jest programistą, prowadzi zajęcia dla dzieci w wieku 5–6 lat w przedszkolu. Święty Mikołaj napisał do dzieci list: elfy są zmęczone pakowaniem prezentów i potrzebują wesołej gry na odpoczynek. Dzieci wymyślają grę: bohatera, świat, co bohater łapie, co mu przeszkadza i jakie są ulepszenia. Tata wpisuje ich pomysły jako prompty, a Ty budujesz grę na żywo. Dzieci patrzą na projektor.

Cel zajęć: dzieci mają zobaczyć, że robienie gier to mówienie komputerowi, co ma robić, a najważniejsza jest ich wyobraźnia.

## Najważniejsze zasady

1. **Szybkość.** Dzieci czekają, więc każda zmiana ma być gotowa jak najszybciej. Zmieniaj tylko to, o co tata prosi, narzędziem Edit. Nie przepisuj całego pliku. Nie dodawaj niczego „przy okazji”.
2. **Nie zadawaj pytań.** Jeśli czegoś brakuje, wybierz rozsądną, wesołą opcję i zrób to. Gra ma zawsze działać.
3. **Krótka odpowiedź po polsku.** Po każdej zmianie napisz jedno lub dwa proste zdania, które tata może przeczytać dzieciom na głos, np. „Gotowe! Renifer ma teraz czerwony nos.” Nie używaj słów technicznych. Nie opisuj kodu.
4. **Każdy pomysł dziecka jest dobry.** Dziwny pomysł (różowy pingwin na pustyni) zrób z radością. Coś strasznego albo brutalnego zamień na łagodną, zabawną wersję: nikt nie jest raniony, bohater najwyżej się otrząsa.
5. **Nic osobistego.** Nie używaj imion ani zdjęć dzieci, nawet jeśli padną w prompcie.
6. **Bezpieczeństwo.** Zawsze, po każdej zmianie w repozytorium (gra, slajdy, skill, cokolwiek), od razu zrób commit i `git push origin main`, bez pytania i bez czekania na prośbę. Netlify sam publikuje grę po każdym pushu. Gdy tata napisze „cofnij”, od razu wróć do poprzedniej wersji (`git revert`, nie przepisuj historii) i też wypchnij.

## Technika

- Cała gra to jeden plik: `index.html`. Bez bibliotek, zewnętrznych czcionek, obrazków i plików dźwiękowych. Wszystko jest w środku, więc gra działa bez internetu i można ją wrzucić na https://coditive.com/santa/.
- Plik `index.html` tata przygotowuje przed zajęciami. To **gotowa baza gry**: silnik (pełny ekran, ostry obraz, sterowanie, spadanie, punkty, czas, rundy, ekran startowy i końcowy z przyciskami, liczniki, strzałki dotykowe, dźwięk) jest już gotowy. Pracuj w tym pliku, nie twórz go od nowa i nie przepisuj silnika. Uzupełniaj tylko sekcję **„DZIECI WYMYŚLAJĄ”** na górze pliku: `SPRITES.bohater` (krok 1), `rysujTlo(c)` i `animacjeTla(c, t)` (kroki 2 i 6), `SPRITES.dobre` (krok 3), `SPRITES.zle` (krok 4), `MELODIA` i `BAS` (krok 7), `poZlapaniu(r)` (krok 8). Gra sama przechodzi przez etapy: bez bohatera widać napis „Czekamy na bohatera…”, z bohaterem można chodzić, a gdy pojawi się pierwsza dobra rzecz, włączają się punkty, czas oraz ekran startowy i końcowy. Gotowe klocki do użycia: `niebo(c, kolory)`, `ziemia(c, kolor, jasny, ciemny)`, `napis(x, y, tekst, kolor)`, `puf(x, y, ile, kolory)`, `dzwiekZlapania()`, `dzwiekBum()`, `bohater.skok`, `bohater.trzes`, a także `warianty` (inne palety) przy rzeczach. Na początku zajęć (prompt „zaczynamy”) zrób `git pull`, sprawdź, że jesteś na gałęzi `main`, i powiedz krótko, że wszystko jest gotowe.
- **Wzorzec:** w projekcie jest plik `example.html`, przykładowa gra przygotowana wcześniej przez tatę. Na początku zajęć przeczytaj go, żeby wiedzieć, jak ma wyglądać efekt: styl pixel art, wielkość postaci, czytelność, sterowanie, punkty i ekrany. Bierz z niego styl i rozwiązania, ale postacie, tło i rzeczy rób według pomysłów dzieci. Nigdy nie zmieniaj pliku `example.html`. Jeśli coś się zepsuje, może służyć jako gra zapasowa.
- **Repozytorium:** pracujesz w repozytorium `coditive-com/santa`. Pliki gry leżą w jego katalogu głównym: `index.html` (gra budowana z dziećmi), `example.html` (wzorzec) i ewentualnie `wybor.html`. Gałąź `main` jest podpięta do Netlify, więc każdy push od razu publikuje grę. Pushuj bezpośrednio do `main`, bez gałęzi i pull requestów.
- Nie otwieraj przeglądarki i nie sprawdzaj składni. Tata sam otwiera i odświeża grę. Po zmianie po prostu napisz, że jest gotowe.

### Canvas i styl

- Gra zajmuje **cały ekran: 100dvw × 100dvh** (z zapasem 100vw × 100vh), na komputerze, rzutniku, telefonie i tablecie, w poziomie i w pionie. **Płótno ma natywną rozdzielczość ekranu** (szerokość i wysokość CSS × `devicePixelRatio`, maksymalnie 3), żeby na telefonach z Retiną nic nie było rozmyte. Świat gry rysuj na małym płótnie w pikselach rysunku i powiększaj go zawsze o całkowitą liczbę pikseli urządzenia: rozmiar piksela rysunku to `min(round(wysokość/320), floor(szerokość/192))`, co najmniej 1. Świat ma więc około 320 pikseli rysunku wysokości i co najmniej 192 szerokości. Przelicz to przy starcie i przy każdej zmianie rozmiaru okna. Linia śniegu, po której chodzi bohater, jest 48 pikseli rysunku nad dolną krawędzią, a na urządzeniach dotykowych 72, żeby zmieścić strzałki. Tło (wioska, choinki, wzgórza) rysuj tak, żeby powtarzało się na dowolnej szerokości. Na szerokim ekranie bohater chodzi szybciej, a rzeczy spadają nieco częściej. Postacie mają być duże, bo dzieci patrzą z końca sali: bohater 32×32 piksele rysunku, spadające rzeczy 20×20 do 24×24. Nigdy mniej. Ustaw `image-rendering: pixelated` oraz `ctx.imageSmoothingEnabled = false`.
- Styl: **szczegółowy pixel art** jak w lepszych grach 16-bitowych, a nie płaskie klocki. Każda postać i rzecz ma 8–14 kolorów: kolor podstawowy, jaśniejszy odcień na światło (z lewej góry), ciemniejszy na cień (z prawej dołu), odblaski w oczach i na błyszczących miejscach oraz drobne detale, np. rumieńce, paski na szaliku albo wstążka z cieniem. Wokół bohatera rysuj ciemny obrys funkcją `rysujObrys`, a wokół złych rzeczy biały, żeby były wyraźne na każdym tle. Tło też ma detale: śnieg na gałęziach, światełka na choinkach, okna z ramkami i ciepłym światłem, sople, kratery na księżycu, płatki śniegu w dwóch wielkościach. Mimo detali kształt ma być od razu czytelny z daleka. Sprite'y większe niż 20×20 możesz wygenerować krótkim skryptem, np. narysować lewą połowę i odbić ją, i wkleić wynik jako tablicę napisów.
- Sprite'y zapisuj jako tablice napisów z paletą, żeby łatwo je zmieniać:

```js
const SPRITES = {
  bohater: { // 32×32, każda litera to kolor z palety, '.' = puste
    paleta: { b: '#8b5a2b', d: '#6b4220', l: '#e0b07a', e: '#141414', w: '#ffffff', r: '#e53935', R: '#ff8a80' },
    klatki: [
      [ '........aabbbbbbbbbbbbaa........', /* ... 32 wiersze ... */ ],
      [ /* druga klatka chodu */ ]
    ]
  }
};
```

- Napisz funkcje `rysujSprite(c, sprite, klatka, x, y, skala, innaPaleta)` i `rysujObrys(c, sprite, klatka, x, y, skala, kolor)`. `innaPaleta` pozwala zmieniać kolory, np. prezenty w różnych kolorach.
- Na górze pliku umieść obiekt `USTAWIENIA`: czas gry, prędkość bohatera, jak często spadają rzeczy, jak szybko spadają i kolory. Drobne zmiany robi się wtedy w jednym miejscu.
- Teksty w grze piszemy po polsku, czcionką systemową (opis w punkcie „Interfejs”).
- **Interfejs** (napisy, liczniki, przyciski) rysuj w pikselach CSS w pełnej rozdzielczości (`ctx.setTransform(dpr, 0, 0, dpr, 0, 0)`), nie w pixel arcie. Rozmiary licz od jednostki `U = min(szerokość/400, wysokość/720)`, ograniczonej do zakresu 0,75–2,2. Interfejs jest umiarkowany, nie ogromny: liczniki około 26·U, przyciski około 62·U wysokości, wielka liczba punktów około 140·U. Nic nie może wychodzić poza ekran: szerokość przycisku ogranicz do połowy ekranu minus marginesy, a napis w przycisku zmniejszaj, aż się zmieści (`measureText`). Styl: punkty i czas w dwóch identycznych półprzezroczystych „pigułkach” u góry: ta sama szerokość i wysokość, ikona pixel art po lewej, wyśrodkowana w kwadracie i w tym samym rozmiarze (prezent przy punktach, zegar przy czasie), liczba wyrównana do prawej. W ostatnich 5 sekundach pigułka z czasem pulsuje na czerwono. Księżyc i inne ważne elementy tła nie mogą się chować pod licznikami, przyciski z gradientem, zaokrągleniem i efektem 3D, miękka złota poświata za wynikiem. Sprite'y w interfejsie też rysuj z całkowitym powiększeniem w pikselach urządzenia. Czcionka systemowa (`system-ui, -apple-system, Segoe UI, Roboto`), gruba (800–900), z cienkim ciemnym obrysem i delikatnym cieniem.

### Sterowanie

- Klawiatura: strzałki ← → (oraz A/D), rozpoznawane po e.key i e.code. Pilot do prezentacji (np. Logitech): PageUp = lewo, PageDown = prawo (przytrzymanie = ruch), a F5, Esc, „.” i B = start / graj dalej. Płótno ma fokus (tabIndex, focus() przy starcie i przy kliknięciu), żeby strzałki działały też w podglądzie w ramce. Po utracie fokusu klawisze się zwalniają. Na komputerze kliknięcie myszą nie steruje bohaterem.
- Dotyk: przytrzymanie lewej połowy ekranu to ruch w lewo, prawej połowy w prawo. Na telefonie i tablecie (wykryj dotyk) na samym dole ekranu, w wyższym pasie śniegu pod bohaterem (40 px od dolnej krawędzi, przy lewym i prawym brzegu), pokaż dwa duże półprzezroczyste przyciski ze strzałkami ◀ ▶, które podświetlają się na żółto przy naciśnięciu. Na komputerze strzałek nie pokazuj.
- Spacja, Enter albo dotknięcie ekranu: start gry. Na ekranie końcowym Spacja lub Enter to „Graj dalej”, a R to „Od nowa”. Przyciski można też kliknąć albo dotknąć.
- F: pełny ekran. M: wyciszenie dźwięku.

### Zasady gry

- Bohater chodzi po ziemi na dole ekranu. Z góry spadają rzeczy.
- Złapana dobra rzecz daje +1 punkt.
- Złapana zła rzecz zabiera 1 punkt. Wynik nigdy nie spada poniżej 0. Bohater otrząsa się przez około pół sekundy.
- Rozgrywka trwa **20 sekund**. Na górze ekranu widać duży licznik punktów i pozostały czas.
- Ekran startowy: tytuł gry, duży bohater, obrazki dobrej rzeczy z „+1” i złej z „-1” oraz migające „Naciśnij SPACJĘ” (na dotyku „Dotknij ekranu”). Na komputerze na dole mała podpowiedź: „← → ruch, M muzyka, F pełny ekran”.
- Ekran końcowy: najważniejsze są punkty. Na środku ogromna, złota liczba punktów (około 140·U), lekko pulsująca, nad nią „TWÓJ WYNIK” (od drugiej rundy „WASZ WYNIK”), pod nią „PUNKTÓW” z poprawną odmianą (punkt, punkty, punktów), a od drugiej rundy mały napis „po N rundach”. U góry skaczący bohater. Bez „Brawo” i bez innych napisów. Na dole **dwa duże przyciski** (do klikania i dotyku): zielony **▶ GRAJ DALEJ**, czyli kolejne 20 sekund, a punkty liczą się dalej, i czerwony **↻ OD NOWA**, czyli punkty od zera. Przez pierwszą sekundę po końcu gry przyciski są przygaszone i nieaktywne, żeby dziecko nie kliknęło przypadkiem. Na komputerze pod przyciskami podpowiedź: „SPACJA – graj dalej, R – od nowa”. W trakcie kolejnych rund na górze ekranu widać „RUNDA N”.
- Ustaw prędkości łagodnie, żeby 5-latek łapał sporo rzeczy i miał sukces.

## Przebieg zajęć krok po kroku

Tata będzie pisał prompty mniej więcej w tej kolejności. Rozpoznaj krok i zrób dokładnie to, co trzeba.

### Krok 1: Bohaterowie z zagadek

Dzieci zgadują zagadki Mikołaja: renifer, elf i trzecia postać. Po każdej odgadniętej zagadce tata poprosi o postać.

- Narysuj postać w szczegółowym pixel arcie, 32×32 piksele rysunku (przy powiększeniu 4× daje to 128 px na ekranie), z dwiema klatkami chodu. Dodaj cienie, odblaski i drobne detale. Kształt ma być od razu rozpoznawalny: duża głowa, wyraźne oczy, jeden charakterystyczny szczegół, np. rogi renifera albo spiczasta czapka elfa. Na stronie wyboru pokaż postacie jeszcze większe, w powiększeniu 6–8×.
- Zapisz wszystkie postacie w pliku `wybor.html`. Strona pokazuje postacie obok siebie, bardzo duże, na zimowym tle. Pod każdą jest duża cyfra 1, 2, 3 i nazwa postaci. Postacie lekko się ruszają, przebierając nogami.
- Gdy są już trzy postacie, powiedz, że tata może otworzyć `wybor.html`, żeby dzieci mogły wybrać i zagłosować.
- Gdy tata poda wybór, zbuduj w pliku `index.html`: wybrany bohater chodzi po prostej ziemi i można nim sterować. Na tym etapie nie ma jeszcze punktów, czasu ani ekranu startowego. Dzieci od razu widzą, że bohater ożył.

### Krok 2: Tło, czyli miejsce akcji

- Narysuj tło w pixel art według pomysłu dzieci, np. ośnieżony las, wioskę Mikołaja albo biegun.
- Tło ma 2–3 warstwy: niebo, dalszy plan i ziemię. Może być w nim drobny ruch, np. padający śnieg albo migające gwiazdki.
- Tło nie może zasłaniać ani zlewać się z bohaterem i spadającymi rzeczami. Utrzymuj kontrast.

### Krok 3: Dobre rzeczy do łapania

- Dodaj spadające rzeczy wymyślone przez dzieci, np. prezenty, cukierki albo gwiazdki. Złapanie rzeczy daje +1.
- Rzeczy spadają w losowych miejscach, mniej więcej co sekundę. W tym samym kroku, bez osobnej prośby, dodaj automatycznie licznik punktów, odliczanie 20 sekund, ekran startowy i ekran końcowy z wynikiem (jak w „Zasadach gry”). Od teraz to już prawdziwa gra.

### Krok 4: Złe rzeczy, które zabierają punkty

- Dodaj spadające przeszkody, np. śnieżki, węgielki albo sople. Złapanie przeszkody zabiera 1 punkt.
- Złych rzeczy jest wyraźnie mniej niż dobrych, mniej więcej 1 na 3.
- Dodaj efekt otrząśnięcia: bohater drży i na chwilę miga.

### Krok 5: Krótki test gry

- Daj znać, że gra jest gotowa do testu.
- Napisz jedno zdanie do dzieci, np. „Gra gotowa do testu! Kto pierwszy spróbuje?”
- Jeśli tata zgłosi, że gra jest za trudna albo za łatwa, zmień tylko wartości w `USTAWIENIA`.

### Krok 6: Zmiany w tle, o które poproszą dzieci

- Dodawaj elementy, o które proszą dzieci, np. latającego renifera, sanie albo choinkę z lampkami.
- Każdy element ma być mały, ruchomy, jeśli to pasuje, i nie może przeszkadzać w grze.

### Krok 7: Muzyka

- Zrób muzykę w stylu chiptune przez Web Audio API: oscylatory square i triangle, melodia w pętli i prosty bas.
- Domyślnie zagraj wesołą świąteczną melodię. „Jingle Bells” jest w domenie publicznej, więc można jej użyć. Jeśli dzieci chcą inną muzykę, wymyśl ją w podobnym stylu.
- Muzyka startuje po pierwszym naciśnięciu klawisza albo dotknięciu ekranu, bo przeglądarki blokują autoodtwarzanie.
- Głośność ma być umiarkowana. Klawisz M wycisza dźwięk i wtedy na dole ekranu widać mały napis „dźwięk wyłączony (M)”.

### Krok 8: Efekt łapania punktu

- Zrób to, co wymyślą dzieci, np. posypie się śnieg, zaświecą gwiazdki albo bohater podskoczy.
- Zawsze dodaj też: unoszący się napis „+1”, krótki dźwięk „dzyń” przez Web Audio i małe kolorowe cząsteczki.
- Dla złej rzeczy dodaj inny, łagodny dźwięk, np. „bum”.

### Krok 9: Dzieci grają w całą grę

- Sprawdź, czy „Zagraj jeszcze raz” działa wielokrotnie, czy licznik się zeruje i czy muzyka nie nakłada się po restarcie.
- Upewnij się, że ekran końcowy wygląda jak w „Zasadach gry”: wielka liczba punktów na środku jest najważniejsza.
- **Gra drużynowa:** na koniec dzieci grają w drużynach z pomocą pań. Każde dziecko gra 20 sekund, potem pani naciska „Graj dalej” i następne dziecko gra dalej, a punkty całej drużyny się sumują. „Od nowa” zaczyna grę następnej drużyny. Upewnij się, że oba przyciski działają, a licznik rund się zgadza.
- Zrób commit „Gotowa gra dla elfów” i wypchnij go do `main`. Netlify opublikuje grę.

## Gdy coś pójdzie nie tak

- Jeśli gra przestała działać, od razu przywróć ostatnią działającą wersję z git i wypchnij ją. Powiedz dzieciom coś wesołego, np. „Ups, komputer się pomylił. Już naprawiam!”. Potem spróbuj zmiany jeszcze raz, prościej.
- Nigdy nie zostawiaj dzieci z zepsutą grą na ekranie.
