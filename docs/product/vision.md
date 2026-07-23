# NeoRepo — wizja produktu

**Status:** Draft  
**Data:** 2026-07-23  
**Etap:** Discovery

## 1. Problem

Materiały cyfrowe powstające podczas zajęć, szkoleń i wydarzeń uczelnianych są często rozproszone pomiędzy systemami wideokonferencyjnymi, lokalnymi dyskami, współdzielonymi katalogami i ręcznymi procesami pracowników.

Powoduje to między innymi:

- brak jednego wiarygodnego katalogu materiałów,
- trudność w ustaleniu, który plik jest oryginałem,
- duplikowanie nagrań,
- ręczne pobieranie i kompresowanie plików,
- brak spójnej informacji o pochodzeniu materiału,
- brak kontroli nad retencją i archiwizacją,
- niejasny status publikacji,
- trudność w automatyzacji transkrypcji i innych procesów pochodnych.

## 2. Wizja

NeoRepo ma być uczelnianym systemem zarządzania cyklem życia nagrań i materiałów cyfrowych.

System powinien umożliwiać przejście od rozproszonych, ręcznych operacji do kontrolowanego procesu:

```text
pozyskanie
→ rejestracja
→ weryfikacja
→ przechowanie
→ przetwarzanie
→ publikacja
→ retencja
→ archiwizacja lub usunięcie
```

NeoRepo nie ma być tylko miejscem składowania plików. Ma przechowywać również wiedzę o tym:

- skąd materiał pochodzi,
- z jakim wydarzeniem lub zajęciami jest związany,
- która wersja jest źródłowa,
- jakie artefakty pochodne powstały,
- jakim procesem zostały utworzone,
- kto i kiedy podjął decyzję o publikacji,
- jaka polityka retencji obowiązuje.

## 3. Główni użytkownicy

### Operator repozytorium

Pracownik odpowiedzialny za kontrolę importu, przetwarzania, publikacji i archiwizacji materiałów.

### Administrator systemu

Osoba odpowiedzialna za konfigurację integracji, polityk, storage, uprawnień i obserwowalności.

### Właściciel biznesowy materiału

Jednostka, prowadzący lub organizator odpowiedzialny za znaczenie i dopuszczalny sposób wykorzystania materiału.

### Odbiorca materiału

Student, uczestnik, pracownik lub inna osoba uzyskująca dostęp przez system docelowy lub kanał publikacji.

## 4. Propozycja wartości

NeoRepo powinno dostarczać:

- jednoznaczny katalog materiałów,
- ochronę plików źródłowych przed nadpisaniem,
- kontrolę integralności,
- jawne wersjonowanie i pochodzenie artefaktów,
- automatyzację powtarzalnych procesów,
- możliwość audytu decyzji i przetwarzania,
- przygotowanie do skalowania storage bez uzależnienia od struktury katalogów,
- podstawę pod późniejszą transkrypcję, indeksowanie i funkcje AI.

## 5. Granice domeny

NeoRepo odpowiada za:

- import materiału,
- katalogowanie,
- przechowywanie,
- kontrolę integralności,
- relacje pomiędzy źródłem a artefaktami pochodnymi,
- status przetwarzania,
- status publikacji,
- retencję i archiwizację.

NeoRepo nie powinno przejmować odpowiedzialności za:

- pełny proces dydaktyczny LMS,
- rekrutację uczestników,
- przechowywanie dokumentów osobowych uczestników,
- wystawianie poświadczeń i certyfikatów,
- zarządzanie tożsamością jako osobną domeną,
- rozbudowaną publiczną platformę streamingową w MVP.

## 6. Relacja do innych koncepcji

### Document Vault

Document Vault jest odrębnym przyszłym kontekstem dla dokumentów kandydatów i uczestników. Może wykorzystywać podobne wzorce techniczne, ale musi mieć własne zasady dostępu, retencji, szyfrowania i audytu.

### Credential Platform

System poświadczeń może korzystać z artefaktów i metadanych, ale sam odpowiada za wydawanie, weryfikację i unieważnianie poświadczeń.

### LMS i systemy uczelniane

NeoRepo może udostępniać materiały lub ich identyfikatory innym systemom, ale nie powinno kopiować ich pełnej logiki biznesowej.

## 7. Zasady produktowe

1. Najpierw pełny przepływ end-to-end, później wygoda i automatyzacja.
2. Oryginał nie jest nadpisywany.
3. Każdy artefakt ma jawne pochodzenie.
4. System rozróżnia stan techniczny od decyzji biznesowej.
5. Metadane biznesowe nie zależą od fizycznej ścieżki pliku.
6. Funkcje post-MVP nie wchodzą do MVP bez mierzalnej potrzeby.
7. Każda automatyzacja musi mieć status, diagnostykę błędu i możliwość bezpiecznego ponowienia.

## 8. Wstępna definicja sukcesu MVP

MVP jest użyteczne, gdy operator może:

1. zarejestrować źródło materiału,
2. pobrać lub przesłać nagranie,
3. potwierdzić zapis i integralność pliku,
4. zobaczyć materiał w katalogu,
5. uruchomić jeden proces pochodny, np. kompresję,
6. rozróżnić źródło od wyniku przetwarzania,
7. ustalić aktualny status materiału,
8. bezpiecznie ponowić operację po błędzie.

## 9. Otwarte pytania

- Które źródło materiałów będzie pierwszą integracją referencyjną?
- Czy pierwsze wdrożenie ma działać wyłącznie lokalnie, czy od początku w modelu sieciowym?
- Jaki jest minimalny zakres publikacji w MVP?
- Kto jest właścicielem decyzji o retencji?
- Czy materiał ma być przypisywany do kursu, wydarzenia, przedmiotu, grupy, czy bardziej ogólnego kontenera?
- Czy przetwarzanie ma być synchroniczne, kolejkowane, czy mieszane?

Odpowiedzi na te pytania powinny zostać zapisane w kolejnych dokumentach lub ADR.