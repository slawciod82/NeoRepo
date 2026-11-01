# Granice domen: NeoRepo, UAG, PCG i Rep.o_O.ps

**Ustalenie:** 2026-11-01. **Status:** podział odpowiedzialności zatwierdzony koncepcyjnie; integracje są projektowane, nie wdrożone.

| Komponent | Własność danych i odpowiedzialność | Nie należy do jego zakresu |
| --- | --- | --- |
| **Microsoft Entra ID** | Uwierzytelnienie kont MS365 | Harmonogram i akademickie uprawnienia |
| **PCG Dziekanat10** | Referencyjne dane studentów, grup, zapisów i harmonogramu | Nagrania, sterowanie bramkami, lista obecności UAG |
| **PCG Wirtualna Uczelnia / mStudent** | Istniejące kanały użytkownika, już z logowaniem MS365 | Nowa baza nagrań lub niezależny silnik obecności |
| **UAG** | Dostęp fizyczny i docelowo Attendance: zgłoszenia, dowody uczestnictwa, zatwierdzona obecność | Ingest, storage i udostępnianie nagrań |
| **NeoRepo** | Docelowy nowy cykl życia nagrań i materiałów: import, integralność, przetwarzanie, publikacja, dostęp i retencja | Bramy, rejestracja i zatwierdzanie obecności, źródłowe dane akademickie, pełny portal studenta |
| **Rep.o_O.ps / RepoOps** (`CMSnapshootUI`) | Obsługa **Legacy**: filesystem REPOSITORY/SOURCE, snapshoty, odzyskiwanie, Maker/Puter, katalogi z loginem/hasłem wspólnym dla grupy | Nowy portal VOD, DB-first katalog i uprawnienia studentów NeoRepo, Attendance |
| **ClickMeeting / potencjalnie Teams** | Świadczenie usługi spotkań / źródło danych o połączeniu i nagrań | Autorytatywna decyzja o zatwierdzeniu obecności |

## Wspólne zasady

1. Ten sam Entra ID / konto MS365 może uwierzytelniać w PCG, UAG i NeoRepo, ale **nie współdzielimy sesji ani nie przekazujemy tokenu jednej aplikacji drugiej**. Weryfikujemy `(tenant_id, object_id)` ↔ identyfikator PCG; dostępność OID w API PCG pozostaje niepotwierdzona.
2. Nie budujemy trzeciego pełnego portalu dla studentów: WU/mStudent pozostają istniejącymi kanałami. Integracja interfejsu i deep linki są **do potwierdzenia z PCG**, nie gwarantowane.
3. Wspólna referencja do konkretnych zajęć pozwala korelować obecność UAG z nagraniem NeoRepo. Źródłem harmonogramu jest PCG; API i mapowanie identyfikatorów wymagają weryfikacji.
4. **UAG** gromadzi dowody obecności z NFC/QR, ewentualnie sygnałów fizycznego przejścia oraz dostawców spotkań. `ACCESS_GRANTED` nie jest równoznaczne z `PASSAGE_CONFIRMED`, a zgłoszenie nie jest zatwierdzeniem. Zamknięcie rejestracji w trakcie zajęć jest odwracalne; po planowym zakończeniu backend zamyka przyjmowanie zgłoszeń, pozostawiając audytowane korekty.
5. **NeoRepo** odpowiada za nagrania i ich politykę publikacji na podstawie uprawnień, a nie za wystawianie obecności. Docelowy adapter ClickMeeting/Teams wymaga osobnego projektu i testów; migracja do Teams **nie została zatwierdzona**.
6. **RepoOps** pozostaje legacy-only, choć istniejące Downloader/Compressor lub wyodrębnione techniczne API mediów mogą być wykorzystane przez inne systemy po zweryfikowaniu kontraktu. Nie kopiujemy legacy group paths, `.htaccess` ani wspólnych haseł do NeoRepo.
7. Bazy danych i reguły domenowe pozostają po stronie właścicieli; komunikacja przez jawne, wersjonowane interfejsy. Brak automatycznej zgody na przetwarzanie danych, produkcyjne integracje czy zmiany zakresów sprintów.

## Stan i pytania otwarte

- **UAG:** działający hardware Stage 1, ale SSO, PCG i Attendance dopiero planowane.
- **NeoRepo:** discovery/draft; kontrakty spotkań, publikacji i SSO do zaprojektowania.
- **RepoOps:** priorytet bieżących sprintów = bezpieczeństwo i usprawnienie Legacy; nie angażować do nowych funkcji UAG/NeoRepo.
- Otwarte: API PCG i OID, integracja UI WU/mStudent, dane przejścia z GASTOP, polityka obecności, retencja, prawa nagrań i dostępność API platformy spotkań.

**Referencje:** [UAG boundary](https://github.com/slawciod82/UniversityAccessGateway/blob/main/docs/14-ecosystem-boundaries.md) (docelowo po merge), [RepoOps ADR-004](https://github.com/slawciod82/CMSnapshootUI/blob/feature/repoops-foundation/docs/adr_004_shared_media_pipeline_api.md), [NeoRepo vision](product/vision.md).
