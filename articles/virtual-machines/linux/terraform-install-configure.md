---
title: Instalación y configuración de Terraform para su uso con Azure | Microsoft Docs
description: Obtenga información sobre cómo instalar y configurar Terraform para crear recursos de Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/19/2018
ms.author: echuvyrov
ms.openlocfilehash: 71cf07b227a75e53119f2f35e79ccd7926b551e7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60418870"
---
# <a name="install-and-configure-terraform-to-provision-vms-and-other-infrastructure-into-azure"></a>Instalación y configuración de Terraform para aprovisionar máquinas virtuales y otras infraestructuras en Azure
 
Terraform proporciona una manera fácil de definir, previsualizar e implementar infraestructura en la nube con un [lenguaje de plantillas simple](https://www.terraform.io/docs/configuration/syntax.html). En este artículo se describen los pasos necesarios para usar Terraform para aprovisionar recursos en Azure.

Para obtener más información sobre cómo usar Terraform con Azure, visite el [hub de Terraform](/azure/terraform).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Terraform se instala de forma predeterminada en [Cloud Shell](/azure/terraform/terraform-cloud-shell). Si decide instalar Terraform localmente, complete el paso siguiente; en caso contrario, continúe con la [configuración del acceso de Terraform a Azure](#set-up-terraform-access-to-azure).

## <a name="install-terraform"></a>Instalar Terraform

Para instalar Terraform, [descargue](https://www.terraform.io/downloads.html) el paquete apropiado para su sistema operativo en un directorio de instalación independiente. La descarga contiene un único archivo ejecutable, para el que también debe definir una ruta de acceso global. Para obtener instrucciones sobre cómo establecer la ruta de acceso en Linux y Mac, vaya a [esta página web](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux). Para obtener instrucciones sobre cómo establecer la ruta de acceso en Windows, vaya a [esta página web](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows).

Compruebe la configuración de ruta de acceso con el comando `terraform`. Se muestra una lista de las opciones disponibles de Terraform, como en la siguiente salida de ejemplo:

```bash
azureuser@Azure:~$ terraform
Usage: terraform [--version] [--help] <command> [args]
```

## <a name="set-up-terraform-access-to-azure"></a>Configurar el acceso de Terraform a Azure

Para habilitar Terraform para aprovisionar recursos en Azure, cree una [entidad de servicio de Azure AD](/cli/azure/create-an-azure-service-principal-azure-cli). La entidad de servicio concede sus scripts de Terraform para aprovisionar recursos en su suscripción de Azure.

Si tiene varias suscripciones a Azure, primero consulte su cuenta con [az account show](/cli/azure/account#az-account-show) para obtener una lista de valores de identificadores de suscripción e identificadores de inquilino:

```azurecli-interactive
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

Para utilizar una suscripción seleccionada, defina la suscripción para esta sesión con [az account set](/cli/azure/account#az-account-set). Establezca la variable de entorno `SUBSCRIPTION_ID` para almacenar el valor devuelto del campo `id` de la suscripción que quiere usar:

```azurecli-interactive
az account set --subscription="${SUBSCRIPTION_ID}"
```

Ahora puede crear una entidad de servicio para su uso con Terraform. Utilice[az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) y defina el *ámbito* de la suscripción como se indica a continuación:

```azurecli-interactive
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

Se devuelven su `appId`, `password`, `sp_name` y `tenant`. Anote los valores de `appId` y de `password`.

## <a name="configure-terraform-environment-variables"></a>Configuración de las variables de entorno de Terraform

Para configurar Terraform para que use la entidad de servicio de Azure AD, establezca las siguientes variables de entorno, que se usan por los [módulos de Terraform de Azure](https://registry.terraform.io/modules/Azure). También puede establecer el entorno si trabaja con una nube de Azure distinta de la pública.

- `ARM_SUBSCRIPTION_ID`
- `ARM_CLIENT_ID`
- `ARM_CLIENT_SECRET`
- `ARM_TENANT_ID`
- `ARM_ENVIRONMENT`

Puede usar el siguiente script de shell de ejemplo para establecer esas variables:

```bash
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id

# Not needed for public, required for usgovernment, german, china
export ARM_ENVIRONMENT=public
```

## <a name="run-a-sample-script"></a>Ejecución de un script de ejemplo

Cree un archivo `test.tf` en un directorio vacío y péguelo en el siguiente script.

```tf
provider "azurerm" {
}
resource "azurerm_resource_group" "rg" {
        name = "testResourceGroup"
        location = "westus"
}
```

Guarde el archivo y, después, inicialice la implementación de Terraform. Este paso descarga los módulos de Azure necesarios para crear un grupo de recursos de Azure.

```bash
terraform init
```

La salida es similar a la del ejemplo siguiente:

```bash
* provider.azurerm: version = "~> 0.3"

Terraform has been successfully initialized!
```

Puede obtener una vista previa de las acciones que van a completar el script de Terraform con `terraform plan`. Cuando esté listo para crear el grupo de recursos, aplique el plan de Terraform de la forma siguiente:

```bash
terraform apply
```

La salida es similar a la del ejemplo siguiente:

```bash
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_resource_group.rg
      id:       <computed>
      location: "westus"
      name:     "testResourceGroup"
      tags.%:   <computed>

azurerm_resource_group.rg: Creating...
  location: "" => "westus"
  name:     "" => "testResourceGroup"
  tags.%:   "" => "<computed>"
azurerm_resource_group.rg: Creation complete after 1s
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha instalado Terraform o ha utilizado Cloud Shell para configurar las credenciales de Azure y comenzar a crear recursos en su suscripción a Azure. Para crear una implementación más completa de Terraform en Azure, consulte el artículo siguiente:

> [!div class="nextstepaction"]
> [Crear una máquina virtual de Azure con Terraform](terraform-create-complete-vm.md)
