---
title: Configurar el entorno de desarrollo de Azure Red Hat OpenShift | Microsoft Docs
description: Estos son los requisitos previos para trabajar con Microsoft Azure Red Hat OpenShift.
services: openshift
keywords: configurar el programa de instalación de red hat openshift
author: jimzim
ms.author: jzim
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: openshift
manager: jeconnoc
ms.openlocfilehash: f0ef421d7954aa33cf69e7de2f4902a86ed8b580
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306398"
---
# <a name="set-up-your-azure-red-hat-openshift-dev-environment"></a>Configuración del entorno de desarrollo de Red Hat OpenShift en Azure

Para compilar y ejecutar aplicaciones de Microsoft Azure Red Hat OpenShift, deberá:

* Comprar instancias reservadas de máquina virtual Azure.
* Instalar versión 2.0.65 (o superior) de la CLI de Azure (o use Azure Cloud Shell).
* Registrarse para el `openshiftmanagedcluster` característica y los proveedores de recursos asociados.
* Crear a un inquilino de Azure Active Directory (Azure AD).
* Cree un objeto de aplicación de Azure AD.
* Creación de un usuario de Azure AD.

Las instrucciones siguientes le guiará a través de todos estos requisitos previos.

## <a name="purchase-azure-red-hat-openshift-application-nodes-reserved-instances"></a>Comprar instancias reservadas de nodos de aplicación de Azure Red Hat OpenShift

Para poder usar Azure Red Hat OpenShift, deberá adquirir un mínimo de 4 nodos de la aplicación de Azure Red Hat OpenShift reservado, tras el cual podrá para aprovisionamiento de clústeres.

Si es cliente de Azure, [comprar instancias reservadas de Azure Red Hat OpenShift](https://aka.ms/openshift/buy) a través del portal de Azure. Después de la compra, se activará su suscripción dentro de 24 horas.

Si no es un cliente de Azure, [póngase en contacto con ventas](https://aka.ms/openshift/contact-sales) y rellene el formulario de venta en la parte inferior de la página para iniciar el proceso.

Hacer referencia a la [página de precios de Azure Red Hat OpenShift](https://aka.ms/openshift/pricing) para obtener más información.

## <a name="install-the-azure-cli"></a>Instalación de la CLI de Azure

Azure Red Hat OpenShift requiere la versión 2.0.65 o posterior de la CLI de Azure. Si ya ha instalado la CLI de Azure, puede comprobar qué versión tiene, ejecute:

```bash
az --version
```

La primera línea de salida tendrá la versión de la CLI, por ejemplo `azure-cli (2.0.65)`.

Aquí encontrará instrucciones para [instalar la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) si necesita una nueva instalación o una actualización.

Como alternativa, puede usar el [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview). Cuando se usa Azure Cloud Shell, no olvide seleccionar la **Bash** entorno si tiene previsto seguir el [crear y administrar un clúster de Azure Red Hat OpenShift](tutorial-create-cluster.md) serie de tutoriales.

## <a name="register-providers-and-features"></a>Registrar proveedores y características

El `Microsoft.ContainerService openshiftmanagedcluster` característica, `Microsoft.Solutions`, y `Microsoft.Network` proveedores deben estar registrados en su suscripción manualmente antes de implementar su primer clúster de Azure Red Hat OpenShift.

Para registrar manualmente estos proveedores y características, utilice las instrucciones siguientes desde un shell de Bash si ha instalado la CLI, o desde la sesión de Azure Cloud Shell (Bash) en el portal de Azure:

1. Si tiene varias suscripciones de Azure, especifique el identificador de suscripción correspondiente:

    ```bash
    az account set --subscription <SUBSCRIPTION ID>
    ```

1. Registre la característica de openshiftmanagedcluster Microsoft.ContainerService:

    ```bash
    az feature register --namespace Microsoft.ContainerService -n openshiftmanagedcluster
    ```

1. Registre el proveedor Microsoft.Storage:

    ```bash
    az provider register -n Microsoft.Storage --wait
    ```
    
1. Registre el proveedor de Microsoft.Compute:

    ```bash
    az provider register -n Microsoft.Compute --wait
    ```

1. Registre el proveedor Microsoft.Solutions:

    ```bash
    az provider register -n Microsoft.Solutions --wait
    ```

1. Registre el proveedor de Microsoft.Network:

    ```bash
    az provider register -n Microsoft.Network --wait
    ```

1. Registre el proveedor Microsoft.KeyVault:

    ```bash
    az provider register -n Microsoft.KeyVault --wait
    ```

1. Actualizar el registro del proveedor de recursos Microsoft.ContainerService:

    ```bash
    az provider register -n Microsoft.ContainerService --wait
    ```

## <a name="create-an-azure-active-directory-azure-ad-tenant"></a>Crear a un inquilino de Azure Active Directory (Azure AD)

El servicio de Azure Red Hat OpenShift requiere a un inquilino de Azure Active Directory (Azure AD) asociado que representa su organización y su relación a Microsoft. Inquilino de Azure AD le permite registrar, compilar y administrar aplicaciones, así como usar otros servicios de Azure.

Si no tiene Azure AD para usarla como el inquilino de su clúster de Azure Red Hat OpenShift, o que desea crear un inquilino de prueba, siga las instrucciones de [crear un inquilino de Azure AD para el clúster de Azure Red Hat OpenShift](howto-create-tenant.md) antes continuar con esta guía.

## <a name="create-an-azure-ad-user-security-group-and-application-object"></a>Crear un usuario, grupo de seguridad y aplicación de Azure AD objeto

Azure Red Hat OpenShift necesita permisos para realizar tareas en el clúster, como la configuración de almacenamiento. Estos permisos se representan mediante un [serviceprincipal](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object). También conviene crear un nuevo usuario de Active Directory para realizar pruebas de aplicaciones que se ejecutan en el clúster de Azure Red Hat OpenShift.

Siga las instrucciones de [crear un objeto de aplicación de Azure AD y un usuario](howto-aad-app-configuration.md) para crear una entidad de servicio, generar una URL de devolución de llamada de secreto y autenticación de cliente para la aplicación y crear un nuevo grupo de seguridad de Azure AD y el usuario para tener acceso a la clúster.

## <a name="next-steps"></a>Pasos siguientes

Ya está listo para usar Azure Red Hat OpenShift.

Pruebe el tutorial:
> [!div class="nextstepaction"]
> [Creación de un clúster de Red Hat OpenShift en Azure](tutorial-create-cluster.md)

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
