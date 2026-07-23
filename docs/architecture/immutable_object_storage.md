# NeoRepo — rekomendacje dotyczące niezmiennego magazynu obiektów

**Status:** Draft / rekomendacja projektowa  
**Data:** 2026-07-23  
**Zakres:** MVP foundations + post-MVP cases

## 1. Kontekst

NeoRepo ma przechowywać duże pliki oraz ich pochodne: nagrania źródłowe, wersje skompresowane, ścieżki audio, miniatury, napisy, transkrypcje i inne artefakty.

Repozytorium Git nie jest właściwym magazynem dla takich danych. Warto jednak wykorzystać wybrane idee znane z modelu obiektowego Gita:

- niezmienność obiektów,
- identyfikację zawartości za pomocą skrótu kryptograficznego,
- oddzielenie bajtów od nazwy i znaczenia biznesowego,
- współdzielenie jednego obiektu przez wiele referencji,
- jawne pochodzenie kolejnych wersji i artefaktów.

Docelowym wzorcem jest Content-Addressed Storage lub rozwiązanie częściowo adresowane zawartością, oparte na magazynie obiektowym i relacyjnej bazie metadanych.

## 2. Zasady wprowadzane od początku

### 2.1. Pliki nie są nadpisywane

Każda zmiana zawartości tworzy nowy obiekt fizyczny.

Dotyczy to między innymi:

- ponownego importu zmienionego nagrania,
- nowej kompresji,
- poprawionej transkrypcji,
- nowej wersji napisów,
- regeneracji miniatury,
- ręcznej korekty materiału.

Rekord biznesowy może wskazać nowy obiekt jako aktualny, ale wcześniejszy obiekt nie jest modyfikowany w miejscu.

Korzyści:

- zachowanie pochodzenia,
- prostszy audyt,
- bezpieczne ponowienia,
- brak niejawnej utraty źródła,
- możliwość porównania wyników różnych procesorów.

### 2.2. Każdy plik ma SHA-256 i rozmiar zapisany w bazie

Minimalne metadane obiektu:

```text
id
sha256
size_bytes
mime_type
storage_key
created_at
integrity_status
```

Zalecane pola dodatkowe:

```text
storage_backend
content_disposition_name
created_by_process
encryption_key_reference
last_integrity_check_at
```

SHA-256 służy do:

- kontroli integralności,
- wykrywania identycznej zawartości,
- idempotencji importu,
- powiązania wejścia z wynikiem przetwarzania,
- identyfikacji obiektu niezależnie od jego nazwy logicznej.

Hash nie zastępuje:

- szyfrowania,
- kontroli dostępu,
- audytu,
- retencji,
- bezpiecznego usuwania.

### 2.3. Obiekt fizyczny jest oddzielony od biznesowego zastosowania

Obiekt fizyczny opisuje bajty. Nie powinien sam w sobie oznaczać:

- nagrania wykładu,
- materiału szkoleniowego,
- transkrypcji,
- publikacji,
- załącznika,
- wersji aktywnej.

Znaczenie biznesowe należy do osobnych rekordów domenowych.

Przykład:

```text
StoredObject
- techniczny zapis bajtów

MediaAsset
- materiał źródłowy i jego kontekst biznesowy

MediaDerivative
- pochodna konkretnego materiału

Publication
- decyzja o udostępnieniu konkretnej wersji
```

Zmiana nazwy kursu, semestru, prowadzącego lub logicznej nazwy pliku nie powinna powodować przenoszenia albo kopiowania obiektu fizycznego.

### 2.4. Kilka rekordów biznesowych może wskazywać ten sam obiekt fizyczny

Jeden obiekt może być używany w kilku kontekstach, np.:

- materiał przypisany do kilku grup,
- ten sam plik wykryty przy ponowionym imporcie,
- wspólny materiał używany w kilku kursach,
- identyczny artefakt wynikowy utworzony przez dwa ponowione zadania.

