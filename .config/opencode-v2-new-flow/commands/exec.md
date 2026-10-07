---
description: "Wykonaj zatwierdzony PLAN.md dla bieżącego repozytorium, a następnie deleguj przegląd różnic do subagenta `reviewer`. Użycie: /exec <slug> [--review-only]"
agent: orchestrator
---
Identyfikator funkcji (slug): $1. TRYB ZATWIERDZONEGO PLANU.
Flaga trybu: $2

Działasz w trybie zatwierdzonego planu. Dokładnie przestrzegaj sekcji Tryb zatwierdzonego planu z instrukcji agenta.

Polecenie wykonuje kolejno dwie fazy: A) wykonanie zatwierdzonego planu, B) delegowanie przeglądu różnic do subagenta `reviewer`.
Jeśli flaga trybu to `--review-only`, pomiń fazę A (wykonanie już nastąpiło i użytkownik chce tylko ponownego przeglądu różnic, np. po poprawkach) i przejdź bezpośrednio do fazy B. Jeśli brakuje wtedy `specs/$1/EXEC.md`, zatrzymaj się i poleć użytkownikowi najpierw uruchomić `/exec $1`.

## Faza A — Wykonanie

1. Przeczytaj `specs/$1/PLAN.md`. Jeśli Status nie jest APPROVED: natychmiast zatrzymaj się i poinformuj użytkownika — niczego nie wykonuj i nie uruchamiaj fazy B.
2. Plan jest zatwierdzony przez użytkownika. Pomiń kroki 2–4 przebiegu pracy (rozpoznanie, ponowne planowanie, bramka zatwierdzenia). Nie planuj ponownie.
3. Rozpoznaj bieżące repozytorium (backend = Symfony/PHP, frontend = Next.js/React). Wykonuj tylko kroki, których pole „Repozytorium” odpowiada temu repozytorium.
4. Dla każdego kroku: deleguj do jego Wykonawcy (sidekick lub design), przekazując Specyfikację dosłownie. Dodawaj tylko fakty z już ukończonych kroków potrzebne specyfikacji zależnej.
5. Wykonuj równolegle tam, gdzie krok ma oznaczenie „równoległy” i nie występują konflikty plików.
6. Przejrzyj każdy raport wykonawcy zgodnie z krokami 7–8 przebiegu pracy (pętla informacji zwrotnej, potem dyktowanie po dwóch nietrafionych próbach).
7. Przy Odstępstwie (kroku nie da się wykonać bez zmiany planu): zatrzymaj ten krok, dopisz go pod „## Odstępstwa” w `specs/$1/PLAN.md` z jednowierszowym wyjaśnieniem i zgłoś użytkownikowi. Nigdy nie improwizuj zmiany planu.
8. Uruchom weryfikację o ograniczonym zakresie dla każdego ukończonego kroku. Dopisz podsumowanie do `specs/$1/EXEC.md` (utwórz, jeśli brakuje). Nie zatwierdzaj zmian w git.

Jeśli faza A zakończyła się nierozwiązanym Odstępstwem lub blokadą, zgłoś to i zakończ — nie uruchamiaj fazy B dla częściowego wykonania.

## Faza B — Delegowanie przeglądu różnic

9. Deleguj do subagenta `reviewer` przez narzędzie `subagent`. Nie pomijaj tej fazy; własny przegląd poszczególnych kroków w fazie A jej nie zastępuje. `reviewer` nie ma Twojego kontekstu, więc wyślij samowystarczalny opis (podstaw właściwy identyfikator funkcji):

   > Tryb przeglądu różnic dla funkcji: $1
   >
   > Przeczytaj `specs/$1/REQ.md`, `specs/$1/PLAN.md` i `specs/$1/EXEC.md`. Następnie uruchom `git diff`, aby zobaczyć rzeczywiste zmiany drzewa roboczego.
   >
   > Sprawdź różnice względem planu i wymagań:
   > - Dla każdego AC z REQ.md istotnego dla tego repozytorium: spełnione | niespełnione | niemożliwe do zweryfikowania, z dowodami z różnic.
   > - Dla każdego kroku planu dotyczącego tego repozytorium: czy różnice odpowiadają Specyfikacji kroku (Pliki, Interfejsy, Ograniczenia)?
   > - Zgłoś każdy zmieniony plik niewymieniony w polu „Pliki” żadnego kroku jako rozszerzenie zakresu.
   > - Zgłoś każdy interfejs niezgodny z sekcją „Interfejsy” Specyfikacji.
   > - Sprawdź bezpieczeństwo: walidację wejścia, ryzyko wstrzyknięć, sekrety, niebezpieczne wartości domyślne w zmienionym kodzie.
   > - Uwaga: weryfikacja PHP w trybie docker (DEV_ENVIRONMENT=docker) nie może być uruchamiana przez `reviewer` — oznacz odpowiednie kroki PHP jako „niezweryfikowane przez `reviewer`; zweryfikować przez sidekick lub build”.
   >
   > Zwróć FORMAT RAPORTU.

10. Dopisz STATUS `reviewer` i jednowierszowe podsumowanie ustaleń pod „## Przegląd” w `specs/$1/EXEC.md`. Nie zaczynaj samodzielnie poprawiać ustaleń — użytkownik wybiera, które naprawić.

## Raport dla użytkownika

Przedstaw w tej kolejności:
- Podsumowanie wykonania dla tego repozytorium (pomiń w trybie `--review-only`).
- Raport `reviewer` dosłownie w FORMACIE RAPORTU (STATUS, USTALENIA, ZWERYFIKOWANO, LUKI).

Następnie zakończ. Powiedz użytkownikowi: wskaż, które ustalenia poprawić, a delegujesz poprawki do sidekick/design zgodnie ze zwykłą pętlą obsługi nietrafionych prób; uruchom `/exec $1 --review-only`, aby ponowić przegląd różnic po poprawkach.
