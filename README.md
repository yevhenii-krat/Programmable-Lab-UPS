<h1 align="center">
  <strong><em>Programmable UPS for Automation</em></strong>
</h1>

### Informacja wstępna
Jest to pierwsza wersja mego projektu "Programowalny UPS dla Automatyki/laboratorium".
Najgłówniejszą cechą projektu jest przetwarzanie napięcia z akumulatora na napięcie przemienne w zakresie od 0 *VAC* do 24 *VAC* lub napięcie stałe w zakresie od 0 *VDC* do 48 *VDC*.

### Konstrukcja
Konstrukcja będzie składała się z trzech płytek PCB połączoną dystansami nylonowymi w tak zwaną "kanapkę". Połączenia niskoprądowe/sygnałowe między warstwami kanapki będą wykonane Headerami, a połączenia wysokoprądowe/mocy będą wykonane przewodami podłączonymi do złącz śrubowych. Kanapka wraz z akumulatorem będzie umieszczona w obudowie Kradex ZP240.190.105SJp IP67 TM ABS - 240x190x105mm z mosiężnymi tulejami.

### Elementy sterujące i złącza
Wejście oraz wyjście urządzenia są przedstawione złączami bananowymi po obu stronach urządzenia. W obudowę będzie zamontowany ekran LCD, przełącznik bistabilny dla sprzętowego i cyfrowego wyłączenia urządzenia (za pomocą przekaźników), przełącznik bistabilny dla sprzętowego i cyfrowego przełączania trybu wyjścia z DC na AC i odwrotnie, oraz cztery przyciski monostabilne do regulacji napięcia i nawigacji w menu.

### Topologia urządzenia
<img width="1800" height="1245" alt="UPSForAutomationTopology" src="https://github.com/user-attachments/assets/31d2e207-0a03-4860-8d56-fd7963ffd1e5" />


### Jednostka sterująca
Całym wnętrzem projektu będzie sterował mikrokontroler STM32G4. Dla tego procesora będzie zaprojektowana oddzielna (środkowa) płytka w "kanapce". Ten "goły" procesor będzie posiadał wyprowadzenia dla wszystkich pinów i portów, wraz z SWD do programowania tego mikrokontrolera przez programatory, szczególnie przez programatory wbudowane do płytek NUCLEO STM32, w moim przypadku to L476RGT6.
Mikrokontroler będzie zasilany z baterii żeby przy zaniku prądu nie miał szansy się zresetować. Bateria będzie cały czas doładowywana, a przy długim zaniku prądu i krytycznie rozładowanej baterii mikrokontroler odłączy się wraz z całym urządzeniem od baterii żeby przeszkadzać "Wampiryzmu elektronicznemu" i nie zniszczyć baterii. Mikrokontroler będzie inteligentnie sterował i nadzorował wszystkie stany lub procesy odbywające się wewnątrz urządzenia. STM32G4 będzie precyzyjnie obliczał stan baterii przez metodę Coulomb Counting oraz sterował regulacją PID+PR dla DC/DC oraz Full bridge monitorując sygnały ze sprzężenia zwrotnego, a także będzie zarządzał wyświetlaczem LCD dla łatwej nawigacji użytkownika.

### Bateria
Bateria będzie wykonana z ogniw Samsung INR21700-53G 5300mAh 15A. Producent wskazuje maksymalny prąd rozładowania 15A, ale niekture testy pokazują maksymalny ciągły prąd rozładowania na poziomie 9A, dla tego przy łączeniu ognił w baterię będę używał topologii 5S2P, czyli 5 szeregowo i 2 równolegle połączone ogniwa. Ładowarka będzie dynamicznie obniżać lub zwiększać napięcie i prąd ładowania w zależności od dostarczonej energii elektrycznej z sieci, ustawień mikrokontrolera oraz stopnia naładowania baterii z użyciem protokołu CC/CV (Constant Current/Constant Voltage), żeby bezpiecznie ładować baterię przeszkadzając jej gławtownej degradacji. Na płytce będzie się znajdował własnoręcznie zaprojektowany BMS na układzie BQ76920 od firmy Texas Instruments, który ma zaimplementowany ADC, algorytm Coulomb Counter i balansowanie, przez co nie potrzebuję budowania osobnych torów pomiarowych dla każdej celi, a tylko nawiązać komunikację z mikrokontrolerem przez I2C. Bateria będzie załączana przy zaniku napięcia z sieci praktycznie natychmiast bo będzie współdzielić i stabilizować szynę DC z głównym torem prądowym (zasilaniem głównym).

#### Regulacja napięcia
DC/DC Buck Boost będzie regulować napięcie maksymalnie do 48 VDC, zatem mikrokontroler steruje MOSFETAMI Full Bridge żeby wygenerować czysty sinus 50HZ o napięciu regulowanym w zakresie od 0 VAC do 24 VAC.

### Dodatkowo
Projekt będzie zawierał filtry sprzętowe i cyfrowe a także będzie posiadał zabezpieczenia nadprądowe.
Na PCB będą rozmieszczone punkty testowe (test points) do ułatwienia serwisowania.
Projekt będzie robiony w Altium Designer. Dla każdej płytki będzie osobny projekt które połączę w jeden dla wydruku PCB używając narzędzia Embedded Board Array oraz połączę w kanapkę dla precyzyjności rozstawienia pinów używając narzędzia Multi-board PCB.