Relacja biznesowa nie musi oznaczać kolejnej kopii pliku.

Deduplikacja nie może jednak naruszać:

- izolacji tenantów,
- polityk retencji,
- granic szyfrowania,
- prawa do usunięcia,
- zasad dostępu.

Domyślną granicą deduplikacji powinna być pojedyncza instalacja lub tenant.

## 3. Minimalny model dla MVP

MVP nie musi wdrażać kompletnego CAS. Powinno jednak uniknąć decyzji, które później wymuszą kosztowną migrację.

Minimalny fundament:

1. każdy zapisany plik ma własny identyfikator techniczny,
2. plik nie jest nadpisywany,
3. SHA-256 i rozmiar są obliczane podczas importu,
4. metadane biznesowe nie są zakodowane wyłącznie w ścieżce storage,
5. źródło i artefakt pochodny są osobnymi obiektami,
6. relacja source → derivative jest jawna,
7. powtórzony import może zostać rozpoznany,
8. każde zadanie przetwarzania zapisuje wersję procesora,
9. usunięcie referencji biznesowej nie usuwa automatycznie współdzielonego obiektu,
10. mechanizm garbage collection nie jest domyślnym efektem operacji użytkownika.

## 4. Przykładowy klucz storage

Nie zaleca się używania pełnego kontekstu biznesowego jako trwałego klucza:

```text
/course/2026/group-a/lecture-01.mp4
```

Taka ścieżka staje się problemem po zmianie nazwy, grupy lub klasyfikacji.

Bezpieczniejszy kierunek:

```text
objects/sha256/ab/cd/abcdef...
```

albo:

```text
objects/2026/07/<opaque-object-id>
```

Hash pozostaje w bazie jako identyfikator zawartości, nawet jeżeli fizyczny klucz storage korzysta z nieprzewidywalnego identyfikatora.

Wybór między kluczem opartym bezpośrednio na hashu a kluczem nieprzewidywalnym powinien uwzględnić bezpieczeństwo, szyfrowanie i możliwości backendu storage.

## 5. Kontrola integralności

Minimalny proces:

```text
pobranie / upload
→ obliczenie SHA-256 podczas strumieniowania
→ zapis obiektu
→ zapis metadanych
→ opcjonalna weryfikacja odczytem
```

System powinien umożliwiać późniejszy integrity check:

```text
odczyt obiektu
→ ponowne obliczenie SHA-256
→ porównanie
→ zapis wyniku i daty kontroli
```

Nie ma potrzeby sprawdzania wszystkich obiektów przy każdym odczycie. Polityka powinna zależeć od ryzyka i właściwości storage.

## 6. Pochodzenie artefaktu

Każdy wynik przetwarzania powinien wskazywać:

- obiekt wejściowy,
- typ zadania,
- nazwę procesora,
- wersję procesora,
- parametry wpływające na wynik,
- moment uruchomienia i zakończenia,
- wynikowy obiekt,
- status i diagnostykę błędu.

Przykład:

```text
ProcessingJob
- input_object_id: A
- job_type: COMPRESS_VIDEO
- processor: ffmpeg
- processor_version: 8.x
- parameter_profile: web-standard-v1
- output_object_id: B
```

Pozwala to odtworzyć, dlaczego powstały dwa różne wyniki z tego samego źródła.

## 7. Post-MVP: pełne snapshoty podobne do commitów

Snapshot jest niezmiennym manifestem stanu określonego zakresu, np. kursu, wydarzenia lub semestru.

Przykładowe zastosowania:

- zamknięcie semestru,
- przygotowanie paczki audytowej,
- odtworzenie publikowanego zestawu materiałów,
- eksport kompletnego kursu,
- potwierdzenie, które wersje były aktywne w danym dniu.

Snapshot wskazuje istniejące obiekty i wersje. Nie powinien kopiować ich zawartości.

Przykładowy model:

