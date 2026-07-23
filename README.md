# NeoRepo

> **Status projektu: Draft / discovery**
>
> Repozytorium zawiera początkową dokumentację koncepcyjną. Zakres, architektura i model wdrożenia nie są jeszcze decyzjami ostatecznymi.

NeoRepo to koncepcja uczelnianego repozytorium nagrań i materiałów cyfrowych. System ma porządkować cały cykl życia materiału: od pobrania ze źródła, przez weryfikację, przetwarzanie i opis, aż po publikację, retencję oraz archiwizację.

Projekt wyrasta z potrzeby zastąpienia rozproszonych skryptów i ręcznych operacji spójnym produktem, który daje uczelni kontrolę nad materiałami, ich pochodzeniem, integralnością i dostępnością.

## Główne cele

- automatyczne lub kontrolowane pobieranie nagrań z systemów zewnętrznych,
- centralny katalog nagrań i materiałów,
- przechowywanie oryginałów oraz jawnie opisanych wersji pochodnych,
- kontrola integralności plików,
- możliwość kompresji, transkrypcji, generowania napisów i miniatur,
- powiązanie materiałów z kontekstem uczelnianym,
- kontrola publikacji, retencji i archiwizacji,
- pełna historia pochodzenia i przetwarzania artefaktu.

## Czym NeoRepo nie jest

NeoRepo nie jest:

- repozytorium Git dla dużych plików,
- zwykłym współdzielonym katalogiem sieciowym,
- Document Vault przeznaczonym do dokumentów kandydatów i uczestników,
- systemem LMS,
- publiczną platformą wideo na etapie MVP.

Może integrować się z takimi systemami, ale powinien zachować własną odpowiedzialność domenową.

## Wstępny podział odpowiedzialności

```text
Źródła materiałów
    ↓
Ingestion / import
    ↓
Weryfikacja i rejestracja obiektu
    ↓
Magazyn obiektowy + metadane biznesowe
    ↓
Przetwarzanie i artefakty pochodne
    ↓
Publikacja / udostępnienie / archiwizacja
```

## Dokumentacja

- [Wizja produktu](docs/product/vision.md)
- [Wstępna architektura](docs/architecture/architecture_draft.md)
- [Rekomendacje dotyczące niezmiennego magazynu obiektów](docs/architecture/immutable_object_storage.md)
- [Wstępny zakres MVP i post-MVP](docs/roadmap/mvp_and_post_mvp.md)

## Stan prac

Obecny etap obejmuje:

- uporządkowanie celu produktu,
- wydzielenie odpowiedzialności domenowych,
- zapis podstawowych zasad przechowywania plików,
- określenie granicy pomiędzy MVP a pomysłami post-MVP.

Kod aplikacji nie został jeszcze rozpoczęty.

## Założenia robocze

- dokumentacja i kod będą źródłem prawdy projektu,
- ważne decyzje architektoniczne będą zapisywane jawnie,
- MVP ma dostarczyć jeden kompletny przepływ end-to-end,
- rozwiązania współdzielone z innymi projektami będą wydzielane dopiero po potwierdzeniu stabilnych, powtarzalnych przypadków użycia,
- złożoność post-MVP nie może wejść do MVP bez mierzalnej potrzeby.

## Licencja

Licencja projektu nie została jeszcze ustalona.