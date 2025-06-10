# Sprawozdanie lab 4
## Cel laboratoriów
Celem laboratoriów jest zapoznanie się z narzędziem docker jako możliwość tworzenia własnych obszarów roboczych i ich automatyzacji.
## 1. Aktualizacja repozytorium:
Na początku za pomocą poleceń:

```
git fetch --all
git checkout main
git pull
```

zaktualizowano repozytorium, aby mieć pewność, że lokalna wersja jest zsynchronizowana z najnowszymi zmianami w repozytorium zdalnym.

## 2. Tworzenie Kontenera za Pomocą Terminala:
Pobieranie obrazu Ubuntu, weryfikacja za pomocą images, tworzenie kontenera oraz weryfikacja działającego kontenera za pomocą komendy ```docker ps```.

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%204/ss1.png)

Podłączanie się terminalem do kontenera:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%204/ss2.png)

## 3. Ręczna konfiguracja kontenera:

Instalacja brakujących pakietów:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%204/ss3.png)

Tworzenie ```main.py``` oraz uruchomienie go:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%204/ss4.png)

Instalacja Pandas za pomocą pip'a kończyła się błędem, więc użyłem komendy ```apt install python3-pandas ```

Ponowne uruchomienie ```main.py```, wyjście, zatrzymanie oraz usunięcie kontenera:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%204/ss5.png)

## 4. Automatyzacja konfigurowania środowiska:

Tworzenie folderu env_409947 za pomocą komendy ``` touch env_409947 ```, skopiowanie i uzupełnienie pliku ```Dockerfile``` o komendy:

```
apt-get update 
apt-get install -y net-tools nano 
pip install --no-cache-dir pandas
```

Następnie budowanie obrazu, uruchamianie kontenera oraz weryfikacja działania:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%204/ss6.png)

Podłączanie do kontenera za pomocą terminala oraz utworzenie i uruchomienie skryptu ```main.py```:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%204/ss7.png)

Skrypt działa bez dodatkowej instalacji pakietów dzięki zautomatyzowaniu tego procesu w ```Dockerfile```.

# Tematy dodatkowe

### Co oznaczają flagi ```-t``` i ```-d``` w komendzie docker run i dlaczego zostały użyte:

Flaga `-t` przydziela kontenerowi terminal, a `-d` uruchamia go w tle. Używane razem pozwalają na działanie kontenera w tle z możliwością późniejszego połączenia się z terminalem (np. przez `docker exec`).

### Czym różni się komenda ```RUN``` w dockerfile od komendy ```CMD```, Wymień 2 inne najczęściej pojawiające się komendy:

**`RUN`** – wykonuje polecenie podczas budowania obrazu (np. instalacja pakietów).

**`CMD`** – określa domyślne polecenie uruchamiane przy starcie kontenera.

**Dwie inne często używane komendy w Dockerfile:**

**`FROM`** – określa bazowy obraz (np. `FROM ubuntu`).

**`COPY`** – kopiuje pliki z systemu hosta do obrazu.

### Jak kopiować pliki z komputera do działającego kontenera i na odwrót:

Z komputera do kontenera:

```docker cp ścieżka_na_komputerze kontener_id:/ścieżka_w_kontenerze```

Z kontenera do komputera:

```docker cp kontener_id:/ścieżka_w_kontenerze ścieżka_na_komputerze```









