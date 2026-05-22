# Roadmapa aplikacji Zwierzątko

## Etap 1: MVP

Celem pierwszej wersji jest dostarczenie podstawowej elektronicznej książeczki zdrowia dla jednego zwierzęcia, z obsługą właściciela, weterynarza i administratora.

### Zakres MVP

#### 1. Rejestracja i logowanie

- Rejestracja użytkownika.
- Wybór roli: właściciel zwierzęcia lub weterynarz.
- Brak możliwości samodzielnej rejestracji jako administrator.
- Logowanie i wylogowanie.
- Usunięcie konta oraz danych użytkownika z systemu.

#### 2. Role i uprawnienia

- Właściciel może zarządzać swoim zwierzęciem.
- Weterynarz widzi tylko zwierzęta, które zostały mu udostępnione.
- Administrator widzi listę użytkowników.
- Administrator zatwierdza i cofa zatwierdzenie kont weterynarzy.
- Zatwierdzenie weterynarza może mieć datę obowiązywania od-do.

#### 3. Profil zwierzęcia

- Dodanie jednego zwierzęcia.
- Obsługa psa i kota.
- Imię zwierzęcia.
- Data urodzenia.
- Możliwość oznaczenia daty urodzenia jako orientacyjnej.
- Rasa jako pole tekstowe z podpowiedziami z predefiniowanej listy.
- Waga.
- Zdjęcie.
- Dodatkowe informacje opcjonalne.
- Podgląd danych zwierzęcia.
- Edycja danych zwierzęcia.

#### 4. Historia zdrowia

- Dodawanie wpisów do historii zwierzęcia.
- Możliwość wpisania leków, dawek i zaleceń.
- Edycja wpisów dodanych przez właściciela.
- Przechowywanie historii leczenia w jednym miejscu.

#### 5. Współpraca z weterynarzem

- Właściciel może udostępnić zwierzę wybranemu weterynarzowi.
- Właściciel może wycofać udostępnienie.
- Weterynarz może wyszukać tylko zwierzęta widoczne dla niego.
- Weterynarz może dodawać zalecenia.
- Weterynarz może dodawać informacje o szczepieniach i leczeniu.

### Efekt po MVP

Użytkownik ma działającą elektroniczną książeczkę zdrowia dla jednego psa lub kota. Może prowadzić historię zdrowia, aktualizować dane zwierzęcia i udostępniać je zatwierdzonemu weterynarzowi.

## Etap 2: Rozszerzenie opieki nad zwierzęciem

Celem drugiej wersji jest rozwinięcie aplikacji o bardziej kompletne zarządzanie historią życia zwierzęcia i lepszą obsługę przypadków adopcji, śmierci lub zmiany właściciela.

### Zakres

- Oznaczenie zwierzęcia jako pożegnanego.
- Dodanie daty pożegnania.
- Osobna, mniej eksponowana sekcja dla pożegnanych zwierząt.
- Możliwość wyłączenia historii pożegnanych zwierząt.
- Transfer danych zwierzęcia do innego użytkownika.
- Proces przekazania zwierzęcia przy adopcji, sprzedaży lub zmianie opiekuna.
- Zaplanowanie bezpiecznego mechanizmu transferu, np. przez kod QR lub czasowy kod dostępu.

## Etap 3: Wiele zwierząt i model premium

Celem trzeciej wersji jest odejście od ograniczenia jednego zwierzęcia i przygotowanie aplikacji pod większą skalę użytkowania.

### Zakres

- Obsługa wielu zwierząt na jednym koncie.
- Lista aktywnych zwierząt.
- Filtrowanie i wyszukiwanie zwierząt.
- Limity darmowego planu.
- Płatny plan dla właścicieli z większą liczbą zwierząt.
- Limity dla weterynarzy na liczbę podopiecznych.
- Płatny plan dla weterynarzy obsługujących większą liczbę zwierząt.

## Etap 4: Schroniska, hodowle i opiekunowie

Celem czwartej wersji jest wsparcie organizacji, które opiekują się większą liczbą zwierząt i często przekazują je nowym właścicielom.

### Zakres

- Specjalna rola opiekuna lub organizacji.
- Darmowy wariant dla schronisk.
- Zarządzanie większą liczbą zwierząt.
- Uproszczony transfer zwierzęcia do nowego właściciela.
- Historia opieki przed adopcją.
- Możliwość prowadzenia danych zdrowotnych przez schronisko lub hodowlę.

## Etap 5: Komunikacja i rezerwacje

Celem piątej wersji jest dodanie funkcji, które wspierają codzienny kontakt właściciela z weterynarzem.

### Zakres

- System rezerwacji wizyt.
- Podstawowy kalendarz wizyt.
- Status wizyty.
- Historia wizyt.
- Czat z weterynarzem.
- Powiadomienia o zaleceniach, szczepieniach i wizytach.

## Etap 6: Funkcje społecznościowe

Celem szóstej wersji jest dodanie funkcji wokół codziennego życia zwierzęcia, niezwiązanych bezpośrednio z dokumentacją medyczną.

### Zakres

- Grupy spacerowe.
- Dodawanie znajomych zwierząt.
- Zapraszanie przez kod QR lub krótko ważny kod.
- Profil znajomego zwierzęcia.
- Opcjonalna widoczność wybranych informacji publicznych.

## Priorytet realizacji

1. MVP: książeczka zdrowia, role, jedno zwierzę, historia, udostępnianie weterynarzowi.
2. Kolejna wersja: pożegnane zwierzęta i transfer danych.
3. Wiele zwierząt i podstawy monetyzacji.
4. Schroniska, hodowle i rola opiekuna.
5. Rezerwacje i czat.
6. Funkcje społecznościowe.
