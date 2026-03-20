
# Azure Sandbox Terraform Deployment

This repository documents the full process of deploying Azure resources using **Terraform** in a **Pluralsight Azure Sandbox** environment.

## 📸 Screenshots

### Terraform Deployment Output
![Terraform Apply](Screenshot (514).png)

### Azure Portal – Resources Deployed
![Azure Portal](Screenshot (515).png)

## 🚀 Steps Followed

### 1. Login to Azure Sandbox using Device Code
```sh
az login --use-device-code
```
Sign in using your Pluralsight sandbox credentials.

### 2. Verify Subscription
```sh
az account show
az account list --output table
```
Set your sandbox subscription:
```sh
az account set --subscription "<subscription-id>"
```

### 3. Generate SSH RSA Key (required for Azure Linux VM)
Run inside your Terraform project folder:
```sh
ssh-keygen -t rsa -b 4096 -f sandbox_key
```
This produces:
```
sandbox_key
sandbox_key.pub
```

### 4. Terraform Initialize
```sh
terraform init
```

### 5. Terraform Plan
```sh
terraform plan -out tfplan
```

### 6. Terraform Apply
```sh
terraform apply tfplan
```

## 📂 Terraform Code Used (main.tf)

```hcl
terraform {
  required_version = ">= 1.4.0"

  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.100"
    }
  }
}

provider "azurerm" {
  features {}
}

locals {
  location           = "eastus"
  resource_group     = "1-26963041-playground-sandbox"
  vnet_name          = "vnet-simple"
  subnet_name        = "snet-simple"
  nsg_name           = "nsg-simple"
  public_ip_name     = "pip-simple"
  nic_name           = "nic-simple"
  vm_name            = "vm-simple"
  admin_username     = "azureuser"
  vm_size            = "Standard_B1s"
}

data "azurerm_resource_group" "rg" {
  name = local.resource_group
}

resource "azurerm_virtual_network" "vnet" {
  name                = local.vnet_name
  address_space       = ["10.0.0.0/16"]
  location            = data.azurerm_resource_group.rg.location
  resource_group_name = data.azurerm_resource_group.rg.name
}

resource "azurerm_subnet" "subnet" {
  name                 = local.subnet_name
  resource_group_name  = data.azurerm_resource_group.rg.name
  virtual_network_name = azurerm_virtual_network.vnet.name
  address_prefixes     = ["10.0.1.0/24"]
}

resource "azurerm_network_security_group" "nsg" {
  name                = local.nsg_name
  location            = data.azurerm_resource_group.rg.location
  resource_group_name = data.azurerm_resource_group.rg.name

  security_rule {
    name                       = "SSH"
    priority                   = 1001
    direction                  = "Inbound"
    access                     = "Allow"
    protocol                   = "Tcp"
    source_port_range          = "*"
    destination_port_range     = "22"
    source_address_prefix      = "*"
    destination_address_prefix = "*"
  }
}

resource "azurerm_public_ip" "pip" {
  name                = local.public_ip_name
  location            = data.azurerm_resource_group.rg.location
  resource_group_name = data.azurerm_resource_group.rg.name
  allocation_method   = "Static"
  sku                 = "Standard"
}

resource "azurerm_network_interface" "nic" {
  name                = local.nic_name
  location            = data.azurerm_resource_group.rg.location
  resource_group_name = data.azurerm_resource_group.rg.name

  ip_configuration {
    name                          = "ipconfig1"
    subnet_id                     = azurerm_subnet.subnet.id
    private_ip_address_allocation = "Dynamic"
    public_ip_address_id          = azurerm_public_ip.pip.id
  }
}

resource "azurerm_network_interface_security_group_association" "nic_nsg" {
  network_interface_id      = azurerm_network_interface.nic.id
  network_security_group_id = azurerm_network_security_group.nsg.id
}

variable "ssh_public_key_path" {
  type    = string
  default = "C:/Users/anvijayd/Documents/Terraform_prac/sandbox_key.pub"
}

resource "azurerm_linux_virtual_machine" "vm" {
  name                = local.vm_name
  resource_group_name = data.azurerm_resource_group.rg.name
  location            = data.azurerm_resource_group.rg.location
  size                = local.vm_size
  admin_username      = local.admin_username
  network_interface_ids = [azurerm_network_interface.nic.id]

  admin_ssh_key {
    username   = local.admin_username
    public_key = file(var.ssh_public_key_path)
  }

  disable_password_authentication = true

  os_disk {
    name                 = "${local.vm_name}-osdisk"
    caching              = "ReadWrite"
    storage_account_type = "Standard_LRS"
  }

  source_image_reference {
    publisher = "Canonical"
    offer     = "0001-com-ubuntu-server-jammy"
    sku       = "22_04-lts"
    version   = "latest"
  }

  computer_name = local.vm_name
}

output "public_ip" {
  value = azurerm_public_ip.pip.ip_address
}
```

---

## 🎉 Deployment Successful!
Your VM, networking stack, public IP, and NSG were deployed successfully in Azure.

Feel free to extend this with more resources such as load balancers, Windows VMs, VNets, or modules.
