# NeoRepo — wstępna architektura

**Status:** Draft  
**Data:** 2026-07-23  
**Etap:** Discovery

## 1. Cel dokumentu

Dokument opisuje roboczy podział systemu na odpowiedzialności. Nie przesądza jeszcze o frameworku, języku implementacji, modelu wdrożenia ani konkretnym dostawcy storage.

## 2. Główna zasada

NeoRepo powinno rozdzielać:

- obiekt fizyczny,
- znaczenie biznesowe materiału,
- proces przetwarzania,
- decyzję o publikacji,
- politykę retencji.

Nie należy traktować ścieżki pliku jako modelu domenowego.

## 3. Wstępne komponenty

```text
[Source adapters]
        ↓
[Ingestion service]
        ↓
[Object registry] ─────→ [Object storage]
        ↓
[Media catalog]
        ↓
[Processing jobs]
        ↓
[Derivatives / artifacts]
        ↓
[Publication and retention]
```

### Source adapters

Odpowiadają za komunikację z zewnętrznymi źródłami, np. systemem wideokonferencyjnym, importem ręcznym lub katalogiem wymiany.

Adapter powinien:

- mapować dane zewnętrzne na jawny kontrakt wewnętrzny,
- przechowywać identyfikator źródłowy,
- wspierać idempotencję,
- nie zawierać logiki katalogowania ani publikacji.

### Ingestion service

Odpowiada za bezpieczne przyjęcie materiału:

- pobranie lub przyjęcie strumienia,
- walidację podstawowych parametrów,
- obliczenie SHA-256,
- ustalenie rozmiaru,
- zapis do storage,
- rejestrację obiektu,
- obsługę ponowień i błędów.

### Object registry

Rejestr technicznych obiektów fizycznych. Przechowuje metadane potrzebne do integralności, lokalizacji i zarządzania cyklem życia obiektu.

### Object storage

Przechowuje niezmienne bajty. Docelowo powinien to być magazyn zgodny z S3, np. MinIO lub usługa chmurowa, ale wybór nie jest jeszcze decyzją.

### Media catalog

Przechowuje biznesowy opis materiału i jego kontekst, np. nazwę logiczną, źródło, typ, właściciela, powiązanie z wydarzeniem lub kursem oraz status.

### Processing jobs

Rejestruje i wykonuje procesy pochodne, np.:

- kompresję,
- ekstrakcję audio,
- generowanie miniatur,
- transkrypcję,
- generowanie napisów,
- analizę techniczną.

Każde zadanie powinno mieć jawny status, wejście, wynik, wersję procesora, diagnostykę błędu i możliwość bezpiecznego ponowienia.

### Publication and retention

Odpowiada za decyzje o widoczności, okresie przechowywania, archiwizacji i usunięciu. Stan techniczny pliku nie może automatycznie oznaczać zgody na publikację.

## 4. Roboczy model pojęciowy

```text
StoredObject
- id
- sha256
- size_bytes
- mime_type
- storage_key
- created_at
- integrity_status

MediaAsset
- id
- source_object_id
- logical_name
- source_type
- external_source_id
- business_context
- lifecycle_status

ProcessingJob
- id
- input_object_id
- job_type
- processor_name
- processor_version
- status
- started_at
- finished_at
- error_code

MediaDerivative
- id
- media_asset_id
- stored_object_id
- processing_job_id
- derivative_type
- created_at
```

Nazwy są robocze i powinny zostać zweryfikowane podczas modelowania domeny.

## 5. Przepływ referencyjny MVP

```text
1. Operator lub harmonogram wskazuje materiał źródłowy.
2. Adapter pobiera metadane ze źródła.
3. Ingestion pobiera zawartość i oblicza SHA-256.
4. Obiekt zostaje zapisany bez nadpisywania istniejącej zawartości.
5. Powstaje rekord techniczny StoredObject.
6. Powstaje rekord biznesowy MediaAsset.
7. Operator widzi materiał i jego status.
8. Operator uruchamia proces pochodny.
9. Wynik jest zapisywany jako nowy StoredObject.
10. MediaDerivative wiąże wynik ze źródłem i zadaniem.
```

## 6. Zasady projektowe

- Pliki są niezmienne.
- Baza danych jest źródłem prawdy dla relacji i stanu biznesowego.
- Storage jest źródłem prawdy dla bajtów, nie dla semantyki.
- Integracje zewnętrzne są izolowane adapterami.
- Operacje importu i przetwarzania są idempotentne.
- Każda pochodna wskazuje źródło i proces, który ją utworzył.
- Publikacja jest osobną decyzją od zakończenia przetwarzania.
- Retencja jest jawna, nie wynika przypadkiem ze struktury katalogu.
- Wspólne komponenty dla innych projektów nie są wydzielane przed potwierdzeniem stabilnego wzorca.

## 7. Bezpieczeństwo i audyt

Minimalny kierunek:

- kontrola dostępu po stronie backendu,
- szyfrowanie transportu,
- szyfrowanie storage lub warstwy dyskowej,
- sekrety poza repozytorium,
- rejestrowanie istotnych operacji operatora,
- rejestrowanie importów, ponowień i wyników procesów,
- bezpieczne komunikaty błędów,
- polityka usuwania obejmująca obiekt i wszystkie referencje.

Szczegółowy model uprawnień wymaga osobnego dokumentu.

## 8. Obserwowalność

System powinien umożliwiać ustalenie:

- czy import działa,
- ile materiałów oczekuje,
- które zadania zakończyły się błędem,
- ile miejsca zajmują źródła i pochodne,
- czy hash pliku nadal odpowiada zapisanej wartości,
- jaki jest czas przetwarzania,
- które operacje są wielokrotnie ponawiane.

## 9. Otwarte decyzje techniczne

- język i framework aplikacji,
- PostgreSQL lub inna baza relacyjna,
- MinIO, S3 lub storage lokalny dla pierwszego wdrożenia,
- kolejka zadań i worker,
- sposób autoryzacji,
- model instalacji,
- format kontraktów integracyjnych,
- mechanizm publikacji materiału,
- zakres antywirusowej lub technicznej walidacji plików.

Decyzje te powinny wynikać z konkretnego przepływu MVP, a nie z potrzeby zbudowania pełnej platformy z wyprzedzeniem.