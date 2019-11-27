---
title: Creación de un equilibrador de carga mediante API REST
titleSuffix: Azure Load Balancer
description: En este artículo aprenderá a crear una instancia de Azure Load Balancer mediante API REST.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: load-balancer
ms.date: 06/06/2018
ms.author: allensu
ms.openlocfilehash: b8acf1faff17f657999769216f71cfb5fa6e3181
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74077087"
---
# <a name="create-an-azure-basic-load-balancer-using-rest-api"></a>Creación de una instancia de Azure Basic Load Balancer mediante API REST

Una instancia de Azure Load Balancer distribuye los nuevos flujos de entrada que llegan a las instancias del grupo de servidores front-end a back-end del equilibrador de carga, según reglas y sondeos de estado. Load Balancer está disponible en dos SKU: Básico y Estándar. Para comprender la diferencia entre las dos versiones de SKU, consulte [Comparación de las SKU de Load Balancer](load-balancer-overview.md#skus).
 
Este procedimiento muestra cómo crear una instancia de Azure Load Balancer básico mediante [API REST de Azure](/rest/api/azure/) para ayudar a la solicitud entrante de equilibrio de carga entre varias máquinas virtuales dentro de una red virtual de Azure. La documentación de referencia completa y ejemplos adicionales están disponibles en la [referencia de REST de Azure Load Balancer](/rest/api/load-balancer/).
 
## <a name="build-the-request"></a>Compilar la solicitud
Utilice la siguiente solicitud PUT de HTTP para crear un nuevo Azure Load Balancer básico.
 ```HTTP
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}?api-version=2018-02-01
  ```
### <a name="uri-parameters"></a>Parámetros del identificador URI

|NOMBRE  |En  |Obligatorio |type |DESCRIPCIÓN |
|---------|---------|---------|---------|--------|
|subscriptionId   |  path       |  True       |   string      |  Las credenciales de suscripción que identifican de forma única la suscripción de Microsoft Azure. El identificador de suscripción forma parte del URI para cada llamada al servicio.      |
|resourceGroupName     |     path    | True        |  string       |   Nombre del grupo de recursos.     |
|loadBalancerName     |  path       |      True   |    string     |    El nombre del equilibrador de carga.    |
|api-version    |   query     |  True       |     string    |  Versión de API del cliente.      |



### <a name="request-body"></a>Cuerpo de la solicitud

El único parámetro necesario es `location`. Si no se define la versión *SKU*, se crea una instancia de Load Balancer básico de forma predeterminada.  Use [parámetros opcionales](https://docs.microsoft.com/rest/api/load-balancer/loadbalancers/createorupdate#request-body) para personalizar el equilibrador de carga.

| NOMBRE | type | DESCRIPCIÓN |
| :--- | :--- | :---------- |
| location | string | Ubicación del recurso Obtenga una lista actualizada de ubicaciones mediante la operación [Enumerar ubicaciones](https://docs.microsoft.com/rest/api/resources/subscriptions/listlocations) operación. |


## <a name="example-create-and-update-a-basic-load-balancer"></a>Ejemplo: Creación y actualización de una instancia de Load Balancer básico

En este ejemplo, primero hay que crear una instancia de Load Balancer básico junto con sus recursos. A continuación, configurará los recursos del equilibrador de carga que incluyen una configuración de IP de front-end, un grupo de direcciones de back-end, una regla de equilibrio de carga, un sondeo de estado y una regla NAT entrante.

Antes de crear un equilibrador de carga mediante el siguiente ejemplo, cree una red virtual denominada *vnetlb* con una subred con el nombre *subnetlb* en un grupo de recursos llamado *rg1* en la ubicación **East US**.

### <a name="step-1-create-a-basic-load-balancer"></a>PASO 1. Creación de una instancia de Load Balancer básico
En este paso, creará una instancia de Load Balancer básico denominada *lb* en la ubicación **EAST US** ubicación dentro del grupo de recursos *rg1*.
#### <a name="sample-request"></a>Solicitud de ejemplo

  ```HTTP    
  PUT https://management.azure.com/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb?api-version=2018-02-01
  ```
#### <a name="request-body"></a>Cuerpo de la solicitud

  ```JSON
   {
    "location": "eastus",
   }
  ```
### <a name="step-2-configure-load-balancer-resources"></a>PASO 2. Configuración de recursos del equilibrador de carga
En este paso, configurará los recursos del equilibrador de carga *lb* que incluyen una configuración de IP de front-end (*fe-lb*), un grupo de direcciones de back-end (*be-lb*), una regla de equilibrio de carga (*rulelb*), un sondeo de estado (*probe-lb*) y una regla NAT entrante (*in-nat-rule*).
#### <a name="sample-request"></a>Solicitud de ejemplo

  ```HTTP    
  PUT https://management.azure.com/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb?api-version=2018-02-01
  ```
#### <a name="request-body"></a>Cuerpo de la solicitud

  ```JSON
{
  "properties": {
    "frontendIPConfigurations": [
      {
        "name": "fe-lb",
        "properties": {
          "subnet": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/virtualNetworks/vnetlb/subnets/subnetlb"
          },
          "loadBalancingRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/loadBalancingRules/rulelb"
            }  ],
          "inboundNatRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/inboundNatRules/in-nat-rule"
            }  ]  }  }  ],
    "backendAddressPools": [
      {
        "name": "be-lb",
        "properties": {
          "loadBalancingRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/loadBalancingRules/rulelb"
            }  ]   }   }   ],
    "loadBalancingRules": [
      {
        "name": "rulelb",
        "properties": {
          "frontendIPConfiguration": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/frontendIPConfigurations/fe-lb"
          },
          "frontendPort": 80,
          "backendPort": 80,
          "enableFloatingIP": true,
          "idleTimeoutInMinutes": 15,
          "protocol": "Tcp",
          "loadDistribution": "Default",
          "backendAddressPool": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/backendAddressPools/be-lb"
          },
          "probe": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/probes/probe-lb"
          }  }  }  ],
    "probes": [
      {
        "name": "probe-lb",
        "properties": {
          "protocol": "Http",
          "port": 80,
          "requestPath": "healthcheck.aspx",
          "intervalInSeconds": 15,
          "numberOfProbes": 2,
          "loadBalancingRules": [
            {
              "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/loadBalancingRules/rulelb"
            }  ]  }  } ],
    "inboundNatRules": [
      {
        "name": "in-nat-rule",
        "properties": {
          "frontendIPConfiguration": {
            "id": "/subscriptions/subid/resourceGroups/rg1/providers/Microsoft.Network/loadBalancers/lb/frontendIPConfigurations/fe-lb"
          },
          "frontendPort": 3389,
          "backendPort": 3389,
          "enableFloatingIP": true,
          "idleTimeoutInMinutes": 15,
          "protocol": "Tcp"
        } } ],
    "inboundNatPools": [],
    "outboundNatRules": []
  }  }
```
