---
title: Cómo crear una oferta de Dynamics 365 for Operations a través de Cloud Partner Portal
description: Cómo crear una oferta de Dynamics 365 for Operations a través de Cloud Partner Portal
services: Azure, Marketplace, Cloud Partner Portal,
author: pbutlerm
manager: Ricardo.Villalobos
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: a9ada25641e2a56beb9083b145a507c8fd41a46f
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935104"
---
# <a name="how-to-create-dynamics-365-for-operations-offer-via-cloud-partner-portal"></a>Cómo crear una oferta de Dynamics 365 for Operations a través de Cloud Partner Portal

El portal de publicación proporciona un acceso basado en rol al portal que permite que varias personas puedan colaborar en la publicación de una oferta. Para obtener más información, vea el tema sobre [administración de usuarios en Cloud Portal](./cloud-partner-portal-manage-users.md).

Antes de publicar una oferta en nombre de un anunciante de cuenta, una persona con el rol \"Propietario\" tiene que aceptar los [Términos de uso](https://azure.microsoft.com/support/legal/website-terms-of-use/), la [Declaración de privacidad de Microsoft](https://www.microsoft.com/privacystatement/default.aspx) y el [Contrato del programa Microsoft Azure Certified](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).

## <a name="how-to-create-a-new-dynamics-365-for-operations-offer"></a>Cómo crear una nueva oferta de Dynamics 365 for Operations

Cuando cumpla todos los requisitos previos, ya puede empezar a crear la oferta de Dynamics 365 for Operations.

1. Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2. En la barra de navegación izquierda, haga clic en \"+ Nueva oferta\" y seleccione \"Dynamics 365 for Operations\".
3. Ahora se abre una vista del \"Editor\" para una nueva oferta, donde puede empezar a crearla.
4. Los \"formularios\" que se tienen que rellenar aparecen a la izquierda en la vista del \"Editor\". Cada \"formulario\" consta de un conjunto de campos que hay que rellenar. Los campos obligatorios están marcados con un asterisco rojo (\*).

Hay cuatro formularios principales para crear una oferta de Dynamics 365 for Operations:

- Configuración de oferta
- Información técnica
- Detalles del escaparate
- Contactos

## <a name="how-to-fill-out-the-offer-settings-form"></a>Cumplimentación del formulario de configuración de oferta

El formulario de configuración de oferta es un formulario básico en el que se especifica la configuración de la oferta. Los diferentes campos se describen a continuación.

### <a name="offer-id"></a>Id. de oferta

Se trata de un identificador único de la oferta en un perfil del publicador. Este identificador será visible en las direcciones URL de producto. Puede contener solo caracteres alfanuméricos en minúscula o guiones (-). El identificador, que tendrá 50 caracteres como máximo, no puede terminar con un guion. Este campo queda bloqueado en cuanto se lanza una oferta.

