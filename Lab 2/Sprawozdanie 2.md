# Sprawozdanie lab 2
## Cel laboratoriów
Celem laboratoriów jest zapoznanie się z mergami konflikatmi i dwoma sposobami ich rozwiązaywania.

## 1. Aktualizacja repozytorium:
Na początku za pomocą poleceń:

```
git fetch --all
git checkout main
git pull
```

zaktualizowano repozytorium, aby mieć pewność, że lokalna wersja jest zsynchronizowana z najnowszymi zmianami w repozytorium zdalnym.

## 2. Nowe branche:

Utworzono branch z pierwszą wersją rozwiązania problemu:

```
git switch -c lab_2/new_branch_409947_v1
git push
```
Z drugą wersją rozwiązania problemu:

```
git switch -c lab_2/new_branch_409947_v2
git push
```
Oraz trzecią:

```
git switch -c lab_2/new_branch_409947_v3
git push
```

## 3. Edycja branchy:

Skopiowano zawartość folderu ```model_0000``` do folderu ```model_409947```.

Przełączono się na gałąź z pierwszą wersją rozwiązania:

```
git checkout lab_2/new_branch_409947_v1
```

Edytowano plik ```model.py```, zostawiając tylko pierwsze rozwiązanie.

```
import pickle 
import os 

###### Pierwsze rozwiązanie ###########

def run_model_v1(input):
    #Wczytywanie modelu z pliku
    path= os.path.dirname(__file__)
    
    with open(path+"/model.pkl","rb")as f:
        model= pickle.load(f)
```
Następnie edytowano plik ```app.py```, dodając tylko pierwsze rozwiązanie:

```
# W tej sekcji dodajemy nasz nowy model 
from flask import Flask, request, jsonify
from model_409947 import model as model_409947


###########################################


app = Flask(__name__)

##### Ta funkcja powinna być skopiowana do utworzenia nowego endpointa ####################
@app.route('/api/model_409947', methods=['POST'])
def model_409947_input():
    # Pobieranie treści zapytania w naszym przypadku array[4]
    data = request.get_json()
    input=data["input"]
    #Wykonywanie predykcji
    result=[]

    result.append(model_409947.run_model_v1(input=input)) #Wersja 1
    return jsonify({'result': result}), 200

#######################################################################



if __name__ == '__main__':
    app.run(host="0.0.0.0",debug=True,port=5000)
```
Oraz dodano nr indeksu do pliku ```config.py```:

```
id_list=[
409947
]
```
Dodano i spushowano zmiany:

```
git add *
git commit -m "zrobiono wersje 1"
git push
```

Analogicznie dla gałęzi v2 i v3 powtórzono wykonane kroki zostawiając jedynie drugie i trzecie rozwiązanie.

## 4. Mergowanie kodu z poziomu prostego brancha:

Wykonano merge request z wersji 2 do wersji 1:

```
git switch -c lab_2/new_branch_409947_v1
git merge lab_2/new_branch_409947_v2
```
W plikach ```model.py``` oraz ```app.py``` nastąpiły konflikty, co uniemożliwiło automatyczne połączenie gałęzi, konflikty zostały rozwiązane ręcznie.

## 5. Mergowanie kodu z wykorzystaniem brancha pomocniczego:

Utworzono branch pomocniczy:

```
git switch -c lab_2/new_branch_409947_merge_3_to_1
git push
```

Zmergowano go z branchem v3:

```
git merge lab_2/new_branch_409947_v3
```

Znów pojawiły się konflikty, które zostały rozwiązane tak jak w poprzednim kroku.

Następnie spushowano zmiany:

```
git add *
gi commit -m "zmergowano 3 z 1 "
git push
```
Oraz zmergowano i spushowano zmiany z brancha pomocniczego do brancha v1:

```
git checkout lab_2/new_branch_409947_v1
git merge lab_2/new_branch_409947_merge_3_to_1
git push 
```

Drzewko commitów:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%202/commity.png)

