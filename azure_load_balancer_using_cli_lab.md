# Azure CLI Lab: Create a Load Balancer

This README documents the steps I followed to create an **Azure Load Balancer** using the Azure CLI. It includes resource creation, configuration, and verification commands.

---

## ✅ Prerequisites
- Azure CLI installed and logged in (`az login`)
- Existing Resource Group or create one:

```bash
az group create --name <ResourceGroupName> --location <Region>
```

---

## 🔹 Steps to Create a Load Balancer

### 1. Create a Virtual Network and Subnet
```bash
az network vnet create   --resource-group <ResourceGroupName>   --name MyVNet   --subnet-name MySubnet
```

### 2. Create a Public IP Address
```bash
az network public-ip create   --resource-group <ResourceGroupName>   --name MyPublicIP   --sku Standard   --allocation-method static
```

### 3. Create the Load Balancer
```bash
az network lb create   --resource-group <ResourceGroupName>   --name MyLoadBalancer   --sku Standard   --frontend-ip-name MyFrontEnd   --backend-pool-name MyBackEndPool   --public-ip-address MyPublicIP
```

### 4. Create a Health Probe
```bash
az network lb probe create   --resource-group <ResourceGroupName>   --lb-name MyLoadBalancer   --name MyHealthProbe   --protocol tcp   --port 80
```

### 5. Create a Load Balancer Rule
```bash
az network lb rule create   --resource-group <ResourceGroupName>   --lb-name MyLoadBalancer   --name MyHTTPRule   --protocol tcp   --frontend-port 80   --backend-port 80   --frontend-ip-name MyFrontEnd   --backend-pool-name MyBackEndPool   --probe-name MyHealthProbe
```

### 6. Create NICs and Attach to Backend Pool
```bash
az network nic create   --resource-group <ResourceGroupName>   --name MyNicVM1   --vnet-name MyVNet   --subnet MySubnet

az network nic create   --resource-group <ResourceGroupName>   --name MyNicVM2   --vnet-name MyVNet   --subnet MySubnet
```

Add NICs to backend pool:
```bash
az network lb address-pool address add   --resource-group <ResourceGroupName>   --lb-name MyLoadBalancer   --pool-name MyBackEndPool   --nic MyNicVM1

az network lb address-pool address add   --resource-group <ResourceGroupName>   --lb-name MyLoadBalancer   --pool-name MyBackEndPool   --nic MyNicVM2
```

### 7. Create VMs and Attach NICs
```bash
az vm create   --resource-group <ResourceGroupName>   --name MyVM1   --nics MyNicVM1   --image UbuntuLTS   --generate-ssh-keys

az vm create   --resource-group <ResourceGroupName>   --name MyVM2   --nics MyNicVM2   --image UbuntuLTS   --generate-ssh-keys
```

---

## ✅ Verification
- Get Public IP:
```bash
az network public-ip show --resource-group <ResourceGroupName> --name MyPublicIP --query ipAddress -o tsv
```
- Access the IP in a browser or use `curl` to confirm load balancing.

---

## 🗑 Cleanup
```bash
az group delete --name <ResourceGroupName> --yes --no-wait
```

---

### Notes
- Replace `<ResourceGroupName>` and `<Region>` with your values.
- Ensure NSGs allow inbound traffic on port 80.

