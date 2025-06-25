# Sprawozdanie lab 9
## Cel laboratoriów
Celem laboratoriów jest zapoznanie się z narzędziem Terraform, jako koncepcja IaC.

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
git checkout -b Lab9/409947
```
Następnie skopiowano zawartość folderu env_00000 do nowego folderu env_409947 za pomocą polecenia:

```
cp -r env_00000 env_409947
```
## 2. Tworzenie infrastruktury:

Utworzono konto na Azure, następnie pobrano Terraform, dodano go do zmiennej środowiskowej PATH oraz zainstalowano Azure CLI.
Następnie zalogowano się do Azure w bashu:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%209/ss1.png)

Kolejnym krokiem była edycja pliku ```main.tf```.

Tworzenie resource group:

```
resource "azurerm_resource_group" "rg" {
    name     = "rg-env-409947"
    location = "West Europe"
}
```

Tworzenie sieci i podsieci:

```
resource "azurerm_virtual_network" "vnet" {
    name                = "vnet-409947"
    address_space       = ["10.0.0.0/16"]
    location            = azurerm_resource_group.rg.location
    resource_group_name = azurerm_resource_group.rg.name
}

resource "azurerm_subnet" "subnet" {
    name                 = "subnet-409947"
    resource_group_name  = azurerm_resource_group.rg.name
    virtual_network_name = azurerm_virtual_network.vnet.name
    address_prefixes     = ["10.0.1.0/24"]
}
```

Tworzenie VM z ubuntu:

```
resource "azurerm_public_ip" "public_ip" {
    count               = 2
    name                = "vm-ip-${count.index}"
    location            = azurerm_resource_group.rg.location
    resource_group_name = azurerm_resource_group.rg.name
    sku                 = "Basic"
    allocation_method   = "Dynamic"
}

resource "azurerm_network_interface" "nic" {
    count               = 2
    name                = "vm-nic-${count.index}"
    location            = azurerm_resource_group.rg.location
    resource_group_name = azurerm_resource_group.rg.name

    ip_configuration {
        name                          = "internal"
        subnet_id                     = azurerm_subnet.subnet.id
        private_ip_address_allocation = "Dynamic"
        public_ip_address_id          = azurerm_public_ip.public_ip[count.index].id
    }
}

resource "azurerm_linux_virtual_machine" "vm" {
    count               = 2
    name                = "vm-409947-${count.index}"
    resource_group_name = azurerm_resource_group.rg.name
    location            = azurerm_resource_group.rg.location
    size                = "Standard_B2s"
    admin_username      = "azureuser"
    admin_ssh_key {
        username = "azureuser"
        public_key = file("~/.ssh/id_rsa.pub")
    }
    network_interface_ids = [
        azurerm_network_interface.nic[count.index].id
    ]

    os_disk {
        caching              = "ReadWrite"
        storage_account_type = "Standard_LRS"
    }

    source_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "18.04-LTS"
        version   = "latest"
    }

}
```

Tworzenie NSG:

```
resource "azurerm_network_security_group" "nsg" {
    name                = "nsg-409947"
    location            = azurerm_resource_group.rg.location
    resource_group_name = azurerm_resource_group.rg.name

    security_rule {
        name                       = "Allow-SSH-From-MyIP"
        priority                   = 100
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_range     = "22"
        source_address_prefix      = "moje_ip/32"
        destination_address_prefix = "*"
    }

    security_rule {
        name                       = "Allow-Internal-ICMP"
        priority                   = 150
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Icmp"
        source_port_range          = "*"
        destination_port_range     = "*"
        source_address_prefix      = "10.0.1.0/24"
        destination_address_prefix = "*"
  }

    security_rule {
        name                       = "Deny-All-Inbound"
        priority                   = 200
        direction                  = "Inbound"
        access                     = "Deny"
        protocol                   = "*"
        source_port_range          = "*"
        destination_port_range     = "*"
        source_address_prefix      = "*"
        destination_address_prefix = "*"
    }
}

resource "azurerm_subnet_network_security_group_association" "nsg_assoc" {
  subnet_id                 = azurerm_subnet.subnet.id
  network_security_group_id = azurerm_network_security_group.nsg.id
}
```

Dodano również output, który pokazywał publiczne adresy IP utworzonych maszyn:

```
output "vm_public_ips" {
  description = "Lista publicznych IP dla wszystkich VM"
  value       = [for ip in azurerm_public_ip.public_ip : ip.ip_address]
}
```
Następnie wykonano polecenia ```terraform init```, ```terraform plan``` oraz ```terraform apply``` w celu utworzenia maszyn:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%209/ss2.png)
![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%209/ss3.png)

## 3. Komunikacja między maszynami:

Za pomocą polecenia ```terraform refresh``` odświeżono stan infrastruktury i uzyskano aktualne publiczne adresy IP utworzonych maszyn wirtualnych, 
następnie połączono się z nimi poleceniem ```ssh azureuser@adres_IP``` oraz czterokrotnie wysłano ping z maszyny 1 do maszyny 0, 
wykorzystując jej prywatny adres IP poleceniem ```ping -c 4 prywatne_IP_maszyny```:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%209/ss4.png)
![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%209/ss5.png)

Po poprawnym wykonaniu zadania usunięto grupę zasobów:

![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%209/ss6.png)
![](https://github.com/Ciotomate/dev-ops-sprawozdania/blob/main/Lab%209/ss7.png)

