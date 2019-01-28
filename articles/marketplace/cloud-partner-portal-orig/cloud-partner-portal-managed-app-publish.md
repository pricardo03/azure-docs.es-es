---
title: Publicar una aplicación administrada de Azure en Azure Marketplace
description: Publicar una aplicación administrada de Azure en Azure Marketplace
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: pbutlerm
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 6e5bcd6d9923b4051d44d51ff1a2534bc5e02f41
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54445667"
---
<a name="publish-an-azure-managed-application-to-azure-marketplace"></a>Publicar una aplicación administrada en Azure Marketplace 
========================================================

En este artículo se enumeran los distintos pasos necesarios para publicar una oferta de aplicación administrada en Azure Marketplace.

<a name="pre-requisites-for-publishing-a-managed-application"></a>Requisitos previos para publicar una aplicación administrada 
---------------------------------------------------

Requisitos previos para publicar en Azure Marketplace

1.  Requisitos previos técnicos

    -   [Creación de plantillas del Administrador de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)

    -   Plantillas de inicio rápido de Azure:

        -   <https://azure.microsoft.com/documentation/templates/>

        -   <https://github.com/azure/azure-quickstart-templates>

    -   Crear definición de UI

        -   [Crear un archivo de definición de la interfaz de usuario](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)

2.  Requisitos previos no técnicos (empresariales)

    -   La empresa o sus subsidiarias deben encontrarse en uno de los países de origen de venta que admite Azure Marketplace.

    -   El producto debe contar con una licencia que sea compatible con los modelos de facturación admitidos por Azure Marketplace.

    -   Usted es responsable de proporcionar soporte técnico a sus clientes: gratuito, de pago o mediante el soporte técnico de la comunidad.

    -   Asimismo, es responsable de la concesión de licencias para su software y de las dependencias de software de terceros.

    -   Debe proporcionar contenido que cumpla los criterios para que su oferta se publique en Azure Marketplace y en el Portal de administración de Azure.

    -   Debe aceptar los términos del acuerdo del publicador y de las directivas de participación de Azure Marketplace.

    -   También debe aceptar los Términos de uso, la Declaración de privacidad de Microsoft y el Contrato del programa Microsoft Azure Certified.

<a name="how-to-create-a-new-azure-application-offer"></a>Cómo crear una oferta de aplicación de Azure 
-------------------------------------------

Una vez satisfechos los requisitos técnicos, estará listo para empezar a crear la oferta de aplicación administrada. Antes de nada, un repaso rápido a los conceptos de oferta y SKU.

**Oferta**

Una oferta de aplicación de Azure se corresponde con una clase de oferta de producto de un publicador. Si tiene una nueva solución o aplicación que quiere publicar en Azure Marketplace, una nueva oferta es la mejor opción. Una oferta es una colección de SKU. Cada oferta aparece como su propia entidad en Azure Marketplace.

**SKU**

Una SKU es una oferta con la unidad de compra mínima. Dentro de la misma clase de producto (oferta), las SKU permiten distinguir entre las diferentes características admitidas, independientemente de si la oferta es administrada o no administrada y de los modelos de facturación.

Agregue varias SKU si desea admitir diferentes modelos de facturación, como traiga su propia licencia (BYOL), pago por uso, etc. 

Agregue varias SKU si cada SKU admite un conjunto de características distinto y si cada una de ellas tiene precios diferentes.

Una SKU se presenta en la oferta principal de Azure Marketplace, aunque se muestra como entidad de compra propia en azure.com.

