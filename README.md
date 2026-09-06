# Sanctuary Distribution — Centrum Wydań

Centralne, publiczne źródło dystrybucji aplikacji Zielonego Sanktuarium. Repozytorium nie przechowuje kodu źródłowego aplikacji ani sekretów. Jego rolą jest publikowanie **podpisanych katalogów wydań** oraz wyłącznie tych binariów, które mają być dostępne dla Sanctuary Hub.

## Architektura

- każda aplikacja zachowuje własne repozytorium i własny cykl wersji;
- **Sanctuary Hub** jest wspólnym klientem GUI do instalacji, aktualizacji i rollbacku;
- automatyzacje korzystają z tego samego mechanizmu przez tryb CLI/background Hub, zamiast utrzymywać osobny updater w każdej aplikacji;
- **Sanctuary Distribution** jest jednym źródłem prawdy o tym, co wolno zainstalować;
- katalog jest podpisywany lokalnym kluczem ECDSA, a artefakty są weryfikowane SHA-256 i metadanymi pakietu.

Przepływ:

`repo aplikacji → build/test → wydanie → podpisany katalog → Sanctuary Distribution → Sanctuary Hub → instalacja/aktualizacja`

## Kanały

Docelowo używamy trzech kanałów, bez kopiowania logiki aktualizacji do aplikacji:

- `catalog-stable` — domyślny kanał produkcyjny;
- `catalog-beta` — wersje do wcześniejszych testów;
- `catalog-dev` — szybkie wydania deweloperskie.

Każdy kanał jest osobnym GitHub Release zawierającym `catalog.json` i `catalog.sig`. Włączenie Beta/Dev po stronie klienta może być dodane niezależnie; Stable pozostaje bezpiecznym domyślnym kanałem.

## Kontrakt katalogu

Katalog wskazuje m.in. nazwę i identyfikator aplikacji, repozytorium źródłowe, platformę, wersję, typ źródła/instalatora, dokładny artefakt, SHA-256, rozmiar, release notes i wymagane dane tożsamości pakietu. Klient ufa dopiero katalogowi po poprawnej weryfikacji podpisu.

W repozytorium znajduje się wyłącznie **szablon** katalogu. Runtime nie powinien ufać plikowi z gałęzi `main`; źródłem prawdy są podpisane zasoby GitHub Release.

## GUI i automatyzacja

Dla człowieka głównym interfejsem pozostaje `Sanctuary Hub.exe`. Automatyzacje i zadania w tle powinny wywoływać ten sam silnik przez argumenty CLI/background, aby GUI i automatyzacja miały identyczne reguły bezpieczeństwa, wersjonowania i rollbacku.
