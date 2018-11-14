---
title: Publicación de una plantilla de solución | Microsoft Docs
description: Publique una plantilla de solución en Azure Marketplace.
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
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: 5320b8d5ca7456a6f1b0fdd1372c9f39ac1edfb0
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219416"
---
# <a name="publish-a-solution-template-to-azure-marketplace"></a>Publicación de una plantilla de solución en Azure Marketplace

En este artículo se proporcionan los pasos para publicar una oferta de plantilla de solución en Azure Marketplace.

## <a name="prerequisites"></a>Requisitos previos

Los siguientes requisitos previos técnicos y de otra índole se aplican a la publicación de una plantilla de solución en Azure Marketplace.

### <a name="technical"></a>Requisitos previos técnicos

- [Nociones sobre las plantillas de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)

- Plantillas de inicio rápido de Azure:

    - [Documentación de plantillas de inicio rápido de Azure](https://azure.microsoft.com/documentation/templates/)

    - [Documentación de inicio rápido de Azure en GitHub](https://github.com/azure/azure-quickstart-templates)

 - [Creación de la interfaz de usuario de Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)

### <a name="non-technical-business-requirements"></a>Requisitos previos no técnicos (empresariales)

-   La empresa o sus subsidiarias deben encontrarse en uno de los países de origen de venta admitidos por Azure Marketplace.

-   El producto debe contar con una licencia que sea compatible con los modelos de facturación admitidos por Azure Marketplace.

-   Usted es el responsable de poner el soporte técnico a disposición de los clientes de forma comercialmente razonable, ya sea de forma gratuita, de pago o a través del soporte técnico de la comunidad.

-   Asimismo, es responsable de la concesión de licencias para su software y las dependencias de software de terceros.

-   Proporcione contenido que cumpla los criterios para que su oferta se publique en Azure Marketplace y en el Portal de administración de Azure.

-   Acepte los términos del contrato del anunciante y de las directivas de participación de Azure Marketplace.

-   Acepte los Términos de uso, la Declaración de privacidad de Microsoft y el Contrato del programa Microsoft Azure Certified.

## <a name="before-you-begin"></a>Antes de empezar

Después de cumplir todos los requisitos previos, puede empezar a crear la oferta de plantilla de la solución. Antes de comenzar, revise la siguiente información de la oferta y la SKU.

**Oferta**

Una oferta de aplicación de Azure se corresponde con una clase de oferta de producto de un publicador. Si tiene un nuevo tipo de solución o aplicación que quiere que esté disponible en Azure Marketplace, la mejor opción es una nueva oferta. Una oferta es una colección de SKU. Cada oferta aparece como su propia entidad en Azure Marketplace.

**SKU**

Una SKU es la unidad de compra más pequeña de una oferta. Dentro de la misma clase de producto (oferta), las SKU permiten distinguir entre las diferentes características admitidas. Por ejemplo, la oferta puede ser administrada o no administrada, y se admiten distintos modelos de facturación.

Agregue varias SKU en los escenarios siguientes:
- Si quiere admitir diferentes modelos de facturación, como traiga su propia licencia (BYOL) o pago por uso (PAYG).
- Si cada SKU admite un conjunto de características distinto y si cada uno de ellos tiene precios diferentes.

Una SKU se presenta en la oferta principal de Azure Marketplace y se muestra como una entidad de compra propia en Azure Portal.

## <a name="to-create-a-new-offer"></a>Procedimiento para crear una nueva oferta

1.  Inicie sesión en [Cloud Partner Portal](http://cloudpartner.azure.com/).

2.  En la barra de navegación izquierda, haga clic en **+ Nueva oferta** y seleccione **Aplicaciones de Azure**.

    ![Crear una nueva oferta](./media/cloud-partner-portal-publish-managed-app/newOffer.png)

3.  En **Nueva oferta**, seleccione **Editor**.

    ![Editor de oferta nueva](./media/cloud-partner-portal-publish-managed-app/newOffer_OfferSettings.png)

4.  En **Editor**, deberá proporcionar información de las vistas siguientes:
    - Configuración de oferta
    - SKU
    - Marketplace
    - Soporte técnico

Cada vista contiene un conjunto de campos que debe rellenar. Los campos obligatorios se marcan con un asterisco rojo (\*).

## <a name="to-configure-offer-settings"></a>Para definir la configuración de la oferta

1. Configure los siguientes campos de **Offer Identity** (Identidad de la oferta) en Configuración de la oferta.

    **Identificador de oferta**

     Identificador único de la oferta en un perfil del anunciante. Este identificador se muestra en direcciones URL de producto, plantillas de ARM e informes de facturación. Solo puede usar caracteres alfanuméricos en minúscula o guiones (-). El identificador, que tendrá 50 caracteres como máximo, no puede terminar con un guion. 
    >[!Note]
    >Este campo queda bloqueado en cuanto se lanza una oferta.

    **Id. del editor**

    Lista desplegable de perfiles del anunciante. Elija el perfil en el que quiera publicar la oferta. 
    >[!Note]
    >Este campo queda bloqueado en cuanto se lanza una oferta.

    **Nombre**

    Nombre para mostrar de la oferta. Este nombre se mostrará en Azure Marketplace y en Azure Portal. Puede tener un máximo de 50 caracteres. Utilice las siguientes instrucciones para el nombre de la oferta:
    -  Incluya un nombre de marca que identifique el producto. 
    - No incluya aquí el nombre de su empresa a menos que sea así como se comercializa la oferta.
    - Si comercializa esta oferta en su propio sitio web, asegúrese de que el nombre sea idéntico al que aparece en su sitio web.

2. Seleccione **Guardar** para cerrar la ventana Configuración de la oferta
de su oferta.

## <a name="to-create-skus"></a>Creación de SKU
------------------

1. Seleccione **SKU**. 

    ![Nueva SKU](./media/cloud-partner-portal-publish-managed-app/newOffer_skus.png)

    El identificador de SKU es el identificador único de la SKU dentro de una oferta. Este identificador se muestra en direcciones URL de producto, plantillas de ARM e informes de facturación. El identificador de SKU:
    - Puede tener un máximo de 50 caracteres.
    - Puede contener solo caracteres alfanuméricos en minúscula o guiones (-).
    - El identificador no puede terminar con un guión.

    >[!Note]
    >Después de agregar una SKU, esta aparece en la lista de SKU en la vista de SKU. Para ver los detalles de la SKU, seleccione el nombre de la SKU. 

2. Seleccione **Nueva SKU** para proporcionar la información que se muestra en la siguiente captura de pantalla. 

    ![Detalles de la SKU](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku.png)


### <a name="sku-settings"></a>Configuración de la SKU

Proporcione la siguiente configuración de la SKU.

- **Título**: título para la SKU. Este título aparece en la galería para este elemento.

- **Resumen**: descripción breve resumida de la SKU. (La longitud máxima es de 100 caracteres).

- **Description**: descripción detallada de la SKU.

- **Tipo de SKU**: lista desplegable con los valores "Aplicación administrada (versión preliminar)" y "Plantilla de solución". Para este escenario, seleccione **Plantilla de solución**.

- **Cloud Availability** (Disponibilidad de la nube): ubicación de la SKU. El valor predeterminado es Azure público.

### <a name="package-details"></a>Detalles del paquete

Después de finalizar la configuración de la SKU, proporcione los siguientes detalles del paquete.

![Detalles del paquete](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku_ST_package.png)

- **Versión actual**: versión del paquete que va a cargar. Debe tener el formato - ..

- **Archivo de paquete**: este paquete contiene los siguientes archivos, guardados en un archivo ZIP.

    -   MainTemplate.json: archivo de plantilla de implementación que se usa para implementar la solución o la aplicación y para crear los recursos que se definen en esta. Para obtener más información, vea el artículo sobre la [creación de archivos de plantilla de implementación](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template).

    -   createUIDefinition.json: Azure Portal usa este archivo para generar la interfaz de usuario para el aprovisionamiento de esta solución o aplicación. Para obtener más información, consulte [Creación de la interfaz de usuario de Azure Portal para una aplicación administrada](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview).

    >[!IMPORTANT]
    >Este paquete debe contener todos los demás scripts o plantillas anidadas que sean necesarios para aprovisionar esta aplicación. La carpeta raíz debe contener los archivos mainTemplate.json y createUiDefinition.json.

## <a name="to-configure-the-marketplace"></a>Para configurar el Marketplace

Use la vista del Marketplace para configurar los campos que se muestran para la oferta en [Azure Marketplace](https://azuremarketplace.microsoft.com) y en [Azure Portal](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Preview Subscription Ids (Id. de suscripción de versión preliminar)

Lista de identificadores de suscripciones de Azure a los que le gustaría conceder acceso a la oferta una vez publicada. Estas suscripciones permitidas le dan la posibilidad de probar la oferta en versión preliminar antes de publicarla. En el portal de asociados puede habilitar como permitidas hasta 100 suscripciones.

### <a name="suggested-categories"></a>Suggested Categories (Categorías sugeridas)

Seleccione hasta cinco categorías de la lista proporcionada con las que la oferta puede asociarse mejor. Las categorías seleccionadas se usarán para asociar la oferta con las categorías de productos disponibles en [Azure Marketplace](https://azuremarketplace.microsoft.com) y [Azure Portal](https://portal.azure.com/).

Los ejemplos siguientes muestran información del Marketplace en Azure Marketplace y en Azure Portal.

**Azure Marketplace**

![publishvm10](./media/cloud-partner-portal-publish-managed-app/publishvm10.png)


![publishvm11](./media/cloud-partner-portal-publish-managed-app/publishvm11.png)


![publishvm15](./media/cloud-partner-portal-publish-managed-app/publishvm15.png)


**Azure Portal**


![publishvm12](./media/cloud-partner-portal-publish-managed-app/publishvm12.png)


![publishvm13](./media/cloud-partner-portal-publish-managed-app/publishvm13.png)


### <a name="logo-guidelines"></a>Directrices para logotipos

Siga estas directrices para los logotipos cargados en Cloud Partner Portal:

-   El diseño de Azure tiene una paleta de colores simple. Utilice pocos colores primarios y secundarios en el logotipo.

-   Los colores del tema del Portal de Azure son el blanco y el negro. Evite emplear estos colores como fondo de los logotipos. Utilice un color que permita destacar los logotipos en Azure Portal. Nosotros recomendamos usar colores primarios simples.

    >[!Note] 
    >Si utiliza un fondo transparente, asegúrese de que el texto y los logotipos no sean de color azul, blanco o negro.

-   No utilice un fondo degradado en el logotipo.

-   Evite colocar texto en el logotipo. Esto incluye el nombre de su empresa o marca. El aspecto del logotipo debe ser *plano* y debe evitar degradados.

-   El logotipo no se debe ajustar.

#### <a name="hero-logo"></a>Logotipo de imagen prominente

El logotipo de imagen prominente es opcional. El publicador puede decidir no cargar un logotipo de imagen prominente. Sin embargo, el logotipo no se puede eliminar una vez cargado. El asociado debe seguir las directrices de Azure Marketplace para imágenes prominentes.

#### <a name="guidelines-for-the-hero-logo-icon"></a>Instrucciones para el logotipo de imagen prominente

-   El nombre para mostrar del anunciante, el título del plan y el resumen largo de la oferta se muestran con una fuente de color blanco. Evite usar colores claros en el fondo. Los fondos transparentes y de color negro o blanco no pueden usarse en las imágenes prominentes.

-   El nombre para mostrar del anunciante, el título del plan, el resumen largo de la oferta y el botón Crear se incrustan mediante programación dentro del logotipo de imagen prominente cuando se publica la oferta. No escriba ningún texto cuando diseñe el logotipo de imagen prominente. Deje un espacio en blanco a la derecha del logotipo. Este espacio debe ser de 415 x 100 píxeles y estar desplazado 370 píxeles de la izquierda.

![publishvm14](./media/cloud-partner-portal-publish-managed-app/publishvm14.png)

## <a name="to-configure-support"></a>Procedimiento para configurar el soporte técnico

Use la vista de Soporte técnico para proporcionar la siguiente información:

- Contactos de soporte técnico de su empresa, como, por ejemplo, de ingeniería.
- Contactos de asistencia al cliente.

## <a name="to-publish-the-offer"></a>Procedimiento para publicar la oferta

El último paso es publicar la oferta. Seleccione **Publicar**.
