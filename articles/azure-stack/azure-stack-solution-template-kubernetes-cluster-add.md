---
title: Adición de Kubernetes a Marketplace de Azure Stack | Microsoft Docs
description: Obtenga información sobre cómo agregar Kubernetes a Marketplace de Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: 69bf788ef30a18bbe70e251fdd6a814d0f528f55
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994571"
---
# <a name="add-kubernetes-to-the-azure-stack-marketplace"></a>Adición de Kubernetes a Marketplace de Azure Stack

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

> [!note]  
> Kubernetes en Azure Stack está en versión preliminar. Para solicitar acceso al elemento Clúster de Kubernetes de Marketplace necesario para seguir las instrucciones de este artículo, [envíe una solicitud para obtener acceso](https://aka.ms/azsk8).

Puede ofrecer Kubernetes como un elemento de Marketplace a los usuarios. Los usuarios pueden implementar Kubernetes en una sola operación coordinada.

En el artículo siguiente explica cómo usar una plantilla de Azure Resource Manager para implementar y aprovisionar los recursos para un clúster de Kubernetes independiente. Antes de empezar, compruebe Azure Stack y la configuración de inquilino de Azure global. Recopile la información necesaria sobre su entorno de Azure Stack. Agregue los recursos necesarios a su inquilino y a Marketplace de Azure Stack. El clúster depende de que Ubuntu Server, el script personalizado y los elementos de Kubernetes se encuentren en Marketplace.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Crear un plan, una oferta y una suscripción

Cree un plan, una oferta y una suscripción para el elemento de Marketplace de Kubernetes. También puede usar un plan y una oferta existentes.

1. Inicie sesión en el [portal de administración](https://adminportal.local.azurestack.external).

1. Cree un plan como plan base. Para obtener instrucciones, vea [Creación de un plan en Azure Stack](azure-stack-create-plan.md).

1. Cree una oferta. Para obtener instrucciones, vea [Creación de una oferta en Azure Stack](azure-stack-create-offer.md).

1. Seleccione **Ofertas** y busque la que ha creado.

1. Seleccione **Información general** en la hoja Oferta.

1. Seleccione **Cambiar estado**. Seleccione **Público**.

1. Seleccione **+ Crear un recurso** > **Offers and Plans** (Ofertas y planes)  > **Suscripción** para crear una suscripción.

    a. Especifique un **Nombre para mostrar**.

    b. Especifique un **Usuario**. Use la cuenta de Azure AD asociada con su inquilino.

    c. **Descripción del proveedor**

    d. Establezca el **Inquilino de directorio** en el inquilino de Azure AD de su entorno de Azure Stack. 

    e. Seleccione **Oferta**. Seleccione el nombre de la oferta que ha creado. Tome nota del identificador de suscripción.

## <a name="add-an-ubuntu-server-image"></a>Agregar una imagen de Ubuntu Server

Agregue la siguiente imagen de Ubuntu Server en Marketplace:

1. Inicie sesión en el [portal de administración](https://adminportal.local.azurestack.external).

1. Seleccione **Todos los servicios** y, a continuación, en la categoría **Administración**, seleccione **Marketplace management** (Administración de Marketplace).

1. Seleccione **+ Add from Azure** (+ Agregar desde Azure).

1. Escriba `UbuntuServer`.

1. Seleccione la versión más reciente del servidor con el perfil siguiente:
    - **Publicador**: Canonical
    - **Oferta**: UbuntuServer
    - **SKU**: 16.04-LTS

1. Seleccione **Descargar**.

## <a name="add-a-custom-script-for-linux"></a>Agregar un script personalizado para Linux

Agregue Kubernetes desde Marketplace:

1. Abra el [portal de administración](https://adminportal.local.azurestack.external).

1. Seleccione **Todos los servicios** y, a continuación, en la categoría **Administración**, seleccione **Marketplace management** (Administración de Marketplace).

1. Seleccione **+ Add from Azure** (+ Agregar desde Azure).

1. Escriba `Custom Script for Linux`.

1. Seleccione el script con el perfil siguiente:
    - **Oferta**: Script personalizado para Linux 2.0
    - **Versión**: 2.0.6
    - **Publicador**: Microsoft Corp.

    > [!Note]  
    > Podría aparecer más de una versión del script personalizado para Linux. Debe agregar la versión que coincida. Kubernetes requiere la versión exacta del elemento.

1. Seleccione **Descargar**.


## <a name="add-kubernetes-to-the-marketplace"></a>Adición de Kubernetes a Marketplace

1. Abra el [portal de administración](https://adminportal.local.azurestack.external).

1. Seleccione **Todos los servicios** y, a continuación, en la categoría **Administración**, seleccione **Marketplace management** (Administración de Marketplace).

1. Seleccione **+ Add from Azure** (+ Agregar desde Azure).

1. Escriba `Kubernetes`.

1. Seleccione `Kubernetes Cluster`.

1. Seleccione **Descargar**.

    > [!note]  
    > El elemento podría tardar cinco minutos en aparecer en Marketplace.

    ![kubernetes](user\media\azure-stack-solution-template-kubernetes-deploy\marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes"></a>Actualización o eliminación de Kubernetes 

Al actualizar el elemento de Kubernetes, debe quitar el elemento que se encuentra en Marketplace. Después, siga las instrucciones de este artículo para agregar Kubernetes a Marketplace.

Para quitar el elemento de Kubernetes:

1. Anote el nombre del elemento actual, por ejemplo, `Microsoft.AzureStackKubernetesCluster.0.2.0`.

1. Conéctese a Azure Stack con PowerShell.

1. Use el siguiente cmdlet de PowerShell para quitar el elemento:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.2.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Pasos siguientes

[Implementación de un clúster de Kubernetes en Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)



[Introducción a la oferta de servicios en Azure Stack](azure-stack-offer-services-overview.md)