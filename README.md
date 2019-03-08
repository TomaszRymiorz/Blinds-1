# Blinds nx
Oprogramowanie rolety automatycznego domu.

### Budowa rolety
Mechanizm rolety zbudowany jest w oparciu o ESP8266 wraz z modułem RTC DS1307. Całości dopełniają silniki krokowe 28BYJ-48 ze sterownikiem oraz fotorezystor pełniący funkcję czujnika zmierzchowego.

### Możliwości
Łączność z roletą odbywa się przez sieć Wi-Fi.
Dane dostępowe do routera przechowywane są wraz z innymi informacjami w pamięci flash.
W przypadku braku informacji o sieci, urządzenie aktywuje wyszukiwania routera z wykorzystaniem funkcji WPS.

Roleta automatycznie łączy się z zaprogramowaną siecią Wi-Fi w przypadku utraty połączenia.

Zawiera czujnik światła wykorzystywany przez funkcje automatycznych ustawień. Dane z czujnika wysyłane są również do bliźniaczego urządzenia w pomieszczeniu jakim jest włącznik światła. Parowanie urządzeń odbywa się poprzez aplikację dedykowaną.

Roleta posiada opcję wykonania pomiaru wysokości okna, zmianę kierunku obrotów silnika oraz możliwość kalibracji rolety. Funkcja mierzenia okna wyklucza stosowanie niewygodnych ograniczników krańcowych, zapobiega pomyłkom błędnie podanych ręcznie wartości i ewentualnych różnic w silnikach w przypadku wartości zdefiniowanych programowo.

Zegar czasu rzeczywistego wykorzystywany jest przez funkcję ustawień automatycznych.
Ustawienia automatyczne obejmują opuszczanie i podnoszenie rolety o wybranej godzinie oraz opuszczanie po zapadnięciu zmroku i podnoszenie o świcie. Powtarzalność obejmuje okres jednego tygodnia, a ustawienia nie są ograniczone ilościowo. W celu zminimalizowania objętości wykorzystany został zapis tożsamy ze zmienną boolean, czyli dopiero wystąpienie znaku wskazuje na włączoną funkcję.

* 'w' cały tydzień - występuje tylko w zapisie aplikacji w celu zminimalizowania ilości przesyłanych danych
* 'o' poniedziałek, 'u' wtorek, 'e' środa, 'h' czwartek, 'r' piątek, 'a' sobota, 's' niedziela
* 'n' opuść po zmroku
* 'd' podnieś o świcie
* '_' opuść o godzinie - jeśli znak występuje w zapisie, przed nim znajduje się godzina w zapisie czasu uniksowego
* '-' podnieś o godzinie - jeśli występuje w zapisie, po nim znajduje się godzina w zapisie czasu uniksowego
* '/' wyłącz ustawienie - obecność znaku wskazuje, że ustawienie będzie ignorowane

Przykład zapisu trzech różnych ustawień automatycznych: 1140_bw-420,basn,/bouehrn-300

Ustawienia automatyczne obejmują również włącznik światła, a obecność znaku 'b' wskazuje, że ustawienie dotyczy rolety.

### Sterowanie
Sterowanie roletą odbywa się poprzez wykorzystanie metod dostępnych w protokole HTTP. Sterować można z przeglądarki lub dedykowanej aplikacji.

* "/hello" - Handshake wykorzystywany przez dedykowaną aplikację, służy do potwierdzenia tożsamości oraz przesłaniu parametrów pracy rolety.

* "/set" - Pod ten adres przesyłane są ustawienia dla rolety, dane przesyłane w formacie JSON. Ustawić można strefę czasową ("offset"), czas RTC ("time"), IP bliźniaczego urządzenia ("twin"), ustawienia automatyczne ("smart"), pozycję rolety na oknie ("val"), dokonać kalibracji pozycji, jak również zmienić ilość kroków czy wartość granicy dnia.

* "/state" - Służy do regularnego odpytywania rolety o jej podstawowe stany, położenie rolety i wskazania czujnika oświetlenia, a także w przypadku wykonywania pomiaru okna ("measure") - ilość kroków silnika.

* "/reversed" - Odpytanie tego adresu zmienia kierunek obracania się silnika rolety.

* "/reset" - Ustawia wartość pozycji rolety na 0.

* "/measurement" - Służy do wykonania pomiaru wysokości okna.

* "/deletewifisettings" - Ten adres służy do usunięcia danych dostępowych do routera.

* "/log" - Pod tym adresem znajduje się dziennik aktywności urządzenia.