```text
RepositorySnapshot
- id
- scope_type
- scope_id
- parent_snapshot_id
- manifest_object_id
- created_at
- created_by
- reason
```

## 8. Post-MVP: Merkle trees

Drzewa Merkle'a mogą wspierać:

- weryfikację dużych zbiorów,
- szybkie wykrywanie różnic,
- podpisywanie manifestów,
- potwierdzenie kompletności archiwum,
- synchronizację między węzłami.

Nie są wymagane do podstawowego katalogowania ani przechowywania plików. Powinny zostać wprowadzone dopiero po pojawieniu się konkretnego przypadku, dla którego prosty manifest z hashami jest niewystarczający.

## 9. Post-MVP: deduplikacja pomiędzy instalacjami

Globalna deduplikacja może oszczędzać przestrzeń, ale tworzy ryzyka:

- możliwość wnioskowania, że inny podmiot posiada ten sam plik,
- konflikt polityk retencji,
- trudniejsze trwałe usuwanie,
- współdzielenie skutków błędu,
- skomplikowane zarządzanie szyfrowaniem.

Domyślna rekomendacja: brak deduplikacji między instalacjami i tenantami.

Powrót do decyzji jest zasadny dopiero przy mierzalnym koszcie storage i przygotowanym threat modelu.

## 10. Post-MVP: automatyczne tieringowanie storage

Możliwy model:

- hot storage — aktywne i często odczytywane materiały,
- warm storage — materiały dostępne, ale używane rzadziej,
- cold/archive — materiały retencyjne i archiwalne.

Automatyczne przenoszenie powinno wynikać z pomiarów:

- częstotliwości dostępu,
- rozmiaru kolekcji,
- kosztu storage,
- dopuszczalnego czasu odtworzenia,
- wymagań retencyjnych.

Nie należy implementować tieringu przed poznaniem rzeczywistego profilu użycia.

## 11. Post-MVP: współdzielenie zaszyfrowanych obiektów

Zaawansowany model może obejmować:

- envelope encryption,
- osobne klucze danych i klucze opakowujące,
- re-encryption klucza bez ponownego szyfrowania dużego pliku,
- różne polityki dostępu do jednego obiektu,
- udostępnianie między domenami lub tenantami,
- rotację kluczy.

Jest to obszar wysokiego ryzyka. Nie powinien być implementowany jako własny protokół kryptograficzny. Wymaga dojrzałych mechanizmów KMS/Vault, modelu zagrożeń i konkretnej potrzeby biznesowej.

## 12. Relacja do Document Vault

Document Vault jest odrębną domeną przeznaczoną do dokumentów kandydatów i uczestników.

Możliwe wspólne wzorce:

- niezmienność,
- SHA-256 lub klucz HMAC do deduplikacji,
- magazyn S3/MinIO,
- wersjonowanie,
- kontrola integralności,
- provenance.

Elementy, które powinny pozostać odrębne:

- buckety,
- klucze szyfrujące,
- role i uprawnienia,
- retencja,
- klasyfikacja danych,
- proces usuwania,
- semantyka biznesowa.

NeoRepo nie powinno przechowywać dokumentów osobowych tylko dlatego, że potrafi przechowywać pliki.

## 13. Rekomendacja zakresowa

### W MVP

- niezmienne pliki,
- SHA-256,
- rozmiar obiektu,
- osobny rekord techniczny,
- rozdzielenie storage i domeny,
- jawne pochodzenie artefaktów,
- deduplikacja lokalna wyłącznie tam, gdzie jest bezpieczna i prosta.

### Po MVP

- snapshoty,
- Merkle trees,
- tiering,
- deduplikacja między instalacjami,
- złożone współdzielenie szyfrowanych obiektów.

Zasada decyzyjna: post-MVP wraca do analizy dopiero po pojawieniu się mierzalnego problemu, konkretnego właściciela decyzji i testu potwierdzającego wartość rozwiązania.