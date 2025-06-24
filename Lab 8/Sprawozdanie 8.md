# Sprawozdanie lab 8
## Cel laboratoriów
Celem laboratoriów jest zapoznanie się z narzędziem git actions (obsługą sekretów), oraz z możliwością deployowania obrazów do zdalnego repo.

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
git checkout -b Lab8/409947
```
Następnie skopiowano zawartość folderu env_00000 do nowego folderu env_409947 za pomocą polecenia:

```
cp -r env_00000 env_409947
```

## 2. DockerHub:

Utworzono konto na Docker Hubie, następnie utworzono prywatne repozytorium na obrazy kontenerów oraz tokeny dostępu i skopiowano je. 
Następnie utworzono repozytorium na GitHubie i dodano do niego tokeny w GitHub Actions.

Kolejnym krokiem było lokalne zainicjowanie repozytorium:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%208/ss1.png)

Następnie dodano do niego pliki potrzebne do wykonania laboratorium:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%208/ss2.png)

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%208/ss3.png)

## 3. Edycja pliku ```yml```:

Utworzono folder ```.github/workflows``` i przeniesiono do niego plik ```pipeline.yml``` a następnie edytowano go w następujący sposób:

```
name: lab_8_test_409947

on:
  push:
    branches:
      - lab8/409947

jobs:
  unit_tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      - name: Install libs for testing
        run: |
          pip  install pytest 
      - name: Install dependencies
        run: |
          cd ./env_409947 && python -m pip install -r requirements.txt
      - name: Run unit tests
        run: |
          cd ./env_409947/main && pytest calculator_test.py

  functional_tests:
    needs: unit_tests
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      - name: Install libs for testing
        run: |
          pip  install pytest 
      - name: Install dependencies
        run: |
          cd ./env_409947 && python -m pip install -r requirements.txt
      - name: Run app
        run: |
          cd ./env_409947/main && nohup python app.py &
      - name: Sleep
        run: |
          sleep 10
      - name: Run functional tests
        run: |
          cd ./env_409947/main && pytest app_test.py

  deploy:
    needs: functional_tests
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_HUB_USERNAME_409947 }}
          password: ${{ secrets.DOCKER_HUB_TOKEN_409947 }}
      - name: Build and push
        uses: docker/build-push-action@v4
        with:
          context: ./env_409947
          file: ./env_409947/Dockerfile
          push: true
          tags: |
            ${{ secrets.DOCKER_HUB_USERNAME_409947 }}/lab8:latest
            ${{ secrets.DOCKER_HUB_USERNAME_409947 }}/lab8:${{ github.sha }}
```
Po spushowaniu zmian na repo kod przeszedł testy pomyślnie:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%208/ss4.png)

Na DockerHubie pojawił się również obraz:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%208/ss5.png)

## 4. Test obrazu:

Zalogowano się lokalnie na DockerHubie, a następnie pobrano i uruchomiono nasz obraz:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%208/ss6.png)

Lokalnie kod również przeszedł testy pomyślnie:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%208/ss7.png)

# Tematy dodatkowe

### Dlaczego istotne jest wykonywanie deploymentu po testach a nie przed

Deployment wykonuje się po testach, aby mieć pewność, że aplikacja działa poprawnie — zapobiega to wdrożeniu błędnego kodu i chroni środowisko produkcyjne.

### Czym szczególnym różniły się testy funkcjonalne od unit testów

Testy funkcjonalne sprawdzają, czy cały system działa zgodnie z wymaganiami użytkownika, testując funkcje jako całość, 
natomiast unit testy koncentrują się na pojedynczych, najmniejszych częściach kodu (np. funkcjach czy metodach) i ich poprawnym działaniu w izolacji.

### Dlaczego dobrą praktyką jest instalowanie requirementsów w oddzielnej komendzie RUN (w dockerfile)

Ponieważ dzięki temu warstwa z zależnościami może być buforowana niezależnie od zmian w kodzie aplikacji, 
co przyspiesza budowanie obrazu przy kolejnych zmianach.

### Dlaczego należy korzystać z przygotowanych magazynów haseł

Zapewniają one bezpieczne przechowywanie, zarządzanie i udostępnianie haseł oraz sekretów, 
minimalizując ryzyko ich wycieku i ułatwiając kontrolę dostępu.


