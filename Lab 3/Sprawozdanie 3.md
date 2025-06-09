
# Sprawozdanie lab 3
## Cel laboratoriów
Celem laboratoriów bylo zapoznanie się z code review i podstawowymi plikami budującymi repo i przywracaniem commitów.
## 1. Aktualizacja repozytorium

Na początku za pomocą poleceń:

```
git fetch --all
git checkout main
git pull
```

zaktualizowano repozytorium, aby mieć pewność, że lokalna wersja jest zsynchronizowana z najnowszymi zmianami w repozytorium zdalnym.
## 2. Tworzenie nowego brancha:
Utworzono nowy branch o nazwie lab_3/new_branch_409947 i spushowano go na zdalne repozytorium za pomocą poleceń:

```
git switch -c lab_3/new_branch_409947
git push --set-upstream origin lab_3/new_branch_409947
```

## 3. Edytowanie brancha:
Utworzono folder model_409947, dodano plik 409947.csv, a następnie wykonano commit i spushowano zmiany.

Zrzut ekranu:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%203/ss2.png)

## 4. Code review:
Utworzono Pull Request do brancha TEST, wykonano code review i dodano komentarze:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%203/ss3.png)

## 5. Poprawka kodu:
Usunięto plik CSV, dodano regułę do .gitignore, utworzono nowy plik CSV (który nie będzie śledzony) oraz dodano .gitkeep:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%203/ss5.png)

## 6. Naprawa repo:
Dodano plik test_plik.txt oraz edytowano go:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%203/ss6.png)

Następnie cofnięto zmiany za pomocą git revert:
```
git revert hash_commita
```
# Tematy dodatkowe
### Czym różnią się git hooksy client-side od serve-side, jakie to są i kiedy się je wykorzystuje:

Git hooksy (zaczepy) to skrypty uruchamiane automatycznie przez Gita w odpowiedzi na konkretne akcje. Dzielą się na dwa rodzaje:

Client-side hooks (po stronie klienta):

* Działają lokalnie na komputerze użytkownika.
* Uruchamiane podczas operacji takich jak commit (pre-commit, commit-msg), merge, rebase, czy checkout.
* Wykorzystywane do sprawdzania stylu kodu (lint), uruchamiania testów przed commitem, walidacji wiadomości commitów itp.

Server-side hooks (po stronie serwera):

* Działają na serwerze Git (np. GitLab, GitHub Enterprise).
* Uruchamiane przy pushu (pre-receive, update, post-receive).
* Służą do kontroli dostępu, wymuszania reguł (np. zakaz pushowania do main), czy wywoływania powiadomień i CI/CD.

Różnice:
* Client-side działa lokalnie i służy głównie do sprawdzania jakości kodu.
* Server-side działa na serwerze i kontroluje to, co trafia do repozytorium.

### Czym różni się git reset od git revert:
* git reset — cofa zmiany lokalnie, przesuwając wskaźnik HEAD do wcześniejszego commita. Może usunąć commity z historii (szczególnie w trybie --hard), więc zmienia historię repozytorium. Używa się go, gdy chcesz trwale cofnąć ostatnie commity i ewentualnie poprawić historię (uwaga: niebezpieczne przy współpracy).
* git revert — tworzy nowy commit, który odwraca zmiany z wybranego commita, nie zmieniając historii. Jest bezpieczny przy pracy zespołowej, bo historia pozostaje nienaruszona, a cofnięcie jest odwracalne.

### Jaką role pełnią mocki w TDD:

Mocki w TDD (Test-Driven Development) pełnią rolę sztucznych zamienników prawdziwych obiektów lub zależności, które są wykorzystywane do izolowania testowanego kodu.

Dzięki mockom można:
* symulować zachowanie zewnętrznych usług, baz danych, API itp.,
* kontrolować odpowiedzi i zachowanie zależności,
* testować tylko logikę konkretnej jednostki kodu bez wpływu innych komponentów,
* szybciej wykonywać testy, bo nie odwołujesz się do rzeczywistych, często wolnych zasobów.
