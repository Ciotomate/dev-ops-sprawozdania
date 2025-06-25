# Sprawozdanie lab 10
## Cel laboratoriów
Celem laboratoriów jest zapoznanie się z platformą K8s do wdrażania gotowych aplikacji z Docker Registry.

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
git checkout -b Lab10/409947
```
Następnie skopiowano zawartość folderu env_00000 do nowego folderu env_409947 za pomocą polecenia:

```
cp -r env_00000 env_409947
```

## 2. Tworzenie infrastruktury:

Zalogowano do Azure CLI za pomocą polecenia ```az login```.
W folderze infra uzupełniono plik ```provider.tf``` o ID subskrypcji, a następnie zbudowano infrastrukturę w oparciu o Terraform za pomocą poleceń:

```
terraform init
terraform plan
terraform apply
```
Następnie zbudowano obraz Dockera, sprawdzono nazwę Azure Container Registry oraz zalogowano się do niego:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2010/ss1fix.png)

Kolejnym krokiem było stworzenie oraz wypchnięcie obrazu do repozytorium Azure:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2010/ss2.png)

## 3. Klaster K8s:

Uzyskano poświadczenie klastra oraz sprawdzono działające węzły:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2010/ss3.png)

Utworzono secret:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2010/ss4.png)

A następnie uzupełniono pliki w folderze k8s w następujący sposób:

```deployment.yml```:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: simple-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: simple-app
  template:
    metadata:
      labels:
        app: simple-app
    spec:
      containers:
      - name:  lab10-app
        image:  lab10acr86mcdq.azurecr.io/lab10_app
        ports:
        - containerPort: 5000
      imagePullSecrets:
      - name:  acr-secret
```

```service.yml```:

```
apiVersion: v1
kind: Service
metadata:
  name: simple-app-service
spec:
  selector:
    app: simple-app
  ports:
    - protocol: TCP
      port:  5000
      targetPort:  5000
  type: ClusterIP
```
Po edycji plików wdrożono aplikację:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2010/ss5.png)

Sprawdzono nazwy usług, następnie zastosowano port forwarding:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2010/ss6.png)

## 4. Testy funkcjonalne:

W osobnym oknie terminala uruchomiono testy:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%2010/ss7.png)

Wszystkie testy zostały zakończone pomyślnie.

# Tematy dodatkowe

### Czym różni się silnik containerd od Dockera (gdzie i jaki się stosuje)

Silnik containerd to lekki, niskopoziomowy runtime kontenerów, który odpowiada za uruchamianie i zarządzanie kontenerami, natomiast Docker to pełen ekosystem narzędzi do tworzenia, 
budowania, zarządzania i uruchamiania kontenerów, który korzysta właśnie z containerd jako swojej części wykonawczej. Docker oferuje wygodne CLI, obsługę budowania obrazów, sieci i wolumenów, 
co czyni go idealnym narzędziem dla deweloperów i środowisk developerskich. Z kolei containerd, pozbawiony warstwy użytkownika i funkcji budowania, 
jest używany przede wszystkim jako runtime kontenerów w produkcyjnych klastrach Kubernetes (np. AKS, GKE, EKS), gdzie liczy się lekkość, wydajność i ścisła integracja z Kubernetesowym interfejsem CRI. 
W efekcie Docker jest narzędziem kompletnym i wygodnym do pracy lokalnej, 
a containerd to bardziej specjalistyczny silnik skupiony na uruchamianiu kontenerów w środowiskach produkcyjnych, zwłaszcza w Kubernetesie.

### Podaj inne możliwe sposoby zapewnienia połączenia z aplikacją niż klasyczny ClusterIP i czym się różnią między sobą

Poza klasycznym typem usługi ClusterIP, który udostępnia aplikację tylko wewnątrz klastra Kubernetes, można zastosować inne sposoby połączenia: NodePort wystawia usługę na stałym porcie na każdym węźle, umożliwiając dostęp spoza klastra przez adres IP węzła i ten port, choć wymaga otwarcia portów i jest mniej elastyczny; LoadBalancer tworzy zewnętrzny, publiczny adres IP wraz z load balancerem zarządzanym przez dostawcę chmury, co ułatwia produkcyjne wystawienie aplikacji; **Ingress** to warstwa wyższego poziomu, która pozwala na zaawansowany routing HTTP/HTTPS, obsługę SSL i zarządzanie wieloma usługami pod jednym adresem IP, wymagając instalacji kontrolera Ingress; natomiast **ExternalName** pozwala na mapowanie usługi Kubernetes na zewnętrzną nazwę DNS poza klastrem, bez tworzenia własnych endpointów. Każda z tych metod różni się zakresem ekspozycji, poziomem kontroli i sposobem dostępu do aplikacji.
