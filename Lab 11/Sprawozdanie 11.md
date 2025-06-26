# Sprawozdanie lab 11
## Cel laboratoriów
Celem laboratoriów jest zapoznanie się z narzędziem Ansible.

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
git checkout -b Lab11/409947
```
Następnie utworzono nowy folder ```env_409947``` oraz skopiowano do niego pliki ```Dockerfile``` i ```compose.yml```:

```
mkdir env_409947

cp Dockerfile compose.yml env_409947/
```
## 2. Tworzenie mastera i workerów:

W folderze roboczym uruchomiono polecenie ```docker compose up -d```:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2011/ss1.png)

## 3. Klucze SSH:

Za pomocą ```docker ps``` sprawdzono adresy kontenerów, uruchomiono terminal na masterze oraz wygenerowano klucz SSH i skopiowano go do użycia w kolejnych etapach zadania:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2011/ss2.png)

Następnie dodano skopiowany klucz do obu workerów z mastera:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2011/ss3.png)

Zweryfikowano działanie klucza, logując się do obu workerów z mastera:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2011/ss4.png)

## 3. Ansible:

Na masterze zainstalowano Ansible, korzystając z poleceń:

```
apt install software-properties-common

add-apt-repository --yes --update ppa:ansible/ansible

apt install ansible
```
Zweryfikowano instalację:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2011/ss5.png)

Następnie zmieniono lokalizację na ```/etc/ansible```, edytowano plik ```hosts``` dodając do niego nazwy workerów:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2011/ss6.png)

Oraz utworzono plik ```playbook.yaml```:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2011/ss7.png)

Uruchomiono go:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2011/ss8.png)

Polecenia zostały wykonane poprawnie. 

Dodatkowo na workerze 1 zweryfikowano działanie ```ifconfig```:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2011/ss9.png)
