---
title: Contenedores | Microsoft Docs
description: Pasos para publicar una imagen de contenedor.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 5653e4b2ec9aa5e21a107611b9d4142168630d4a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48807983"
---
<a name="publishing-a-container-image-in-the-cloud-partner-portal"></a>Publicación de una imagen de contenedor en Cloud Partner Portal
========================================================

En este artículo se describe cómo publicar una nueva imagen de contenedor en Cloud Partner Portal.

Siga estos pasos para publicar una nueva imagen de contenedor.

1. Seleccione **Nueva oferta** y después **Contenedores**

    ![Seleccionar la opción Contenedores de Nueva oferta](media/cpp-containers-guide/azure-container-offer.png)

2. En la pestaña Configuración de la oferta, bajo Identidad de la oferta, incluya el **Id. de la oferta**, el **Id. del publicador** y el **Nombre**.

    ![Identidad de la oferta](media/cpp-containers-guide/containers-offer-settings.png)

3. En la pestaña SKU, seleccione **Nueva SKU**.
    >[!NOTE]
    >Puede agregar más de una SKU.

    ![Mensaje Nueva SKU](media/cpp-containers-guide/containers-sku-settings.png)

4. Proporcione la información del SKU y el contenedor. Cada SKU corresponde a una imagen de contenedor. Una SKU tiene dos partes:

    -   Metadatos de la SKU
    -   Metadatos del contenedor

    Los metadatos de la SKU contienen la información de presentación de las imágenes de contenedor.

    ![Metadatos de la SKU](media/cpp-containers-guide/containers-sku-details.png)

    Los metadatos del contenedor contienen información de referencia de los detalles del repositorio de imágenes dentro de Azure Container Registry (ACR). Después, Azure Marketplace copia esta imagen en el registro de marketplace público y pasa a estar disponible para los clientes después de la certificación. Todas las solicitudes del usuario de Azure para utilizar una imagen de contenedor se sirven desde el registro de contenedor de Marketplace.

   ![Metadatos del contenedor](media/cpp-containers-guide/containers-image-repository.png)

    Los detalles del repositorio de imágenes en la captura de pantalla anterior contienen los siguientes campos:

    -   **Id. de suscripción**: Id. de la suscripción de Azure donde el registro de ACR está presente.
    -   **Nombre del grupo de recursos**: el nombre del grupo de recursos del registro de ACR.
    -   **Nombre del registro**: nombre del registro de ACR.
    -   **Nombre del repositorio**: el nombre del repositorio. Una vez establecido, no se puede cambiar este valor. Debe proporcionarse un nombre único de forma que ninguna otra oferta en su cuenta tenga el mismo nombre.
    -   **Nombre de usuario**: el nombre de usuario asociado con ACR (nombre de usuario de administrador).
    -   **Contraseña**: la contraseña asociada con ACR.

    >[!NOTE]
    >El nombre de usuario y la contraseña son necesarios para asegurar que los asociados tienen acceso al ACR mencionado en el proceso de publicación.

    Al publicar una imagen de contenedor, también puede proporcionar una o varias etiquetas de imagen. Además de una etiqueta de imagen, los asociados también pueden proporcionar resúmenes SHA. No olvide agregar una **etiqueta de prueba** a la imagen para facilitar su identificación durante las pruebas.

5. Agregue el contenido de marketing específico en la pestaña Marketplace.

    ![Información de Marketplace](media/cpp-containers-guide/containers-marketplace-tab.png)

6. En la pestaña Soporte técnico, escriba su contacto de ingeniería y la información de soporte técnico del cliente.

   ![Información de compatibilidad](media/cpp-containers-guide/containers-support-tab.png)

7. Seleccione **Publicar** para publicar la oferta. Después de seleccionar Publicar, podrá ver una escala de tiempo que muestra los pasos necesarios para publicar la imagen de contenedor.
