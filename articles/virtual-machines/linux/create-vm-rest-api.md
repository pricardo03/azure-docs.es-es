---
title: Creación de una máquina virtual Linux con la API REST de Azure | Microsoft Docs
description: Aprenda a crear una máquina virtual Linux e Azure que use Managed Disks y autenticación de SSH con la API REST de Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2018
ms.author: cynthn
ms.openlocfilehash: 11d9f5efb452d46e5ca30169861582f6f2bbbd1b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46969400"
---
# <a name="create-a-linux-virtual-machine-that-uses-ssh-authentication-with-the-rest-api"></a>Creación de una máquina virtual Linux que usa la autenticación de SSH con la API REST

Una máquina virtual (VM) en Azure se define mediante diversos parámetros, como ubicación, tamaño de hardware, imagen del sistema operativo y credenciales de inicio de sesión. En este artículo se muestra cómo usar la API REST para crear una máquina virtual Linux que usa la autenticación de SSH.

Para crear o actualizar una máquina virtual, use la siguiente operación *PUT*:

``` http
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2017-12-01
```

## <a name="create-a-request"></a>Creación de una solicitud

Para crear la solicitud *PUT*, se necesita el parámetro `{subscription-id}`. Si tiene varias suscripciones, consulte [Trabajo con varias suscripciones](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions). Definirá los elementos `{resourceGroupName}` y `{vmName}` para sus recursos, junto con el parámetro `api-version`. En este artículo se usa `api-version=2017-12-01`.

Los siguientes encabezados son obligatorios:

| Encabezado de solicitud   | DESCRIPCIÓN |
|------------------|-----------------|
| *Content-Type:*  | Necesario. Establézcalo en `application/json`. |
| *Authorization:* | Necesario. Establézcalo en un [token de acceso](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients) `Bearer` válido. |

Para más información sobre cómo crear la solicitud, consulte [ Componentes de una solicitud/respuesta de la API REST](/rest/api/azure/#components-of-a-rest-api-requestresponse).

## <a name="create-the-request-body"></a>Creación del cuerpo de la solicitud

Para crear el cuerpo de la solicitud, se usan las siguientes definiciones comunes:

| NOMBRE                       | Obligatorio | Escriba                                                                                | DESCRIPCIÓN  |
|----------------------------|----------|-------------------------------------------------------------------------------------|--------------|
| location                   | True     | string                                                                              | Ubicación del recurso |
| Nombre                       |          | string                                                                              | Escriba un nombre para la máquina virtual. |
| properties.hardwareProfile |          | [HardwareProfile](/rest/api/compute/virtualmachines/createorupdate#hardwareprofile) | Especifica la configuración de hardware de la máquina virtual. |
| properties.storageProfile  |          | [StorageProfile](/rest/api/compute/virtualmachines/createorupdate#storageprofile)   | Especifica la configuración de almacenamiento de los discos de máquina virtual. |
| properties.osProfile       |          | [OSProfile](/rest/api/compute/virtualmachines/createorupdate#osprofile)             | Especifica la configuración del sistema operativo de la máquina virtual. |
| properties.networkProfile  |          | [NetworkProfile](/rest/api/compute/virtualmachines/createorupdate#networkprofile)   | Especifica las interfaces de red de la máquina virtual. |

Para obtener una lista completa de las definiciones disponibles en el cuerpo de solicitud, consulte las [definiciones de cuerpo de solicitud de creación o actualización de máquinas virtuales](/rest/api/compute/virtualmachines/createorupdate#definitions).

### <a name="example-request-body"></a>Cuerpo de solicitud de ejemplo

El cuerpo de solicitud de ejemplo siguiente define una imagen de Ubuntu 18.04 LTS que usa discos administrados Premium. Se usa la autenticación de clave pública SSH, y la máquina virtual emplea una tarjeta de interfaz de red (NIC) virtual existente que ha [creado anteriormente](../../virtual-network/virtual-network-network-interface.md). Proporcione la clave pública SSH en el campo *osProfile.linuxConfiguration.ssh.publicKeys.keyData*. Si es necesario, puede [generar un par de claves SSH](mac-create-ssh-keys.md).

```json
{
  "location": "eastus",
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_DS1_v2"
    },
    "storageProfile": {
      "imageReference": {
        "sku": "18.04-LTS",
        "publisher": "Canonical",
        "version": "latest",
        "offer": "UbuntuServer"
      },
      "osDisk": {
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS"
        },
        "name": "myVMosdisk",
        "createOption": "FromImage"
      }
    },
    "osProfile": {
      "adminUsername": "{your-username}",
      "computerName": "myVM",
      "linuxConfiguration": {
        "ssh": {
          "publicKeys": [
            {
              "path": "/home/{your-username}/.ssh/authorized_keys",
              "keyData": "ssh-rsa AAAAB3NzaC1{snip}mf69/J1"
            }
          ]
        },
        "disablePasswordAuthentication": true
      }
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/{existing-nic-name}",
          "properties": {
            "primary": true
          }
        }
      ]
    }
  },
  "name": "myVM"
}
```

## <a name="responses"></a>Respuestas

Hay dos respuestas correctas a la operación para crear o actualizar una máquina virtual:

| NOMBRE        | Escriba                                                                              | DESCRIPCIÓN |
|-------------|-----------------------------------------------------------------------------------|-------------|
| 200 OK      | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | OK          |
| 201 Creado | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Creado     |

Para más información sobre las respuestas de API REST, consulte [Process the response message](/rest/api/azure/#process-the-response-message) (Procesamiento del mensaje de respuesta).

### <a name="example-response"></a>Respuesta de ejemplo

Una respuesta *201 Creado* condensada del cuerpo de solicitud de ejemplo anterior que crea una máquina virtual muestra que se ha asignado *vmId* y que *provisioningState* es *Creando*:

```json
{
    "vmId": "e0de9b84-a506-4b95-9623-00a425d05c90",
    "provisioningState": "Creating"
}
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las API REST de Azure u otras herramientas de administración como la CLI de Azure o Azure PowerShell, consulte lo siguiente:

- [API REST del proveedor de Azure Compute](/rest/api/compute/)
- [Get started with Azure REST API](/rest/api/azure/) (Introducción a la API REST de Azure)
- [CLI de Azure](/cli/azure/)
- [Módulo de Azure PowerShell](/powershell/azure/overview)
