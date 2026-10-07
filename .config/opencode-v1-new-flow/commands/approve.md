---
description: "Bramka akceptacji: przyjmij decyzję po przeglądzie planu i ustaw Status: APPROVED. Użycie: /approve <slug>"
agent: plan
---
Identyfikator funkcji: $1

Użytkownik zdecydował, które uwagi subagenta `reviewer` zaakceptować, a które odrzucić. Deleguj do subagenta `sidekick` przez narzędzie `subagent` poniższe zadania:

1. Przeczytaj plik `specs/$1/PLAN.md`. Jeśli status to już `APPROVED`, przerwij działanie i poinformuj o tym.
2. Zmień nagłówek ze `Status: DRAFT` na `Status: APPROVED`. Nie wprowadzaj żadnych innych zmian w planie.
3. Zwróć APPROVED

Potwierdź użytkownikowi: „Plan zatwierdzony. Uruchom `/exec $1`, aby rozpocząć realizację”.