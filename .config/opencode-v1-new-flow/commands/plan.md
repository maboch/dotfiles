---
description: "Faza wymagań i planowania: zbierz i zapisz specs/<slug>/REQ.md, następnie utwórz specs/<slug>/PLAN.md (Status: DRAFT), potem deleguj krytykę planu do subagenta reviewer. Użycie: /plan <slug> [--review-only]"
agent: plan
---

Identyfikator funkcji (slug): $1\
Flaga trybu: $2

Polecenie wykonuje kolejno trzy fazy: A) zebranie wymagań B) wygenerowanie planu, B) delegowanie przeglądu planu do subagenta reviewer.\
Jeśli flaga trybu to `--review-only`, pomiń fazy A i B (plan już istnieje i użytkownik chce tylko ponownej krytyki, np. po poprawkach) i przejdź bezpośrednio do fazy B. Jeśli brakuje wtedy `specs/$1/PLAN.md`, zatrzymaj się i poleć użytkownikowi najpierw uruchomić `/plan $1`.

## Faza A — Zebranie wymagań

1. Rozwiąż blokujące niejednoznaczności przy użyciu narzędzia `question`. Drobne kwestie rozstrzygaj samodzielnie i wymień je pod „Decyzje podjęte w Twoim imieniu”.

2. Jeśli zadanie odtwarza istniejące zachowanie, deleguj do subagenta `explore` wydobycie aktualnego zachowania i niejawnych reguł biznesowych niewidocznych w tekście wymagań.

3. Zapisz `specs/<slug>/REQ.md` z sekcjami:
   - Cel: jedno zdanie.
   - Zakres: Włączone (co obejmuje) / Wyłączone (co jest wyraźnie poza zakresem).
   - Zachowanie: ponumerowane opisy zachowania, każdy testowalny.
   - Przypadki brzegowe: lista nieoczywistych danych wejściowych lub stanów.
   - Kryteria akceptacji: ponumerowane AC-1, AC-2, ... Każde musi być konkretne i niezależnie weryfikowalne.
   - Decyzje podjęte w Twoim imieniu: wybory rozstrzygnięte bez pytania użytkownika.
   - Otwarte pytania: wszystko, co blokuje planowanie i wymaga odpowiedzi użytkownika przed /plan.

## Faza B — Generowanie planu

1. Przeczytaj `specs/$1/REQ.md`. Jeśli pliku nie ma, ponów Fazę A. Jeśli „Otwarte pytania” nie są puste, wymień je i zakończ — użytkownik musi je rozstrzygnąć przed planowaniem.

2. Rozpoznaj kod w potrzebnym zakresie: szerokie rozpoznanie repozytorium deleguj do subagenta `explore`, badania zewnętrzne do subagenta `research`.

3. Utwórz plan w FORMACIE PLANU (zobacz instrukcje agenta). Dla każdego kroku podaj:

  * Repozytorium: backend | frontend

  * Wykonawca: sidekick | design

  * Zależności: numery kroków albo brak

  * Specyfikacja: pełny pięcioczęściowy kontrakt (Cel, Pliki, Interfejsy, Ograniczenia, Weryfikacja z dokładnymi poleceniami o ograniczonym zakresie we właściwej formie DEV_ENVIRONMENT)\
    Specyfikacja musi być wykonalna przez agenta, który widział tylko ten plik.

4. Dodaj sekcję „Pokrycie kryteriów akceptacji”: każde AC-n -> kroki + weryfikacja, która dowodzi spełnienia.

5. Zapisz do `specs/$1/PLAN.md` z nagłówkiem `Status: DRAFT`.

## Faza C — Delegowanie przeglądu planu

1. Deleguj do subagenta `reviewer` przez narzędzie `subagent`. Nie przeglądaj planu samodzielnie i nie pomijaj tej fazy. `reviewer` nie ma Twojego kontekstu, więc wyślij samowystarczalny opis (podstaw właściwy identyfikator funkcji):

  > Tryb przeglądu planu dla funkcji: <slug>
  >
  > Przeczytaj `specs/<slug>/REQ.md` i `specs/<slug>/PLAN.md`, a następnie rzeczywiste pliki źródłowe wskazane przez plan.
  >
  > Poza standardowymi kontrolami planu zweryfikuj:
  >
  > * Każde AC z [REQ.md](http://REQ.md) jest pokryte przez co najmniej jeden krok i ma rzeczywiste, uruchamialne polecenie weryfikacji.
  >
  > * Specyfikacja każdego kroku jest wykonalna bez zgadywania: wskazane pliki istnieją, wskazane interfejsy odpowiadają rzeczywistemu kodowi.
  >
  > * Oznaczenia zależności i równoległości są spójne: żadne dwa równoległe kroki nie edytują tego samego pliku.
  >
  > * Polecenia weryfikacji odpowiadają DEV_ENVIRONMENT każdego repozytorium (native albo docker).
  >
  > * Pięcioczęściowy kontrakt Specyfikacji w każdym kroku jest pełny (Cel, Pliki, Interfejsy, Ograniczenia i Weryfikacja są obecne i jednoznaczne).
  >
  > Zwróć FORMAT RAPORTU.

2. Nie stosuj samodzielnie żadnego ustalenia `reviewer` w [PLAN.md](http://PLAN.md). Użytkownik wybiera ustalenia do przyjęcia.

## Raport dla użytkownika

Przedstaw w tej kolejności:

* Krótkie podsumowanie planu (pomiń w trybie `--review-only`).

* Raport `reviewer` dosłownie w FORMACIE RAPORTU (STATUS, USTALENIA, ZWERYFIKOWANO, LUKI).

Następnie zakończ. Użyj narzędzia `question` i zapytaj użytkownika: Wskaż ustalenia do przyjęcia (zwykłym tekstem w tej sesji), a zaktualizujesz [PLAN.md](http://PLAN.md); Uruchom `/plan $1 --review-only`, aby ponowić krytykę po poprawkach; Uruchom `/approve $1`, gdy plan będzie satysfakcjonujący.
