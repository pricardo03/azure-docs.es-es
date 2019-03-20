---
title: Configuración técnica para una oferta de aplicación SaaS de Azure | Microsoft Docs
description: Configure la información técnica para la oferta de aplicación SaaS en Azure Marketplace.
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
ms.openlocfilehash: 891d9b7b34e3d30efb46b69ef1aa75566fe634c4
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58120761"
---
# <a name="saas-application-technical-info-tab"></a>Pestaña Información técnica de aplicaciones SaaS

La pestaña Información técnica proporciona el formulario Configuración técnica. Use este formulario para seleccionar el tipo de aplicación de SaaS que va a crear y configure cómo la aplicación se proporciona a los clientes.

![Formulario Configuración técnica](./media/saas-techinfo-techconfig.png)

## <a name="technical-configuration-form"></a>Formulario Configuración técnica

Este formulario tiene 2 campos: Producto y Call to action (Llamada a la acción).

### <a name="product-field"></a>Campo Producto

Puede proporcionar una aplicación SaaS para ambos de los escaparates siguientes:
- Para un usuario profesional, seleccionando la opción **Listado**.
- Para un usuario administrador de TI, seleccionando **Sell through Microsoft** (Venta mediante Microsoft).
Para ayudarle a decidir qué tipo de aplicación SaaS está compilando, consulte [Comprender la selección de escaparate](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type#understand-storefront-selection).

#### <a name="sell-through-microsoft"></a>Venta mediante Microsoft
Para crear esta experiencia, es preciso configurar las siguientes partes:

- Conecte el sitio web del servicio SaaS con las API SaaS de Microsoft. En el artículo [SaaS Sell through Azure - APIs](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-saas-subscription-apis) (Venta de SaaS mediante Azure: API) se explica cómo crear la conexión.
- Habilite venta mediante Azure en Cloud Partner Portal en el formulario Configuración técnica y la información necesaria. Para más información sobre este modelo de facturación y cómo se implementa, consulte [SaaS: vender mediante Azure](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-saas-offer-subscriptions).

  ![Formulario de venta mediante Microsoft](./media/saas-techinfo-sellthrough-ms.png)

En la tabla siguiente se describen los campos obligatorios de Sell through Microsoft (Venta mediante Microsoft).

|  **Nombre del campo**   |  **Descripción**  |
|  ---------------  |  ---------------  |
|    Id. de suscripción de versión preliminar               |    Todos los identificadores de suscripción de Azure que se usan para probar la oferta en una versión preliminar antes de que esté disponible públicamente.               |
|     Instrucciones de introducción              |   Instrucciones que compartir con sus clientes para ayudarles a conectar con la aplicación SaaS. Se permiten las etiquetas HTML básicas, por ejemplo: &lt;p&gt;, &lt;h1&gt;, &lt;li&gt;, etc.                |
|    URL de página de aterrizaje  |   La dirección URL del sitio a la que se puede dirigir a los clientes después de hacer una adquisición en Azure Portal. Esta URL también será el punto de conexión que recibirá las API de conexión para facilitar el comercio con Microsoft.                |
|  Webhook de conexión    |  Para todos los eventos asincrónicos que Microsoft necesita enviarle en nombre del cliente (ejemplo: la Suscripción de Azure se ha vuelto no válida), le pedimos que nos proporcione un webhook de conexión. Si aún no tiene un sistema de webhooks activo, la configuración más sencilla es una aplicación de lógica de punto de conexión HTTP que escuchará los eventos que se publiquen y los gestionará adecuadamente. Para más información, consulte <a href="https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint">Llamada, desencadenamiento o anidamiento de flujos de trabajo con puntos de conexión HTTP en aplicaciones lógicas</a>.                |
|  Identificador del inquilino de Azure AD e ID de la aplicación      |   En Azure Portal, es necesario crear una aplicación de Active Directory para que podamos validar que la conexión entre nuestros dos servicios usa una comunicación autenticada. Para estos campos, cree una aplicación de AD y péguela en el id. de inquilino y el id. de aplicación correspondientes. Tenga en cuenta que el identificador de la aplicación está asociado a publisherID. Por lo tanto, asegúrese de que usa el mismo identificador de la aplicación en todas las ofertas.             |


Por último, si selecciona **Sell through Microsoft**, hay otra pestaña Nueva oferta denominada **Planes**. 

En la [pestaña de planes](./cpp-plans-tab.md) se enumeran los planes específicos y los precios correspondientes que admite la aplicación SaaS. En este momento, se permiten los precios mensuales, con acceso libre para uno o tres meses. Estos planes y precios deben coincidir exactamente con los planes y los precios que tiene en su propio sitio de la aplicación de SaaS.

>[!NOTE] 
>Los planes solo son necesarios si elige Sell through Microsoft.

### <a name="call-to-action-field"></a>Campo Call to action

El campo Call to action (Llamada a la acción) le permite elegir el mensaje que aparece en el botón de adquisición de su oferta. Están disponibles las siguientes opciones:

- Gratis: Si elige esta opción, se le pedirá que escriba una dirección URL de prueba donde los clientes pueden acceder a la aplicación SaaS. Por ejemplo: https://contoso.com/trial
- Evaluación gratuita: Si elige esta opción, se le pedirá que escriba una dirección URL de prueba donde los clientes pueden acceder a la aplicación SaaS. Por ejemplo: https://contoso.com/trial
- Ponerse en contacto conmigo

Para más información sobre las opciones de Call to action, consulte Selección de la opción de publicación.

>[!Note]
>Canal de asociados de cloud Solution Providers (CSP) participar en ahora disponible.  Consulte [Cloud Solution Providers](../../cloud-solution-providers.md) para obtener más información sobre la oferta a través de Microsoft CSP de marketing de canales asociado.

## <a name="next-steps"></a>Pasos siguientes

- [Pestaña Planes (opcional)](./cpp-plans-tab.md)
- [Pestaña Información del canal](./cpp-channel-info-tab.md)
