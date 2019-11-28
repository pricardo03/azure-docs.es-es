---
title: 'Tutorial: Configuración de Azure Cloud Shell para Terraform'
description: Use Terraform con Azure Cloud Shell para simplificar la autenticación y la configuración de plantillas.
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: db9edfadbe01edc1ee9df09c284e3895ee11f3d3
ms.sourcegitcommit: 28688c6ec606ddb7ae97f4d0ac0ec8e0cd622889
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2019
ms.locfileid: "74159132"
---
# <a name="tutorial-configure-azure-cloud-shell-for-terraform"></a>Tutorial: Configuración de Azure Cloud Shell para Terraform

Terraform funciona bien desde una línea de comandos de Bash en macOS, Windows o Linux. La ejecución de las configuraciones de Terraform en la experiencia Bash de [Azure Cloud Shell](/azure/cloud-shell/overview) tiene algunas ventajas exclusivas. En este tutorial se muestra cómo escribir scripts de Terraform que se implementan en Azure mediante Cloud Shell.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Configuración automática de credenciales

Terraform se instala y está disponible inmediatamente en Cloud Shell. Los scripts de Terraform se autentican en Azure cuando se ha iniciado sesión en Cloud Shell para administrar la infraestructura sin configuración adicional. La autenticación automática omite dos procesos manuales:
- Creación de una entidad de servicio de Azure Active Directory
- Configuración de las variables de proveedor de Azure Terraform


## <a name="use-modules-and-providers"></a>Uso de módulos y proveedores

Los módulos de Azure Terraform requieren credenciales para acceder a los recursos de Azure y modificarlos. Para usar los módulos de Terraform en Cloud Shell, agregue el código siguiente:


```hcl
# Configure the Microsoft Azure Provider
provider "azurerm" {
}
```

Cloud Shell pasa los valores necesarios al proveedor de `azurerm` a través de las variables de entorno cuando se usa cualquiera de los comandos de la CLI de `terraform`.

## <a name="other-cloud-shell-developer-tools"></a>Otras herramientas de desarrollo de Cloud Shell

Los archivos y los estados de shell se conservan en Azure Storage entre sesiones de Cloud Shell. Use el [Explorador de Azure Storage](/azure/vs-azure-tools-storage-manage-with-storage-explorer) para copiar y cargar archivos a Cloud Shell desde un equipo local.

La CLI de Azure está disponible en Cloud Shell y es una fantástica herramienta para probar las configuraciones y comprobar el trabajo después de que se completan `terraform apply` o `terraform destroy`.


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de un pequeño clúster de VM mediante el registro de módulos](terraform-create-vm-cluster-module.md)
