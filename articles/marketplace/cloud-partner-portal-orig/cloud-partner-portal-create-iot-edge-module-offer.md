---
title: Creación de una oferta de módulo IoT Edge | Microsoft Docs
description: Publicación de un nuevo módulo IoT Edge para Marketplace.
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
ms.date: 09/19/2018
ms.author: pbutlerm
ms.openlocfilehash: bf7d639c682e443f29b31b3c6d7438e89c406fde
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52838640"
---
# <a name="how-to-publish-a-new-iot-edge-module-in-the-cloud-partner-portal"></a>Publicación de un nuevo módulo IoT Edge en Cloud Partner Portal

En este artículo se describen los pasos para publicar una nueva oferta de módulo IoT Edge.

## <a name="prerequisites"></a>Requisitos previos

Los requisitos previos siguientes se aplican a la publicación de un módulo I para Azure Marketplace.

-   Acceda a [Cloud Partner Portal (CPP)](https://cloudpartner.azure.com/#alloffers). Para más información, consulte la [guía de publicación](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).

-   Hospede el módulo IoT Edge en Azure Container Registry.

-   Prepare los metadatos del módulo IoT Edge (se incluye una lista no exhaustiva):

    -   Título

    -   Descripción (formato HTML básico)

    -   Un logotipo en formato de imagen png en los siguientes tamaños: 40 píxeles x 40 píxeles, 90 píxeles x 90 píxeles, 115 píxeles x 115 píxeles, 255 píxeles x 115 píxeles.

    -   Términos de uso y directiva de privacidad

<a name="prepare-your-iot-edge-module-listing-in-cpp"></a>Preparación de la lista de módulos IoT Edge en CPP
-------------------------------------------

### <a name="create-a-new-offer-of-the-type-iot-edge-module"></a>Creación de una oferta del tipo módulo IoT Edge 

Siga estos pasos para preparar la lista de módulos IoT Edge:

-   Inicie sesión en su [cuenta de CPP](https://cloudpartner.azure.com/).

>[!Note]
>Para información general sobre Cloud Partner Portal, puede revisar la [documentación](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-getting-started-with-the-cloud-partner-portal)

-   Seleccione **Nueva oferta** y seleccione **Módulo IoT Edge**.

>[!NOTE]
>Un módulo IoT Edge es un contenedor diseñado específicamente para ejecutarse en IoT Edge. Estos escenarios dirigidos por un módulo IoT Edge deben tener sentido en un contexto de IoT Edge. También incluye la configuración predeterminada para que la implementación en un dispositivo IoT Edge se realice de manera simple. Es posible que el contenedor también incluya el SDK del módulo IoT Edge para habilitar la comunicación con EdgeHub e IoT Hub.

### <a name="define-your-offer-settings"></a>Definición de la configuración de la oferta

En la pestaña Configuración de la oferta, escriba la información de la oferta.

![Identidad de la oferta](./media/cloud-partner-portal-create-iot-edge-module-offer/offer-identity.png)


-   El **identificador de la oferta** identifica de manera única la oferta en CPP y se puede usar en direcciones URL orientadas al cliente.

-   Solo usted puede ver el **nombre** para hacer referencia a esta oferta en CPP.

### <a name="create-one-or-more-skus"></a>Creación de una o varias SKU

Cada SKU corresponde a una imagen de contenedor. Debe tener al menos una SKU y puede agregar más de una. Una SKU tiene dos partes:

-   Metadatos de la SKU

-   Metadatos del contenedor

**Para crear una SKU:**

Seleccione la pestaña **SKU** y, luego, **Nueva SKU**.

![Nueva SKU](./media/cloud-partner-portal-create-iot-edge-module-offer/SKUs.png)

Los metadatos de la SKU contienen los campos siguientes obligatorios:
- SKU ID (Identificador de la SKU): un identificador único.
- Title (Título): el título de la SKU de hasta 50 caracteres.
- Summary (Resumen): una descripción breve de hasta 100 caracteres.
- Descripción: una descripción larga.
- Hide this SKU (Ocultar esta SKU): el valor predeterminado es **No**.
   
![Detalles de la SKU](./media/cloud-partner-portal-create-iot-edge-module-offer/SKU-settings.png)

#### <a name="iot-edge-module-metadata-and-the-container-registry"></a>Metadatos del módulo IoT Edge y el registro de contenedor

Los metadatos del módulo IoT Edge tienen información de referencia de imagen que se almacenan en Azure Container Registry (ACR). Azure Marketplace copia la imagen en el registro de Marketplace público y está disponible para los clientes después de la certificación. Todas las solicitudes de usuario para consumir una imagen de módulo IoT Edge se atienden desde el registro de contenedor de Marketplace.

![Imágenes de contenedor](./media/cloud-partner-portal-create-iot-edge-module-offer/container-images.png)

**Imágenes de contenedor**

Los detalles del repositorio de imágenes contienen estos campos obligatorios:

-   **Id. de suscripción**: Id. de la suscripción de Azure donde el registro de ACR está presente.

-   **Nombre del grupo de recursos**: el nombre del grupo de recursos del registro de ACR.

-   **Nombre del registro**: nombre del registro de ACR.

-   **Nombre del repositorio**: el nombre del repositorio. Una vez establecido, no es posible cambiar este valor. El nombre debe ser único para que ninguna otra oferta en su cuenta tenga el mismo nombre.

-   **Nombre de usuario**: el nombre de usuario asociado con ACR (nombre de usuario de administrador).

-   **Contraseña**: la contraseña asociada con ACR.

    >[!Note]
    >El nombre de usuario y la contraseña son necesarios para asegurar que los asociados tienen acceso al ACR descrito en el proceso de publicación.

Cuando esté pensando en publicar una imagen de módulo IoT Edge, puede proporcionar una o varias etiquetas de imagen. No olvide agregar una etiqueta "latest" (el valor predeterminado) al módulo para poder identificarlo fácilmente durante las pruebas.

También puede indicar los siguientes aspectos específicos del módulo IoT Edge:

-   **createOptions**: el valor predeterminado de createOptions que se va a pasar para que este módulo IoT Edge se pueda iniciar de inmediato.

-   **twin:**: el dispositivo gemelo predeterminado que se va a pasar para que este módulo IoT Edge se pueda iniciar de inmediato cuando se use el SDK del módulo IoT.

-   **twin:**: el dispositivo gemelo predeterminado que se va a pasar para que este módulo IoT Edge se pueda iniciar de inmediato cuando se use el SDK del módulo IoT.

### <a name="describe-your-iot-edge-module-for-your-customers"></a>Descripción del módulo IoT Edge para los clientes

En la pestaña Marketplace, agregue el contenido específico para marketing. Esta información estará visible públicamente y se mostrará en Azure Marketplace.

![Información general sobre el módulo IoT Edge](./media/cloud-partner-portal-create-iot-edge-module-offer/overview.png)

-   **Title** (Título): título del módulo IoT Edge orientado al público.

-   **Summary** (Resumen): resumen del módulo IoT Edge orientado al cliente en la mayoría de las páginas, como las páginas de exploración.

-   **Long summary** (Resumen largo): resumen del módulo IoT Edge orientado al cliente cuando se presenta el módulo. 

-   **Description** (Descripción): descripción del módulo IoT Edge orientado al cliente en la página de detalles del producto. (Puede usar etiquetas HTML básicas para dar formato a la descripción).

-   **Marketing identifier** (Identificador de marketing): identificador único que se usa para crear la dirección URL del producto. Esta dirección URL tiene el formato siguiente: *azuremarketplace.microsoft.com/enus/marketplace/apps/suiddeeditor.suidentificadordemarketingdemóduloiotedge*.

-   **Preview subscription Ids** (Id. de suscripción de versión preliminar): los usuarios que tienen acceso a estas suscripciones podrán ver el módulo IoT Edge después del paso de certificación y antes de su publicación.

-   **Useful links** (Vínculos útiles): puede agregar hasta 10 vínculos que se mostrarán en la página de detalles del producto.

-   **Suggested categories** (Categorías sugeridas): escoja hasta cinco categorías. Se mostrarán en la página de detalles del producto. Actualmente, los módulos IoT Edge aparecen debajo de las categorías *Internet de las cosas \>Módulo IoT Edge* en las páginas de exploración.

-   **Logotipos**: cargue las imágenes del logotipo del módulo IoT Edge en formato PNG. Use exactamente los siguientes tamaños: 40 píxeles x 40 píxeles, 90 píxeles x 90 píxeles, 115 píxeles x 115 píxeles, 255 píxeles x 115 píxeles.

-   **Screenshots** (Capturas de pantalla): las capturas de pantalla se muestran en la página de detalles del producto. Son una excelente forma de comunicar de manera visual lo que hace el módulo IoT Edge y cómo funciona. Por ejemplo, puede mostrar diagramas de arquitectura o ilustraciones de casos de uso.

-   **Videos** (Vídeos): los vídeos se muestran en la página de detalles del producto. Son una excelente forma de comunicar de manera visual lo que hace el módulo IoT Edge y cómo funciona.

-   **Lead management** (Administración de clientes potenciales): puede elegir un sistema para recopilar todos los clientes potenciales que muestran interés en el producto.

-   **Privacy** (Privacidad): debe haber una dirección URL que apunte a la directiva de privacidad.

-   **Términos de uso**: debe usar los términos de uso. Puede usar etiquetas HTML para dar formato a esta página o apuntar a una de sus otras páginas.

### <a name="enter-your-support-contact-information"></a>Información de contacto del soporte técnico

En la pestaña Soporte técnico, escriba la información del **contacto de ingeniería** y del **soporte técnico para el cliente**.

![Contactos de soporte técnico](./media/cloud-partner-portal-create-iot-edge-module-offer/support-contact-info.png)


## <a name="certify-your-iot-edge-module"></a>Certificación del módulo IoT Edge

Una vez que proporcione toda la información necesaria, seleccione **Publicar** para enviar el módulo IoT Edge para su certificación. Verá una escala de tiempo en la que se muestran los pasos del proceso de certificación.

**Comprobación del módulo**

Nuestro equipo de certificación comprueba el módulo. Una vez que se certifica el módulo, recibirá un vínculo privado para probarlo. Si necesita hacer cambios después de la prueba, edite los metadatos de la oferta y vuelva a enviar el módulo al equipo de certificación. 

## <a name="publish-your-iot-edge-module"></a>Publicación del módulo IoT Edge

Cuando termine la prueba y esté listo para publicar el módulo, seleccione **Go Live** (Publicar) para publicar el módulo IoT Edge.

>[!Important]
>Si quiere que su módulo IoT Edge se anuncie en el evento Ignite, debe estar publicado antes del 23 de septiembre de 2018.
