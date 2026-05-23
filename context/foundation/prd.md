---
project: "Zwierzątko"
version: 1
status: draft
created: 2026-05-23
context_type: greenfield
product_type: "web-app + mobile"
target_scale:
  users: small
  qps: "# TODO: qps — see Open Questions"
  data_volume: "# TODO: data_volume — see Open Questions"
timeline_budget:
  mvp_weeks: 3
  hard_deadline: null
  after_hours_only: true
---

# Zwierzątko PRD

## Vision & Problem Statement

Właściciel zwierzęcia domowego, na MVP właściciel jednego psa lub kota, przez lata opieki nad zwierzęciem gubi lub rozprasza historię zdrowia między papierową książeczką, kartkami z wizyt i różnymi gabinetami weterynaryjnymi. Problem pojawia się szczególnie przy przeprowadzkach, zmianie weterynarza, kolejnej wizycie albo próbie sprawdzenia, co, kiedy i dlaczego wydarzyło się w leczeniu zwierzęcia.

Historia zdrowia zwierzęcia powinna należeć do właściciela, bo weterynarze i miejsca zamieszkania mogą się zmieniać, ale ciągłość danych powinna zostać przy opiekunie. Dzisiejszy koszt to ryzyko zgubienia książeczki lub zaleceń zapisanych na papierze, ręczne odtwarzanie historii i trudność w szybkim przekazaniu pełnego kontekstu nowemu weterynarzowi.

## User & Persona

Primary persona: właściciel jednego psa lub kota, który chce mieć trwałą, łatwo dostępną historię zdrowia zwierzęcia pod ręką przez wiele lat opieki. Sięga po produkt, gdy dodaje dane zwierzęcia, zapisuje zalecenia po wizycie, sprawdza wcześniejsze leczenie albo potrzebuje mieć kontekst zdrowotny pod ręką podczas rozmowy z weterynarzem.

## Success Criteria

### Primary

- W ciągu 3 tygodni pracy po godzinach właściciel może zarejestrować konto, dodać jedno zwierzę, uzupełnić jego podstawowe dane i ręcznie prowadzić historię zdrowia.
- Właściciel może rozpocząć wpis wizyty i uzupełnić prosty formularz/kreator: sekcję opisu jako pole tekstowe oraz sekcję leków z nazwą leku, dawkowaniem i możliwością dodania kolejnego leku; zapisany wpis trafia do historii zdrowia zwierzęcia.

### Secondary

- Właściciel może podać link do profilowego zdjęcia zwierzęcia bez przechowywania samego pliku zdjęcia w systemie.
- Pole rasy ma podpowiedzi z predefiniowanej listy.

### Guardrails

- Właściciel widzi tylko swoje zwierzę.
- Notatki z wizyt nie znikają po wylogowaniu.
- Usunięcie konta usuwa dane użytkownika z systemu.

## User Stories

### US-01: Właściciel zapisuje formularz wizyty

- **Given** zalogowany właściciel ma dodane jedno zwierzę
- **When** rozpoczyna wpis wizyty, uzupełnia sekcję opisu oraz dodaje jeden lub więcej leków z dawkowaniem
- **Then** zapisany wpis pojawia się w historii zdrowia tego zwierzęcia i jest dostępny po ponownym zalogowaniu

#### Acceptance Criteria

- Właściciel nie może zapisać wpisu wizyty dla zwierzęcia, którego nie posiada.
- Zapisany wpis pozostaje widoczny po wylogowaniu i ponownym zalogowaniu.
- Pusty formularz wizyty nie jest zapisywany jako element historii zdrowia.
- Właściciel może dodać kolejny lek w sekcji leków przez plusik.
- Każdy wpis leku zawiera nazwę leku i dawkowanie.

## Functional Requirements

### Konto i role

- FR-001: Właściciel może zarejestrować konto i zalogować się. Priority: must-have
  > Socrates: Counter-argument considered: tarcie rejestracji może zniechęcić do zapisania pierwszej notatki. Resolution: kept; konto i logowanie zostają w MVP.
- FR-002: Właściciel może usunąć konto i swoje dane. Priority: must-have
  > Socrates: Counter-argument considered: usuwanie danych może zabrać czas z głównego przepływu historii zdrowia. Resolution: kept; usunięcie konta i danych zostaje w MVP.

### Profil zwierzęcia

- FR-003: Właściciel może dodać jedno zwierzę. Priority: must-have
  > Socrates: Counter-argument considered: ograniczenie do jednego zwierzęcia frustruje część właścicieli. Resolution: kept; docelowo będzie więcej zwierząt, ale dla MVP wystarczy jedno.
