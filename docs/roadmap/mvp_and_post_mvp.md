# NeoRepo — wstępny zakres MVP i post-MVP

**Status:** Draft  
**Data:** 2026-07-23

## 1. Cel roadmapy

Dokument nie jest jeszcze planem sprintów. Wyznacza granicę pomiędzy pierwszym użytecznym przepływem produktu a funkcjami, które powinny zostać odłożone do czasu potwierdzenia potrzeb.

## 2. Zasada MVP

MVP powinno umożliwić przejście jednego materiału przez kompletny, kontrolowany cykl:

```text
źródło
→ import
→ zapis i integralność
→ katalog
→ jedno przetwarzanie pochodne
→ kontrola statusu
```

MVP nie powinno być platformą obejmującą wszystkie rodzaje materiałów, integracji, publikacji i archiwizacji.

## 3. Proponowany zakres MVP

### 3.1. Jedno źródło referencyjne

- integracja z jednym konkretnym źródłem nagrań albo kontrolowany import ręczny,
- zapis identyfikatora zewnętrznego,
- wykrywanie ponowionego importu,
- możliwość bezpiecznego ponowienia po błędzie.

### 3.2. Rejestracja obiektu

- zapis pliku bez nadpisywania,
- SHA-256,
- rozmiar w bajtach,
- typ MIME,
- identyfikator storage,
- status integralności,
- data przyjęcia.

### 3.3. Katalog materiałów

Minimalny widok operatora:

- lista materiałów,
- źródło,
- nazwa logiczna,
- data importu,
- rozmiar,
- status importu,
- status przetwarzania,
- dostępne artefakty.

### 3.4. Szczegóły materiału

- metadane źródła,
- dane obiektu fizycznego,
- historia importu,
- lista procesów,
- lista artefaktów pochodnych,
- diagnostyka błędów.

### 3.5. Jeden proces pochodny

Rekomendowany kandydat: kompresja nagrania.

Proces powinien zapisywać:

- wejście,
- parametry lub profil,
- nazwę i wersję procesora,
- status,
- czas rozpoczęcia i zakończenia,
- wynikowy obiekt,
- kod i opis błędu.

### 3.6. Podstawowe role

Minimalnie:

- operator,
- administrator.

Model autoryzacji musi działać po stronie backendu. Szczegółowe role organizacyjne mogą pozostać poza MVP.

### 3.7. Audyt i diagnostyka

- rozpoczęcie i zakończenie importu,
- wykrycie duplikatu,
- uruchomienie procesu,
- zakończenie procesu,
- błąd i ponowienie,
- podstawowe działania operatora.

### 3.8. Podstawowa obsługa błędów

- jawne statusy,
- komunikat diagnostyczny bez ujawniania sekretów,
- możliwość ponowienia,
- brak pozostawiania rekordu jako pozornie zakończonego,
- rozróżnienie błędu przejściowego i trwałego, jeżeli jest to możliwe.

## 4. Poza MVP

### 4.1. Wiele integracji źródłowych

Kolejne systemy wideokonferencyjne, importy masowe i obserwowane katalogi powinny zostać dodane dopiero po ustabilizowaniu pierwszego kontraktu adaptera.

### 4.2. Rozbudowana publikacja

Poza MVP pozostają:

- publiczny portal wideo,
- CDN,
- zaawansowane linki czasowe,
- osadzanie w wielu systemach,
- rozbudowane statystyki odtworzeń,
- personalizowane reguły dostępu.

MVP może zakończyć się na kontrolowanym udostępnieniu operatorowi lub prostym linku wewnętrznym, zależnie od decyzji produktowej.

### 4.3. Transkrypcja i AI

Po MVP:

- ekstrakcja audio,
- transkrypcja,
- diarization,
- napisy,
- streszczenia,
- klasyfikacja treści,
- wyszukiwanie semantyczne,
- indeks wiedzy.

Warunkiem powinien być stabilny model pochodzenia artefaktów i zasady ochrony danych.

### 4.4. Pełny system retencji

MVP może przechowywać jawne pole polityki lub termin przeglądu. Pełny silnik reguł retencyjnych, legal hold, automatyczne usuwanie i wieloetapowe zatwierdzanie pozostają na później.

### 4.5. Snapshoty i manifesty

- snapshoty kursu lub semestru,
- podpisywane manifesty,
- odtwarzanie historycznej publikacji,
- Merkle trees.

### 4.6. Zaawansowany storage

- automatyczny tiering,
- replikacja między lokalizacjami,
- globalna deduplikacja,
- multi-region,
- złożone mechanizmy KMS,
- współdzielenie zaszyfrowanych obiektów między tenantami.

### 4.7. Uniwersalna platforma plikowa

NeoRepo nie powinno w pierwszej fazie stawać się wspólnym File Service dla WPFormsCRM, Credential Platform, Document Vault i innych projektów.

Możliwe wydzielenie wspólnego komponentu dopiero wtedy, gdy co najmniej trzy niezależne przypadki użycia potwierdzą stabilny wspólny kontrakt.

## 5. Kandydat na kolejne etapy

### Etap 0 — Discovery

- wizja,
- granice domeny,
- wybór przepływu referencyjnego,
- decyzja o stacku,
- model pojęciowy,
- pierwsze ADR.

### Etap 1 — Skeleton

- struktura aplikacji,
- baza danych,
- storage adapter,
- konfiguracja,
- health checks,
- lokalne środowisko uruchomieniowe.

### Etap 2 — Ingestion slice

- pierwsze źródło,
- pobranie,
- SHA-256,
- zapis obiektu,
- idempotencja,
- status i błędy.

### Etap 3 — Operator catalog

- lista,
- szczegóły,
- filtry,
- historia importu,
- diagnostyka.

### Etap 4 — Processing slice

- kolejka lub runner,
- kompresja,
- artefakt pochodny,
- ponowienie,
- obserwowalność.

### Etap 5 — Stabilizacja MVP

- autoryzacja,
- testy krytycznych ścieżek,
- backup i restore,
- podstawowa retencja,
- instrukcja wdrożenia,
- test pilotażowy.

## 6. Kryteria wyjścia z MVP

MVP można uznać za zakończone, gdy:

1. jeden realny typ materiału przechodzi pełny przepływ,
2. oryginał nie jest nadpisywany,
3. integralność można zweryfikować,
4. ponowiony import nie tworzy niekontrolowanych kopii,
5. operator widzi aktualny stan i błędy,
6. jeden proces pochodny tworzy jawnie powiązany artefakt,
7. system można odtworzyć z backupu według opisanej procedury,
8. uprawnienia są egzekwowane po stronie backendu,
9. dokumentacja odpowiada rzeczywistemu zachowaniu,
10. istnieje wynik pilotażu z realnym materiałem.

## 7. Rejestr odłożonej złożoności

Poniższe mechanizmy są świadomie odłożone:

- pełne snapshoty podobne do commitów,
- Merkle trees,
- deduplikacja pomiędzy instalacjami,
- automatyczne tieringowanie storage,
- skomplikowane współdzielenie zaszyfrowanych obiektów,
- pełny Document Vault,
- publiczna platforma streamingowa,
- rozbudowane AI.

Odłożenie nie oznacza odrzucenia. Każdy temat powinien wrócić do analizy dopiero z konkretnym przypadkiem użycia, właścicielem, mierzalną wartością oraz oceną kosztu i ryzyka.