1.  Inicie sesión en [Cloud Partner Portal](http://cloudpartner.azure.com).

2.  En la barra de navegación izquierda, haga clic en \"+ Nueva oferta\" y seleccione \"Aplicaciones de Azure\".

    ![Nueva oferta](./media/cloud-partner-portal-publish-managed-app/newOffer.png)

3.  Ahora se abre una vista del \"Editor\" para una nueva oferta, donde puede empezar a crearla.

4.  Los \"formularios\" que se tienen que rellenar aparecen a la izquierda en la vista del \"Editor\". Cada \"formulario\" consta de un conjunto de campos que hay que rellenar. Los campos obligatorios están marcados con un asterisco rojo (\*).

    > Hay cuatro formularios principales para crear una aplicación administrada.

    -   Configuración de oferta

    -   SKU

    -   Marketplace

    -   Soporte técnico

<a name="how-to-fill-out-the-offer-settings-form"></a>Cumplimentación del formulario de configuración de oferta 
---------------------------------------

El formulario de configuración de oferta es un formulario básico en el que se especifica la configuración de la oferta.
Los diferentes campos se describen a continuación.

**Id. de oferta**

Identificador único de la oferta en un perfil del anunciante.
Estará visible en direcciones URL de producto, plantillas de Resource Manager e informes de facturación. Puede contener solo caracteres alfanuméricos en minúscula o guiones (-). No puede terminar con un guion ni superar los 50 caracteres. Este campo queda bloqueado en cuanto se lanza una oferta.

**Id. del editor**

Esta lista desplegable permite elegir el perfil del publicador en el que desea publicar esta oferta. Este campo queda bloqueado en cuanto se lanza una oferta.

**Nombre**

Nombre para mostrar de la oferta. Nombre que se mostrará en Azure Marketplace y en Azure Portal. Puede tener un máximo de 50 caracteres. Aquí se recomienda incluir un nombre de marca que identifique el producto. No incluya aquí el nombre de su empresa a menos que sea así como se comercializa. Si comercializa esta oferta en su propio sitio web, asegúrese de que el nombre sea exactamente el mismo que aparece en el sitio web.

Haga clic en \"Guardar\" para guardar lo que ha hecho hasta ahora. El siguiente paso consistiría en agregar SKU para la oferta.

<a name="how-to-create-skus"></a>Creación de SKU 
------------------

Haga clic en el formulario \"SKU\". Aquí verá una opción para \"Agregar una SKU\". Si hace clic en ella, podrá escribir un \"Id. de SKU\".

![SKU de nueva oferta](./media/cloud-partner-portal-publish-managed-app/newOffer_skus.png)

El \"identificador de SKU\" es el identificador único de la SKU dentro de una oferta. Este identificador se mostrará en las direcciones URL de producto, las plantillas de ARM e informes de facturación. Puede contener solo caracteres alfanuméricos en minúscula o guiones (-).
No puede terminar con un guion ni superar los 50 caracteres. Este campo queda bloqueado en cuanto se lanza una oferta. Puede tener varias SKU dentro de una oferta. Necesita una SKU para cada imagen que pretende publicar.

Después de agregar una SKU, esta aparecerá en la lista de SKU dentro del formulario \"SKU\". Haga clic en el nombre de la SKU para obtener detalles sobre dicha SKU específica. A continuación se exponen algunos detalles de varios campos.

Después de hacer clic en \"Nueva SKU\", debe completar el formulario siguiente:

![Nueva oferta: nueva SKU](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku.png)

### <a name="how-to-fill-sku-details-section"></a>Cómo completar la sección de detalles de la SKU 

**Título**: proporciona un título para esta SKU. Esto es lo que se muestra en la galería para este elemento.

**Resumen**: proporciona un breve resumen para esta SKU. Este texto aparece justo debajo del título.

**Descripción**: proporciona una descripción detallada de la SKU.

**Tipo de SKU**: los valores permitidos son \"Aplicación administrada\" y \"Plantillas de solución\". En este caso, seleccionará \"Aplicación administrada\".

### <a name="how-to-fill-package-details-section"></a>Cómo rellenar la sección de detalles del paquete 

En la sección del paquete debe rellenar los campos que se indican a continuación.

![Nueva oferta: nuevo paquete de SKU](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku_package.png)

**Versión actual**: proporciona una versión para el paquete que cargará.
Debe tener el formato correcto.

**Archivo de paquete**: este paquete contiene los siguientes archivos comprimidos en un archivo .zip.

applianceMainTemplate.json: archivo de plantilla de implementación que se usa para implementar la solución o la aplicación, y crear los recursos que se definen en esta. Puede encontrar más detalles sobre cómo crear archivos de plantilla de implementación aquí: <https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template>.

appliancecreateUIDefinition.json: el portal de azure.com usa este archivo para generar la interfaz de usuario para el aprovisionamiento de esta solución o aplicación. Puede encontrar más detalles sobre cómo crear este archivo aquí: <https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview>.

mainTemplate.json: archivo de plantilla que solo contiene el recurso Microsoft.Solution/appliances. A continuación se indican las principales propiedades de este recurso que debe tener en cuenta.

-   \"kind\": el valor debe ser \"Marketplace\" en el caso de un escenario de aplicación administrada de Marketplace.
-   \"ManagedResourceGroupId\": grupo de recursos de la suscripción del cliente donde se implementarán todos los recursos definidos en applianceMainTemplate.json.
-   \"PublisherPackageId\": cadena que identifica el paquete de forma única. 

El valor se debe generar como se indica a continuación: es una concatenación de \[publisherId\].\[OfferId\]-preview\[SKUID\].\[PackageVersion\].
Cada uno de estos valores se puede obtener tal como se muestra a continuación.

Este paquete debe contener todos los demás scripts o plantillas anidadas que sean necesarios para aprovisionar correctamente esta aplicación. Los archivos mainTemplate.json, applianceMainTemplate.json y applianceCreateUIDefinition.json deben estar presentes en la carpeta raíz.

**Authorizations**: esta propiedad define quién obtiene acceso y qué nivel de acceso en los recursos de las suscripciones de los clientes. Esto permite que el publicador administre la aplicación en nombre del cliente.

-   PrincipalId: identificador de Azure Active Directory de un usuario, de un grupo de usuarios o de una aplicación a la que se concederán determinados permisos (como se describe en la definición de roles) en los recursos de la suscripción del cliente.

-   Role Definition: lista de todos los roles RBAC integrados que proporciona Azure AD. Puede seleccionar el rol más adecuado, lo que le permitirá administrar los recursos en nombre del cliente.

Se pueden agregar varias autorizaciones. Aun así, se recomienda crear un grupo de usuarios de Active Directory y especificar su identificador en \"PrincipalId\..  Esto permite agregar más usuarios al grupo sin necesidad de actualizar la SKU.

Puede encontrar más detalles sobre RBAC aquí: <https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is>.

<a name="marketplace-form"></a>Formulario de Marketplace
----------------

El formulario de Marketplace dentro de una oferta de aplicación de Azure solicita campos que aparecerán en [Azure Marketplace](https://azuremarketplace.microsoft.com) y en [Azure Portal](https://portal.azure.com/). A continuación se exponen algunos detalles de varios campos.

#### <a name="preview-subscription-ids"></a>Id. de suscripción de versión preliminar

Escriba aquí una lista de identificadores de suscripciones de Azure a los que le gustaría conceder acceso a la oferta una vez publicada. Estas suscripciones permitidas le dan la posibilidad de probar la oferta en versión preliminar antes de publicarla. En el portal de asociados puede habilitar como permitidas hasta 100 suscripciones.

#### <a name="suggested-categories"></a>Suggested Categories (Categorías sugeridas)

Seleccione hasta cinco categorías de la lista proporcionada con las que la oferta puede asociarse mejor. Las categorías seleccionadas se usarán para asociar la oferta con las categorías de productos disponibles en [Azure Marketplace](https://azuremarketplace.microsoft.com) y [Azure Portal](https://portal.azure.com/).

Estas son algunas de las ubicaciones en que se muestran los datos proporcionados en este formulario.

##### <a name="azure-marketplace"></a>Azure Marketplace

![publishvm10](./media/cloud-partner-portal-publish-managed-app/publishvm10.png)

![publishvm11](./media/cloud-partner-portal-publish-managed-app/publishvm11.png)

![publishvm15](./media/cloud-partner-portal-publish-managed-app/publishvm15.png)

##### <a name="portal-at-azurecom"></a>Portal de azure.com

![publishvm12](./media/cloud-partner-portal-publish-managed-app/publishvm12.png)

![publishvm13](./media/cloud-partner-portal-publish-managed-app/publishvm13.png)

##### <a name="logo-guidelines"></a>Directrices para logotipos

Todos los logotipos cargados en Cloud Partner Portal deberían seguir estas directrices:

-   El diseño de Azure tiene una paleta de colores simple. Utilice pocos colores primarios y secundarios en el logotipo.

-   Los colores del tema del portal de azure.com son el blanco y el negro. Evite emplear estos colores como fondo de los logotipos. Use un color que haga que los logotipos destaquen en el portal de azure.com.
    Nosotros recomendamos usar colores primarios simples. **Si utiliza un fondo transparente, asegúrese de que el texto y los logotipos no sean de color azul, blanco ni negro.**

-   No utilice un fondo degradado en el logotipo.

-   Evite colocar texto, incluso la empresa o el nombre de marca, en el logotipo.
    El aspecto del logotipo debe ser \'plano\' y debe evitar degradados.

-   Evite el estiramiento el logotipo.

##### <a name="hero-logo"></a>Logotipo de imagen prominente

El logotipo de imagen prominente es opcional. El publicador puede decidir no cargar un logotipo de imagen prominente. Sin embargo, una vez que cargue la imagen prominente, esta no se puede eliminar. En tal caso, el asociado debe seguir las instrucciones de Azure Marketplace para imágenes prominentes.

###### <a name="guidelines-for-the-hero-logo-icon"></a>Instrucciones para el logotipo de imagen prominente

-   El nombre para mostrar del publicador, el título del plan y el resumen largo de la oferta se muestran en color blanco. Debe evitar usar colores claros en el fondo de la imagen prominente. Los fondos transparentes y de color negro y blanco no pueden usarse en las imágenes prominentes.

-   Cuando se publica la oferta, el nombre para mostrar del editor, el título del plan, el resumen largo de la oferta y el botón Crear se incrustan mediante programación dentro del logotipo de imagen prominente. No hace falta especificarlos cuando diseñe el logotipo de imagen prominente. Deje espacios en blanco a la derecha para el nombre del editor, el título de plan, el resumen largo de la oferta, etc. Se incluyen mediante programación.
    Los espacios en blanco para el texto deben tener un tamaño de 415 x 100 a la derecha (con un desplazamiento de 370 píxeles a la izquierda).

![publishvm14](./media/cloud-partner-portal-publish-managed-app/publishvm14.png)

<a name="support-form"></a>Formulario de soporte técnico
------------

El siguiente formulario que debe rellenar es el de soporte técnico. En este formulario se solicitan contactos de soporte técnico de su empresa.  Algunos ejemplos son los datos de contacto del departamento de ingeniería y los de soporte técnico al cliente.

<a name="how-to-publish-an-offer"></a>Publicación de una oferta
-----------------------

Ahora que ya ha elaborado la oferta, el siguiente paso es publicarla en Azure Marketplace. Haga clic en el botón \"Publicar\" para iniciar el proceso de publicación de la oferta.
