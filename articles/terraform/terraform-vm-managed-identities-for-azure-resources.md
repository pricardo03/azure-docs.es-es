---
title: 'Tutorial: Creación de una máquina virtual Linux con una identidad administrada a partir de la imagen de Azure Marketplace mediante Terraform'
description: Creación de una máquina virtual Linux de Terraform con una identidad administrada y administración de estado remoto mediante la imagen de Azure Marketplace
ms.topic: tutorial
ms.date: 11/07/2019
ms.openlocfilehash: 6bc6bec2a13dcd5747823de739d1dd11c6027091
ms.sourcegitcommit: 28688c6ec606ddb7ae97f4d0ac0ec8e0cd622889
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2019
ms.locfileid: "74158976"
---
# <a name="tutorial-create-a-linux-vm-with-a-managed-identity-from-the-azure-marketplace-image-using-terraform"></a>Tutorial: Creación de una máquina virtual Linux con una identidad administrada a partir de la imagen de Azure Marketplace mediante Terraform

En este artículo, se muestra cómo usar una [imagen de Marketplace de Terraform](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.terraform?tab=Overview) para crear una máquina virtual Ubuntu Linux (16.04 LTS) con la versión más reciente de [Terraform](https://www.terraform.io/intro/index.html) instalada y configurada mediante las [identidades administradas para recursos de Azure](/azure/active-directory/managed-service-identity/overview). Esta imagen también configura un back-end remoto para habilitar la administración del [estado remoto](https://www.terraform.io/docs/state/remote.html) mediante Terraform. 

La imagen de Marketplace de Terraform permite empezar a trabajar fácilmente con Terraform en Azure sin necesidad de instalar ni configurar Terraform manualmente. 

No se aplica ningún cargo de software por esta imagen de máquina virtual de Terraform. Pague solo el precio de uso del hardware de Azure en función del tamaño de la máquina virtual aprovisionada. 

Para más información acerca del precio de proceso, consulte la [página de precios de las máquinas virtuales Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="prerequisites"></a>Requisitos previos
Para poder crear una máquina virtual Linux de Terraform, es preciso tener una suscripción a Azure. Si aún no tiene una, consulte [Cree su cuenta gratuita de Azure hoy mismo](https://azure.microsoft.com/free/).  

## <a name="create-your-terraform-vm"></a>Creación de una máquina virtual de Terraform 

Estos son los pasos que se deben seguir para crear una instancia de una máquina virtual Linux de Terraform: 

1. En Azure Portal, vaya a la lista [Crear un recurso](https://ms.portal.azure.com/#create/hub).

1. En la barra de búsqueda de **Buscar en el Marketplace**, busque **Terraform**. 

1. Seleccione el botón **Crear**. 

1. En las secciones siguientes, se proporciona información acerca de los pasos del asistente para crear la máquina virtual Linux de Terraform. En la siguiente sección, se muestran los datos necesarios para configurar cada uno de estos pasos.

## <a name="details-on-the-create-terraform-tab"></a>Datos de la pestaña Create Terraform (Crear Terraform)

Especifique los datos siguientes en la pestaña **Create Terraform** (Crear Terraform):

1. **Aspectos básicos**
    
   * **Nombre**: nombre de la máquina virtual de Terraform.
   * **Nombre de usuario**: identificador de acceso de la primera cuenta.
   * **Contraseña**: primera contraseña de la cuenta. (Puede usar una clave pública SSH en lugar de una contraseña).
   * **Suscripción**: suscripción en la que se va a crear y facturar la máquina. Debe tener privilegios de creación de recursos en esta suscripción.
   * **Grupos de recursos**: un grupo de recursos nuevo o existente.
   * **Ubicación**: seleccione el centro de datos más adecuado. Normalmente, será el centro de datos que tenga la mayor parte de los datos o el que esté más cerca de su ubicación física para disfrutar de un acceso más rápido a la red.

2. **Configuración adicional**

   * **Tamaño**: Tamaño de la máquina virtual. 
   * **Tipo de disco de máquina virtual**: SSD o HDD.

3. **Resumen Terraform**

   * Compruebe que toda la información que ha especificado es correcta. 

4. **Comprar**

   * Para iniciar el proceso de aprovisionamiento, seleccione **Comprar**. Se proporciona un vínculo a los términos de la transacción. El único cargo que se aplicará a la máquina virtual es el de proceso para el tamaño de servidor que elija en el paso de selección de tamaño.

La imagen de la máquina virtual de Terraform realiza estos pasos:

* Crea una máquina virtual con una identidad asignada por el sistema basada en la imagen de Ubuntu 16.04 LTS.
* Instala las identidades administradas para la extensión de los recursos de Azure en la máquina virtual para permitir que se emitan tokens de OAuth para los recursos de Azure.
* Asigna permisos de RBAC a la identidad administrada y le concede derechos de propietario para el grupo de recursos.
* Crea una carpeta con la plantilla de Terraform (tfTemplate).
* Establece la configuración previa del estado remoto de Terraform con el back-end de Azure.

## <a name="access-and-configure-a-linux-terraform-vm"></a>Configuración de una máquina virtual Linux de Terraform y acceso a ella

Después de crear la máquina virtual, siga estos pasos:

1. Inicie sesión en la máquina virtual mediante SSH. Use las credenciales de la cuenta que creó en la sección anterior. En Windows, puede descargar una herramienta de cliente SSH como [Putty](https://www.putty.org/).

1. Conceda permisos de colaborador para toda la suscripción a identidades administradas para los recursos de Azure en la máquina virtual. 

    El permiso de colaborador ayuda a las identidades administradas de los recursos de Azure que están en la máquina virtual a usar Terraform para crear recursos fuera del grupo de recursos de la máquina virtual. Para realizar esta acción, ejecute el siguiente script: 
    
    ```bash
    . ~/tfEnv.sh
    ```

    Este script usa el mecanismo de [inicio de sesión interactivo de la CLI de Azure](/cli/azure/authenticate-azure-cli?view=azure-cli-latest#sign-in-interactively) para autenticarse en Azure. Este proceso asigna el [permiso de colaborador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) para toda la suscripción. 

1. La máquina virtual tiene un back-end de estado remoto de Terraform. Para habilitarlo en la implementación de Terraform, debe copiar el archivo `remoteState.tf` en la raíz de los scripts de Terraform.

    ```bash
    cp  ~/tfTemplate/remoteState.tf .
    ```

    Para más información acerca de la administración del estado remoto, consulte el artículo que explica el artículo en el que se explica el [estado remoto de Terraform](https://www.terraform.io/docs/state/remote.html). La clave de acceso de almacenamiento se expone en este archivo. Exclúyala antes de confirmar los archivos de configuración de Terraform en el control de código fuente.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Más información sobre el uso de Terraform en Azure](/azure/terraform)