Por ejemplo, si un publicador de contoso publisher crea una oferta con Id. de oferta *dynamics365 de ejemplo para las operaciones*, se mostrará en AppSource como `https://appsource.microsoft.com/marketplace/apps/**contoso**.*sample-dynamics365 for operations*?tab=Overview\`.

### <a name="publisher-id"></a>Id. de publicador

Esta lista desplegable permite elegir el perfil del publicador en el que desea publicar esta oferta. Este campo queda bloqueado en cuanto se lanza una oferta.

NOMBRE

Se trata del nombre para mostrar de la oferta. Este es el nombre que se mostrará en [AppSource](https://appsource.microsoft.com). Puede tener un máximo de 50 caracteres.

Haga clic en \"Guardar\" para guardar lo que ha hecho hasta ahora. El paso siguiente sería rellenar la información técnica de la oferta.

## <a name="how-to-fill-out-the-technical-info-form"></a>Cómo rellenar el formulario de información técnica

El formulario de información técnica contiene información que se mostrará en la página de la oferta. A continuación se proporcionan las instrucciones para los distintos campos.

![Pantalla Nueva oferta](./media/publish_d365_new_offer/Technical_info.png)

### <a name="solution-identifier"></a>Solution Identifier (Identificador de la solución)

El primer campo es Solution Identifier (Identificador de la solución).

1. Para encontrar este identificador, vaya a Life Cycle Services (Servicios de ciclo de vida) y seleccione Administración de soluciones.
2. Después de elegir la solución adecuada, verá el campo Solution Identifier (Identificador de la solución) en la información general del paquete. \*\*Si el identificador está en blanco, seleccione la opción para editar y volver a publicar el paquete para que se muestre el campo Solution Identifier (Identificador de la solución).

### <a name="validation-assets"></a>Validation Assets (Recursos de validación)

Cargue aquí sus informes de análisis de personalización (CAR).

### <a name="does-solution-enable-translations"></a>Does solution enable translation(s)? (¿La solución permite traducciones?)

Seleccione \'Yes\' (Sí) o \'No\'.

### <a name="does-solution-include-localizations"></a>Does solution include Localization(s)? (¿La solución incluye localizaciones?)

Seleccione \'Yes\' (Sí) o \'No\'.

### <a name="product-version"></a>Versión del producto

Seleccione New AX (AX nuevo). Finalmente, haga clic en Guardar.

## <a name="how-to-fill-out-storefront-details-form"></a>Cómo rellenar el formulario Detalles del escaparate.

La primera página es Detalles de la oferta.

1. **Offer summary** (Resumen de la oferta)

    \- Escriba un breve resumen de la solución (100 caracteres como máximo).

2. **Offer description** (Descripción de la oferta)

    \- Escriba una breve descripción de su solución aquí. La descripción debe tener el impacto funcional de la solución y estar directamente alineada con la biblioteca BPM. Por ejemplo, si afirma que tiene las características x, y, z en el contenido de marketing, durante la revisión final nos aseguraremos de estas estén documentadas en la biblioteca BPM en LCS.

![Pantalla Detalles del escaparate](./media/publish_d365_new_offer/offer_details.png)

### <a name="listing-details"></a>Detalles del anuncio

![Pantalla Detalles del escaparate](./media/publish_d365_new_offer/storefront_details.png)

1. **Sectores**: marque dos sectores como máximo entre las opciones proporcionadas.
2. **Categorías**: marque tres categorías como máximo entre las opciones proporcionadas.
3. **Tipo de aplicación**: seleccionar una de las opciones proporcionadas.
4. **Help link for your app** (Vínculo de ayuda de la aplicación): escriba el vínculo de ayuda de su solución.
5. **Supported countries/regions** (Países/regiones admitidos): marque las opciones que desee de la lista proporcionada.
6. **Idiomas admitidos**: marque las opciones que desee de la lista proporcionada.
7. **Versión de la aplicación**: especifique la versión de la solución que se está publicando. (Por ejemplo, 1.0.0.0)
8. **App Release Date** (Fecha de lanzamiento de la aplicación): especifique la fecha de lanzamiento de su solución (mm/dd/aaaa).
9. **Products your app works with** (Productos con los que su aplicación funciona): lista de los productos específicos con los que funciona la aplicación. Puede enumerar tres productos como máximo. Para agregar un producto a la lista, haga clic en el signo más (junto a la opción Nuevo) y se creará un nuevo campo de texto abierto en el que podrá escribir el nombre de un producto con el que funcione la aplicación.
10. **Palabras clave de búsqueda**: escriba términos comunes que los usuarios pueden usar para buscar tu solución en una búsqueda. \*\*Estas palabras clave no se mostrarán en el Marketplace.
11. **Ocultar clave**: esta es una clave que se combinará con la dirección URL de la versión preliminar de la oferta para ocultarla de la vista pública. No es una contraseña. Puede especificar cualquier cadena aquí.

### <a name="marketing-artifacts"></a>Artefactos de marketing

1. El siguiente paso es cargar sus **logotipos** y **capturas de pantalla**. \*\*Tenga en cuenta que los tamaños de todas las cargas y todas las imágenes deben estar en formato PNG.
2. **Demo videos** (Vídeos de demostración): haga clic en \"+ Nuevo\". Cargue un vídeo de demostración de la solución (solo vínculos de YouTube o Vimeo)\*\*. Tenga en cuenta que debe cargar una miniatura del tamaño especificado para que el vídeo aparezca en el almacenamiento provisional.
3. **Documentos**: cargue los documentos relacionados con la solución y recuerde que debe especificar un nombre para el documento.

### <a name="legal"></a>Información legal

Esta información se vinculará a la directiva de privacidad y a los términos de uso. Escriba la URL de la directiva de privacidad de la solución y sus términos de uso. \*\*El cliente podrá ver estas directivas en el portal.

### <a name="customer-support"></a>Asistencia al cliente

Solo sus usuarios verán la dirección URL de soporte técnico en el portal.

### <a name="leads-management"></a>Administración de clientes potenciales

Seleccione un sistema de CRM donde se guardará el cliente potencial. Seleccione \"Azure Table\" aquí si tiene uno de los siguientes sistemas CRM: Salesforce, Marketo o Microsoft Dynamics CRM. El sistema de CRM que seleccione aquí es donde escribiremos los detalles de los usuarios finales que prueben su aplicación en AppSource (clientes potenciales). En función del sistema de CRM que seleccione, haga clic en la dirección URL correspondiente para obtener información sobre cómo completar el siguiente conjunto de campos.

![Detalles de administración de clientes potenciales](./media/publish_d365_new_offer/leads.png)

1. Para la tabla de Azure, haga clic [aquí](https://aka.ms/leadsettingforazuretable)
2. Para Dynamics CRM online, haga clic [aquí](https://aka.ms/leadsettingfordynamicscrm)
3. Para Marketo, haga clic [aquí](https://aka.ms/leadsettingformarketo)
4. Para Salesforce, haga clic [aquí](https://aka.ms/leadsettingforsalesforce)

## <a name="how-to-fill-out-the-contacts-form"></a>Cómo rellenar el formulario de contactos.

Esta información se usará para el soporte técnico y la asistencia al cliente de Microsoft. Especifique el contacto de ingeniería y la asistencia al cliente para su empresa, así como la dirección URL de soporte técnico para su solución. Esta información se usará como un único punto de contacto si Microsoft tiene alguna pregunta sobre la solución, así como para ofrecer asistencia al cliente.

![Pantalla de contactos de la oferta](./media/publish_d365_new_offer/Contacts.png)
