---
title: Configuración de escaparates para una oferta de aplicación SaaS de Azure | Microsoft Docs
description: Configure el escaparate para la oferta de aplicación SaaS en Azure Marketplace.
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
ms.date: 12/06/2018
ms.author: pbutlerm
ms.openlocfilehash: 7c4bd326c597b8e989dfc7aac5239ddb2d38376a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60594695"
---
# <a name="saas-application-storefront-details-tab"></a>Pestaña Detalles del escaparate de una aplicación SaaS

En este artículo se muestra cómo usar la pestaña Detalles del escaparate para describir la aplicación SaaS y proporcionar recursos de marketing. Esta pestaña incluye los siguientes formularios: Información general, Artefactos de Marketing, Administración de clientes potenciales e Información legal. 


## <a name="overview"></a>Información general

El formulario Información general tiene los campos obligatorios y opcionales que se muestra en la siguiente captura de pantalla. Un asterisco (*) junto al nombre del campo indica que es obligatorio.

![Formulario de información general del escaparate](./media/saas-storefront-overview.png)

En la tabla siguiente se describe los Detalles del escaparate que puede proporcionar para la oferta.

|  **Campos de la oferta**   |  **Descripción**  |
|  ---------------   |  ---------------  |
|      Resumen de la oferta              |        Resumen de la propuesta de valor de la oferta. Aparecerá en la página de búsqueda de la oferta. Debe tener un máximo de 100 caracteres.           |
|      Descripción de la oferta              |      Descripción que aparecerá en la página de detalles de la aplicación. El número máximo de caracteres permitidos es de 1300. Puede usar etiquetas de marcado HTML básicas para dar formato al contenido. Por ejemplo, &lt;p&gt;, &lt;h1&gt;, &lt;h2&gt; y &lt;li&gt;. Para ver el aspecto que tendrá la descripción con formato, use una herramienta HTML en línea en tiempo real, como [http://htmledit.squarefree.com](https://htmledit.squarefree.com)             |
|          Industrias          |        Seleccione los sectores más afines a la oferta. Si la aplicación está relacionada con varios sectores, puede seleccionar un máximo de dos.           |
|          Categorías sugeridas (3 como máximo)         |        Seleccione las categorías más afines a la oferta. Puede seleccionar un máximo de tres categorías.           |
|           Versión de la aplicación         |        Escriba el número de versión de la aplicación.           |
|          Palabras clave de búsqueda (máximo de 3)          |      Escriba un máximo de tres palabras clave de búsqueda que los clientes pueden usar para buscar la aplicación en el sitio web del escaparate de Marketplace.             |

## <a name="marketing-artifacts"></a>Artefactos de marketing

Use el formulario Artefactos de marketing para identificar recursos de marketing de Azure Marketplace, como logotipos, vídeos, capturas de pantalla y documentos.

![Formulario Artefactos de marketing del escaparate](./media/saas-storefront-artifacts.png)

En la siguiente tabla se describen los campos de Artefactos de marketing.

|  **Campos de la oferta**   |  **Descripción**  |
|  ---------------   |  ---------------  |
|                    |                   |
|                    |                   |
|                    |                   |
|          Logotipos          |        Si se trata de una aplicación SaaS de venta mediante Microsoft, debe proporcionar las imágenes de todos los logotipos. Si es un Listado, se necesitan únicamente dos logotipos. Siga estas directrices para los logotipos cargados en Cloud Partner Portal:<br><ul><li>Utilice pocos colores primarios y secundarios en el logotipo. El diseño de Azure tiene una paleta de colores simple. </li><li>Evite emplear el blanco o el negro como color de fondo del logotipo. Los colores del tema de Azure Portal son el blanco y el negro. En su lugar, utilice algún color que haga destacar a los logotipos en Azure Portal. Nosotros recomendamos usar colores primarios simples. Si va a usar un fondo transparente, asegúrese de que el texto y el logotipo no sean de color azul, blanco o negro. </li><li>No use un fondo degradado en el logotipo. </li><li>Evite colocar texto, incluso la empresa o el nombre de marca, en el logotipo. El aspecto del logotipo debe ser "plano" y debe evitar degradados.</li><li>El logotipo de la imagen no se debe extender.</li></ul>            |
|          Vídeos          |         Le permite agregar vínculos de vídeos de la oferta. Puede usar vínculos a vídeos de YouTube o Vimeo para que acompañen a la oferta para los clientes. También deberá especificar una imagen en miniatura del vídeo, con una imagen PNG de 1280 x 720 píxeles. Puede incluir un máximo de cuatro vídeos por oferta.          |
|          Documentos          |          Le permite agregar documentos de marketing a su oferta. Todos los documentos deben estar en formato PDF y puede tener un máximo de tres documentos por oferta.         |
|        Capturas de pantalla            |         Le permite agregar capturas de pantalla de la oferta. Hay un máximo de cinco capturas de pantalla que se pueden agregar por oferta. El tamaño máximo de la imagen es de 1280 x 720 píxeles.          |
|       Vínculos útiles             |       Le permite agregar direcciones URL externas a su oferta para ayudar a señalar diagramas de arquitectura u otros sitios web que puede que el cliente quiera ver.            |

### <a name="marketing-examples"></a>Ejemplos de marketing

La captura de pantalla siguiente muestra un ejemplo de un resultado de búsqueda en Marketplace.

![Resultados de la búsqueda en Marketplace](./media/saas-marketplace-search-result.png)

En la siguiente imagen se muestra cómo aparece la oferta en Marketplace después de que un cliente haga clic en el icono de la oferta en el resultado de la búsqueda.

![Detalles del resultados de la búsqueda en Marketplace](./media/saas-marketplace-search-result-details.png)

## <a name="lead-management"></a>Administración de clientes potenciales

Para configurar la administración de clientes potenciales, seleccione el **Destino de clientes potenciales** en la lista desplegable. En la siguiente captura de pantalla se muestran los destinos disponibles.

![Administración de clientes potenciales de Detalles del escaparate](./media/saas-storefront-lead-destination.png)

>[!TIP] 
>Seleccione el icono de información para ver este mensaje: “Seleccione el sistema donde se almacenarán los clientes potenciales. Obtenga información sobre cómo conectar su sistema CRM [aquí](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)”.

## <a name="legal"></a>Información legal

Use el formulario Información legal para proporcionar la documentación legal necesaria para cada oferta.

![Formulario Información legal de Detalles del escaparate](./media/saas-storefront-lead-legal.png)

Proporcione la siguiente información:

- Dirección URL de la directiva de privacidad: Escriba un vínculo a la directiva de privacidad de la aplicación.
- Términos de uso: Escriba los términos de uso de la aplicación. Los clientes deben aceptar estos términos antes de poder probar la aplicación.

## <a name="next-steps"></a>Pasos siguientes

[Pestaña Contactos](./cpp-contacts-tab.md)
