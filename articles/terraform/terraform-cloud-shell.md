---
title: Usar Terraform con Azure Cloud Shell
description: Use Terraform con Azure Cloud Shell para simplificar la autenticación y la configuración de plantillas.
services: terraform
ms.service: terraform
keywords: terraform, devops, conjunto de escalado, máquina virtual, red, almacenamiento, módulos
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 10/19/2017
ms.openlocfilehash: af71cf07ea12e605f57d34b530fcbcd61838128b
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54074555"
---
# <a name="terraform-cloud-shell-development"></a>Desarrollo de Cloud Shell de Terraform 

Terraform funciona a la perfección desde una línea de comandos Bash como un terminal macOS o Bash en Windows o Linux. Ejecutar las configuraciones de Terraform en la experiencia Bash de [Azure Cloud Shell](/azure/cloud-shell/overview) tiene algunas ventajas exclusivas que aceleran el ciclo de desarrollo.

En este artículo de conceptos se describen las características de Cloud Shell que le ayudan a escribir scripts de Terraform que se implementan en Azure.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Configuración automática de credenciales

Terraform se instala y está disponible inmediatamente en Cloud Shell. Los scripts de Terraform se autentican con Azure cuando se inicia sesión en Cloud Shell para administrar la infraestructura sin ninguna configuración adicional. La autenticación omite la necesidad de crear manualmente una entidad de servicio de Active Directory y de configurar las variables de proveedor de Terraform de Azure.


## <a name="using-modules-and-providers"></a>Uso de módulos y proveedores

Los módulos de Terraform de Azure requieren credenciales para acceder a los recursos de la suscripción de Azure y modificarlos. Cuando trabaje en Cloud Shell, agregue el código siguiente a sus scripts para usar módulos de Terraform de Azure en Cloud Shell:

```tf
# Configure the Microsoft Azure Provider
provider "azurerm" {
}
```

Cloud Shell pasa los valores necesarios para el proveedor `azurerm` a través de variables de entorno cuando se usa alguno de los comandos de la CLI de `terraform`.

## <a name="other-cloud-shell-developer-tools"></a>Otras herramientas de desarrollo de Cloud Shell

Los archivos y los estados de shell se conservan en Azure Storage entre sesiones de Cloud Shell. Use [Explorador de Azure Storage](/azure/vs-azure-tools-storage-manage-with-storage-explorer) para copiar y cargar archivos a Cloud Shell desde el equipo local.

La CLI de Azure está disponible en Cloud Shell y es una fantástica herramienta para probar las configuraciones y comprobar el trabajo después de completarse una acción `terraform apply` o `terraform destroy`.


## <a name="next-steps"></a>Pasos siguientes

[Crear un pequeño clúster de VM mediante el registro de módulo](terraform-create-vm-cluster-module.md)
[Crear un pequeño clúster de VM mediante HCL personalizado](terraform-create-vm-cluster-with-infrastructure.md)
