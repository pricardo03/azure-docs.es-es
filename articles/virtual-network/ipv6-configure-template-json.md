---
title: 'Implementar una aplicación de pila doble de IPv6 en Azure virtual network: plantilla de Resource Manager (versión preliminar)'
titlesuffix: Azure Virtual Network
description: En este artículo se muestra cómo implementa una aplicación de pila doble de IPv6 en Azure virtual network mediante plantillas de VM de Azure Resource Manager.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.workload: infrastructure-services
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: ae90bc4a12763803f38224d917c4644a68ae7d6b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62131034"
---
# <a name="deploy-an-ipv6-dual-stack-application-in-azure---template-preview"></a>Implementar una aplicación de pila doble de IPv6 en Azure: plantilla (versión preliminar)

En este artículo se proporciona una lista de tareas de configuración de IPv6 con la parte de la plantilla de VM de Azure Resource Manager que se aplica. Usar la plantilla que se describe en este artículo para implementar una aplicación de doble pila (IPv4 + IPv6) de Azure que incluye una pila dual red virtual con subredes IPv4 e IPv6, un equilibrador de carga con configuraciones de front-end dual (IPv4 + IPv6), las máquinas virtuales con NIC que tienen una dirección IP dual configuración de grupo de seguridad de red y direcciones IP públicas. 

## <a name="required-configurations"></a>Configuraciones necesarias

Busque las secciones de plantilla en la plantilla para ver dónde se deben producir.

### <a name="ipv6-addressspace-for-the-virtual-network"></a>AddressSpace de IPv6 para la red virtual

Sección de la plantilla para agregar:

```JSON
        "addressSpace": {
          "addressPrefixes": [
            "[variables('vnetv4AddressRange')]",
            "[variables('vnetv6AddressRange')]"    
```

### <a name="ipv6-subnet-within-the-ipv6-virtual-network-addressspace"></a>Subred de IPv6 en el addressSpace de la red virtual IPv6

Sección de la plantilla para agregar:
```JSON
          {
            "name": "V6Subnet",
            "properties": {
              "addressPrefix": "[variables('subnetv6AddressRange')]"
            }

```

### <a name="ipv6-configuration-for-the-nic"></a>Configuración de IPv6 para la NIC

Sección de la plantilla para agregar:
```JSON
          {
            "name": "ipconfig-v6",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
          "privateIPAddressVersion":"IPv6",
              "subnet": {
                "id": "[variables('v6-subnet-id')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(resourceId('Microsoft.Network/loadBalancers','loadBalancer'),'/backendAddressPools/LBBAP-v6')]"
                }
```

### <a name="ipv6-network-security-group-nsg-rules"></a>Reglas de grupo (NSG) de seguridad de red de IPv6

```JSON
          {
            "name": "default-allow-rdp",
            "properties": {
              "description": "Allow RDP",
              "protocol": "Tcp",
              "sourcePortRange": "33819-33829",
              "destinationPortRange": "5000-6000",
              "sourceAddressPrefix": "ace:cab:deca:deed::/64",
              "destinationAddressPrefix": "cab:ace:deca:deed::/64",
              "access": "Allow",
              "priority": 1003,
              "direction": "Inbound"
            }
```

## <a name="conditional-configuration"></a>Configuración condicional

Si usa un dispositivo de red virtual, agregue rutas IPv6 en la tabla de rutas. En caso contrario, esta configuración es opcional.

```JSON
    {
      "type": "Microsoft.Network/routeTables",
      "name": "v6route",
      "apiVersion": "[variables('ApiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "routes": [
          {
            "name": "v6route",
            "properties": {
              "addressPrefix": "ace:cab:deca:deed::/64",
              "nextHopType": "VirtualAppliance",
              "nextHopIpAddress": "deca:cab:ace:f00d::1"
            }
```

## <a name="optional-configuration"></a>Configuración opcional

### <a name="ipv6-internet-access-for-the-virtual-network"></a>Acceso a Internet de IPv6 para la red virtual

```JSON
{
            "name": "LBFE-v6",
            "properties": {
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses','lbpublicip-v6')]"
              }
```

### <a name="ipv6-public-ip-addresses"></a>Direcciones IP públicas IPv6

```JSON
    {
      "apiVersion": "[variables('ApiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "lbpublicip-v6",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "publicIPAddressVersion": "IPv6"
      }
```

### <a name="ipv6-front-end-for-load-balancer"></a>IPv6 front-end de equilibrador de carga

```JSON
          {
            "name": "LBFE-v6",
            "properties": {
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses','lbpublicip-v6')]"
              }
```

### <a name="ipv6-back-end-address-pool-for-load-balancer"></a>Grupo de direcciones de Back-end de IPv6 para el equilibrador de carga

```JSON
              "backendAddressPool": {
                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', 'loadBalancer'), '/backendAddressPools/LBBAP-v6')]"
              },
              "protocol": "Tcp",
              "frontendPort": 8080,
              "backendPort": 8080
            },
            "name": "lbrule-v6"
```

### <a name="ipv6-load-balancer-rules-to-associate-incoming-and-outgoing-ports"></a>Asociar los puertos entrantes y salientes de las reglas del equilibrador de carga de IPv6

```JSON
          {
            "name": "ipconfig-v6",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
          "privateIPAddressVersion":"IPv6",
              "subnet": {
                "id": "[variables('v6-subnet-id')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(resourceId('Microsoft.Network/loadBalancers','loadBalancer'),'/backendAddressPools/LBBAP-v6')]"
                }
```

## <a name="sample-vm-template-json"></a>Ejemplo de JSON de la plantilla de máquina virtual
Haga clic en [aquí](https://azure.microsoft.com/resources/templates/ipv6-in-vnet/) para implementar una aplicación de pila doble de IPv6 en Azure virtual network mediante la plantilla de Azure Resource Manager.

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar detalles sobre los precios de [direcciones IP públicas](https://azure.microsoft.com/pricing/details/ip-addresses/), [ancho de banda de red](https://azure.microsoft.com/pricing/details/bandwidth/), o [equilibrador de carga](https://azure.microsoft.com/pricing/details/load-balancer/).
