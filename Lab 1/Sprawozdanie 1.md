# Sprawozdanie lab 1
## Cel laboratoriów
Celem laboratoriów jest zapoznanie się z podstawowymi komendami git i używaniem gita do tworzenia gałęzi, tworzenia commitów pushowaniem zmian i tworzeniem pull requestów.

## 1. Konfiguracja klucza SSH:

Wygenerowano klucz SSH poleceniem:

```git ssh-keygen -t ed25519 -C "email"```

Następnie dodano go do GitHuba:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%201/ss1.png)

## 2. Pobieranie repo:

Pobrano repozytorium za pomocą komendy:

```
git clone git@github.com:Tomzonkal/dev_ops_lato_2025.git
```
## 3. Stworzenie gałęzi roboczej:

Utworzono nową gałąź roboczą za pomocą komendy:

```
git switch -c lab_1/new_branch_409947
```
Następnie zweryfikowano że pracujemy w nowo utworzonej gałęzi za pomocą polecenia:

```
git branch
```
## 4. Edycja kodu:

Za pomocą komendy:

```
cp -r model_0000/*  model_409947
```
Skopiowano zawartość folderu ```model_0000``` do folderu ```model_409947``` w katalogu LAB_1.

Uzupełniono plik ```config.py``` o mój numer indeksu:

```
id_list=[
    "0000",
    "123456",
    "409947"
]
```
Następnie w pliku ```config.py``` zmieniono nazwę funkcji ```run_model_0000``` na ```run_model_409947```:
```
import pickle 
import os 

def run_model_409947(input):
    #Wczytywanie modelu z pliku
    path= os.path.dirname(__file__)
    
    with open(path+"/model.pkl","rb")as f:
        model= pickle.load(f)
    #Wykonywanie predykcji    
    result=model.predict(input)
    result=float(result[0])
    return result
```
Oraz zaimportowano kod w pliku ```app.py```:

```
# W tej sekcji dodajemy nasz nowy model 
from flask import Flask, request, jsonify
from model_0000 import model as model_0000
from model_123456 import model as model_123456
from model_409947 import model as model_409947

###########################################


app = Flask(__name__)

##### Ta funkcja powinna być skopiowana do utowrzenia nowego endpointa####################3
@app.route('/api/model_0000', methods=['POST'])
def model_00000_input():
    # Pobieranie treści zapytania w naszym przypadku array[4]
    data = request.get_json()
    input=data["input"]
    #Wykonywanie predykcji
    result=model_0000.run_model_0000(input=input)
    return jsonify({'result': result}), 200

##### Ta funkcja powinna być skopiowana do utowrzenia nowego endpointa##########>
@app.route('/api/model_409947', methods=['POST'])
def model_409947_input():
    # Pobieranie treści zapytania w naszym przypadku array[4]
    data = request.get_json()
    input=data["input"]
    #Wykonywanie predykcji
    result=model_409947.run_model_409947(input=input)
    return jsonify({'result': result}), 200


#######################################################################

@app.route('/api/model_123456', methods=['POST'])
def model_123456_input():
    # Pobieranie treści zapytania w naszym przypadku array[4]
    data = request.get_json()
    input=data["input"]
    #Wykonywanie predykcji
    result=model_123456.run_model_123456(input=input)
    return jsonify({'result': result}), 200




if __name__ == '__main__':
    app.run(host="0.0.0.0",debug=True,port=5000)
```

## 5. Dodawanie zmian do repo:

Dodano wszystkie zmiany do commita za pomocą polecenia:

```
git add *
```

Skonfigurowano mail i nazwę:

```
git config --global user.email "your_email@example.com"

git config --global user.name "User_name"
```
Utworzono commita:

```
git commit -m "dodano model dla uzytkownika 409947"
```
A następnie spushowano go do repozytorium:

```
git push
```
## 6. Weryfikacja commita i tworzenie PR:

Zweryfikowano poprawność przesłanych zmian poprzez przejście na stronę repo, przełączenie na nowo utworzoną gałąź i sprawdzenie, czy zmiany są widoczne w przeglądarce:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%201/ss2.png)

Na koniec uruchomiono pull requesta i przeprowadzono testy:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%201/ss3.png)

Drzewko commitów:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%201/commity.png)

# Tematy dodatkowe:

### czym się różni git fetch od git pull

```git fetch``` pobiera zmiany ze zdalnego repozytorium, ale nie łączy ich automatycznie z lokalną gałęzią. ```git pull``` robi to samo, ale od razu scala pobrane zmiany z aktualną gałęzią.

### róznica między local repository a remote repository

Local repository to kopia repozytorium znajdująca się na komputerze, gdzie możemy robić zmiany, commity i eksperymentować.
Remote repository to wersja repozytorium umieszczona na serwerze, dostępna dla innych i służąca do współpracy oraz synchronizacji zmian.

### czym się różni gitflow od github flow

Gitflow to rozbudowany model z wieloma gałęziami, stosowany w większych projektach z długim cyklem wydań, natomiast GitHub Flow to prostszy, szybki proces pracy oparty na krótkotrwałych gałęziach i ciągłym wdrażaniu zmian.

### czemu używa się release branchy

Release branchy używa się, aby przygotować wersję produkcyjną oprogramowania — pozwalają one na dopracowanie, testowanie i poprawki bez przerywania pracy nad nowymi funkcjami w gałęzi develop. Dzięki temu można stabilnie wydać gotowy produkt, a równocześnie kontynuować rozwój projektu.