- FR-004: Właściciel może uzupełnić podstawowe dane zwierzęcia: imię, data urodzenia albo wiek, waga, rasa, link do zdjęcia profilowego i dodatkowe informacje. Priority: must-have
  > Socrates: Counter-argument considered: zbyt wiele pól opóźnia zapis pierwszej wizyty. Resolution: kept; właściciel chce widzieć imię i zdjęcie swojego zwierzęcia, więc profil zostaje częścią wartości.

### Historia zdrowia

- FR-005: Właściciel może ręcznie dodać wcześniejszy wpis historii zdrowia. Priority: must-have
  > Socrates: Counter-argument considered: uzupełnianie historii wstecz może być pracochłonne i zniechęcające. Resolution: kept; to część MVP.
- FR-006: Właściciel może rozpocząć wpis wizyty. Priority: must-have
  > Socrates: Counter-argument considered: osobne rozpoczęcie wizyty może być nadmiarowe wobec prostego "dodaj wpis". Resolution: kept; to część MVP.
- FR-007: Właściciel może uzupełnić sekcję opisu wizyty jako pole tekstowe. Priority: must-have
  > Socrates: Counter-argument considered: samo pole tekstowe jest mało strukturalne i może utrudnić późniejsze filtrowanie szczepień, leków i dawek. Resolution: revised; opis zostaje jako część formularza wizyty, a leki są wydzielone do osobnej sekcji.
- FR-008: Właściciel może dodać w sekcji leków nazwę leku i dawkowanie oraz użyć plusika, żeby dodać kolejny lek. Priority: must-have
  > Socrates: Counter-argument considered: wydzielenie leków zwiększa zakres MVP względem jednego pola tekstowego. Resolution: kept; to nadal prosty kreator, który lepiej symuluje notatki z realnej wizyty.
- FR-009: Właściciel może zobaczyć historię zdrowia zwierzęcia. Priority: must-have
  > Socrates: Counter-argument considered: bez filtrowania lub osi czasu historia szybko stanie się nieczytelna. Resolution: kept; podgląd historii zostaje w MVP.

## Non-Functional Requirements

- Zapisane dane profilu zwierzęcia i historii zdrowia pozostają dostępne po wylogowaniu i ponownym zalogowaniu.
- Właściciel nie widzi danych cudzych zwierząt, a cudzy właściciel nie widzi jego danych.
- Aplikacja jest używalna na telefonie podczas wizyty u weterynarza.

## Business Logic

Aplikacja porządkuje wpisy zdrowotne zwierzęcia według daty i typu zdarzenia, żeby właściciel mógł szybko odtworzyć przebieg leczenia.

Reguła konsumuje dane podane przez właściciela: datę lub moment zdarzenia, typ wpisu zdrowotnego, opis wizyty oraz listę leków z dawkowaniem. Wynikiem jest uporządkowana historia zdrowia widoczna z poziomu karty zwierzęcia.

Właściciel spotyka tę regułę po dodaniu lub zapisaniu wpisu: wpis trafia do historii zwierzęcia w takim miejscu, aby później można było wrócić do przebiegu leczenia bez przeszukiwania papierowych notatek.

## Access Control

Użytkownik dostaje się do aplikacji przez konto z logowaniem email + hasło. Dane zdrowotne są przypisane do konta użytkownika i mają być dostępne długoterminowo.

MVP ma jedną aktywną rolę: właściciel. Właściciel może zarządzać swoim kontem, swoim jednym zwierzęciem, profilem zwierzęcia i historią zdrowia. Właściciel widzi tylko swoje zwierzę i swoje wpisy.

Konto weterynarza, konto administratora, zatwierdzanie weterynarzy oraz dopisywanie wizyt przez weterynarza są poza MVP.

## Non-Goals

- Brak importera historii w MVP — wcześniejsza historia zdrowia jest dodawana ręcznie.
- Brak kont weterynarza i administratora w MVP — pierwsza wersja skupia się na właścicielu prowadzącym historię zdrowia swojego zwierzęcia.
- Brak przechowywania plików zdjęć — właściciel podaje link do zdjęcia profilowego zwierzęcia.
- Brak rezerwacji, kalendarza, czatu i powiadomień — komunikacja i organizacja wizyt są poza pierwszą wersją.
- Brak przypomnień o lekach i odznaczania podanej dawki — to dalszy etap po prostym formularzu wizyty.
- Brak funkcji społecznościowych — MVP skupia się na prywatnej historii zdrowia zwierzęcia.

## Open Questions

1. **Jaki jest ballpark QPS dla pierwszej wersji?** — Owner: user. Block: no; potrzebne do domknięcia frontmatter `target_scale.qps`.
2. **Jaki jest ballpark wolumenu danych dla pierwszej wersji?** — Owner: user. Block: no; potrzebne do domknięcia frontmatter `target_scale.data_volume`.
