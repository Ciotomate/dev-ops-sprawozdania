# Sprawozdanie lab 5
## Cel laboratoriów
Celem laboratoriów jest zapoznanie się z narzędziem docker compose.

## 1. Aktualizacja repozytorium:
Na początku za pomocą poleceń:

```
git fetch --all
git checkout main
git pull
```

zaktualizowano repozytorium, aby mieć pewność, że lokalna wersja jest zsynchronizowana z najnowszymi zmianami w repozytorium zdalnym, 
a następnie przełączono się na branch roboczy za pomocą polecenia:

```
git checkout -b Lab5/409947
```

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%205/ss1.png)

## 2. Tworzenie folderu i uruchamianie docker compose:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%205/ss2.png)

Sprawdzenie czy link http://localhost:8000/ funkcjonuje poprawnie:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%205/ss3.png)

Nowo powstałe kontenery:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%205/ss4.png)

## 3. Docker compose ze strażnikiem:

Docker Compose został zmodyfikowany w następujący sposób:

```
services:
  web:
    build: .
    ports:
      - "8000:5000"
    develop:
      watch:
        - action: sync
          path: .
          target: /code
  redis:
    image: "redis:alpine"
```
Nastepnie uruchomiony z ```docker compose watch```, a w pliku ```app.py``` został zmieniony tekst:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%205/ss5.png)

Odświeżona strona:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%205/ss6.png)

Po odświeżeniu strony treść uległa zmianie, ponieważ mechanizm *watch* automatycznie synchronizuje zmiany w kodzie. Licznik pozostał bez zmian, ponieważ jego stan jest przechowywany w Redisie.

## 4. Separacja kodu od deploymentu:

Struktura katalogów w folderze roboczym została zmieniona na:

```
├── env_409947
    |── main
    |    |── requirements.txt
    |    └── app.py 
    |── Docker
    |    └── Dockerfile
    └── compose.yml
```
A pliki ```dockerfile``` oraz ```compose.yml``` zostały odpowiednio edytowane, żeby polecenie ```docker compose up``` działało z poziomu folderu env_409947.

Edytowany ```dockerfile```:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%205/ss7.png)

Edytowany ```compose.yml```:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%205/ss8.png)

Potwierdzenie działania:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%205/ss9.png)
