# Katalog wydań

Ten katalog opisuje kontrakt używany przez Centrum Wydań. Pliki w gałęzi Git są dokumentacją i bezpiecznym bootstrapem — **nie są źródłem zaufania dla klienta**.

Źródłem runtime są podpisane zasoby GitHub Release:

- `catalog-stable/catalog.json` + `catalog.sig`
- `catalog-beta/catalog.json` + `catalog.sig`
- `catalog-dev/catalog.json` + `catalog.sig`

Publisher z repozytorium `ZieloneSanktuarium/Sanctuary-Hub` powinien przed publikacją:

1. zweryfikować repozytorium, tag i dokładny commit aplikacji;
2. pobrać rzeczywisty artefakt;
3. ustalić jego rozmiar i SHA-256;
4. zweryfikować tożsamość pakietu/instalatora;
5. zbudować katalog zgodny ze wspólnym kontraktem;
6. podpisać `catalog.json` lokalnym kluczem ECDSA;
7. opublikować `catalog.json` i `catalog.sig` do odpowiedniego kanału.

Nie wolno publikować wpisów z fikcyjnym hashem, wersją, commitem, adresem artefaktu ani tożsamością wydawcy.
