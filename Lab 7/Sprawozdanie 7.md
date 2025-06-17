# Sprawozdanie lab 7
## Cel laboratoriów
Celem laboratoriów jest zapoznanie się z narzędziem git actions i zbudowanie własnego flow.

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
git checkout -b Lab7/409947
```
Następnie skopiowano zawartość folderu env_00000 do nowego folderu env_409947 za pomocą polecenia:

```
cp -r env_00000 env_409947
```

## 2. Edycja skryptu yml:

Stworzono folder ```.github/workflows``` oraz skopiowano do niego plik ```pipeline.yml```, nastepnie edytowano go:

```
name: lab_7_409947_test

on:
  push:
    branches:
      - Lab7/409947
    
jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: '3.11'

    - name: Install libs for testing
      run: |
        pip  install pytest 
    
    - name: prepare environemnt 
      run: |
         cd ./Lab_7/env_0000 && python -m pip install -r requirements.txt
      

    - name: Run test
      run: |
        cd ./Lab_7/env_0000 && pytest calculator_test.py
```
Następnie scommitowano plik:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%207/ss3.png)

Zainstalowano bibliotekę pandas oraz wykonano testy:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%207/ss4.png)
![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%207/ss5.png)

## 3. Przerabianie kodu:

Stworzono plik ```app.py``` z nowymi funkcjami:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%207/app.png)

Oraz pliki z testami nowych funkcji:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%207/max.png)

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%207/min.png)

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%207/mean.png)

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%207/sum.png)

I plik odpalający wszystkie testy razem:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%207/new.png)

Następnie odpalono wszystkie testy:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%207/ss7.png)

# Tematy dodatkowe

### Po co są używane stage

Stage są używane w pipeline'ach do logicznego podziału procesu (np. build, test, deploy), co ułatwia organizację,
kontrolę kolejności kroków, umożliwia równoległe wykonywanie zadań i warunkowe uruchamianie poszczególnych etapów.

### Jak można zmniejszyć liczbę własnoręcznie pisanych kroków w jobie (opisać przykładową procedurę z marketu np do tworzenia środowiska pod dockera)

Aby uprościć konfigurację joba, można korzystać z gotowych akcji z Marketplace, np. docker/setup-buildx-action, 
które zastępują ręczne kroki i przyspieszają tworzenie środowiska Dockerowego.

### Co to są artefakty w pipelinie i do czego służą

Artefakty w pipelinie to pliki lub katalogi wygenerowane podczas działania jobów, 
które są zachowywane i udostępniane na późniejszych etapach lub do pobrania po zakończeniu pipeline’u.

### Jakie są inne narzędzia do wykonywania pipelinów testowych (podać co najmniej 3 z czego min 1 ma być open source)

GitHub Actions – zintegrowane z GitHub, popularne w repozytoriach open source.
GitLab CI/CD – open source, działa w GitLab, konfiguracja przez ```.gitlab-ci.yml```.
Jenkins – open source, elastyczne i rozbudowane, działa lokalnie lub w chmurze.

### Do czego w pipelinie może służyć sonar qube

SonarQube w pipeline służy do automatycznej analizy jakości kodu — wykrywa błędy, luki bezpieczeństwa, 
duplikaty oraz problemy z czytelnością i zgodnością ze standardami, pomagając utrzymać wysoką jakość i bezpieczeństwo projektu.

