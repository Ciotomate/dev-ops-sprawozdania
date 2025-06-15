# Sprawozdanie lab 6
## Cel laboratoriów
Celem laboratoriów jest zapoznanie się z narzędziem docker compose.

## 1. Aktualizacja repozytorium:
Na początku za pomocą poleceń:

```
git fetch --all
git checkout main
git pull
```

Zaktualizowano repozytorium, aby mieć pewność, że lokalna wersja jest zsynchronizowana z najnowszymi zmianami w repozytorium zdalnym, 
a następnie przełączono się na branch roboczy za pomocą polecenia:

```
git checkout -b Lab6/409947
```
Następnie skopiowano zawartość folderu env_00000 do nowego folderu env_409947 za pomocą polecenia:

```
cp -r env_00000 env_409947
```

## 2. Uruchomienie Docker Compose bez Volumenu:

W tym kroku uruchomione zostało polecenie docker compose bez użycia wolumenu, następnie dodano plik w folderze home i usunięto kontener:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%206/ss1.png)

Następnie znowu uruchomiono docker compose i sprawdzono, czy plik dalej znajduje się w folderze home:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%206/ss2.png)

Plik nie istniał, co potwierdza, że bez volumnu dane nie są trwałe.

## 3. Uruchomienie Docker Compose z Volumenem:

W tym kroku edytowano plik compose.yml w następujący sposób:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%206/compose%20wolumen.png)

Nastepnie powtorzono kroki z punktu 2:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%206/ss3.png)

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%206/ss4.png)

Tym razem plik istniał, co potwierdza, że dane zostały zachowane dzięki volumnowi.

## 4. Sieć docker i eksport portów:

Zmodyfikowano plik compose.yml w następujący sposób, aby uruchomić obraz Postgres:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%206/compose%20baza.png)

Następnie uruchomiono compose:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%206/ss5.png)


Oraz zalogowano się do bazy danych za pomocą DBeavera i dodano tabelę:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%206/ss6.png)

Następnie utworzono kolejny kontener w oparciu o obraz Postgresa i dodano go do sieci wspólnej z poprzednim kontenerem,
po czym z kontenera db2 połączono się z bazą db za pomocą lokalnego IP i wyświetlono utworzoną tabelę:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%206/compose%202%20baza.png)

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%206/ss8.png)
