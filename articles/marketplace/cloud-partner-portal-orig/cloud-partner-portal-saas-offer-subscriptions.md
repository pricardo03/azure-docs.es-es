---
title: Venta de SaaS mediante Azure | Microsoft Docs
description: Describe el modelo de facturación de suscripción para aplicaciones de SaaS y cómo implementarlo.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
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
ms.openlocfilehash: f193138fbc5e779c3a6671757320d8918e08db46
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48808093"
---
<a name="saas---sell-through-azure"></a>SaaS: vender mediante Azure
=========================

En este artículo se describe el modelo de facturación de suscripción para aplicaciones de SaaS y cómo implementarlo en Cloud Partner Portal.


<a name="overview"></a>Información general
--------

Azure Marketplace le permite publicar y monetizar sus ofertas de SaaS a través de Azure Marketplace. Ahora se puede cobrar a sus clientes directamente mediante la facturación de Azure y solo recibirán una factura de Azure por todos los recursos y servicios que han activado. Para una publicación, las suscripciones de SaaS tienen las siguientes ventajas:

-   **Experiencia coherente de publicación**: si ya publica ofertas de SaaS o cualquier otra oferta en Azure Marketplace, la experiencia de publicación se realiza a través del mismo portal de publicación.
-   **Nuevo escaparate para la detección**: todas las ofertas publicadas serán visibles tanto en el escaparate externo de Azure Marketplace, como en la extensión de Azure Marketplace dentro de Azure Portal.
-   **Facturación integrada**: ahora puede vender en todas las regiones donde se vende Azure y dejar que Azure se encargue de la facturación.
-   **Generación de clientes potenciales integrada**: ahora puede recibir automáticamente clientes potenciales de Azure en el CRM que elija cada vez que un usuario de Azure se suscriba al servicio de SaaS con Azure Marketplace.
-   **Realice una venta conjunta con otros vendedores de Microsoft**: puede tener derecho al uso compartido de clientes potenciales bidireccional, a listas de catálogos prioritarios, y la oportunidad de participar y cerrar acuerdos conjuntos junto con otros vendedores de Microsoft.

<a name="billing-models"></a>Modelos de facturación
--------------

El único modelo de facturación admitido actualmente es un precio mensual por suscripción del servicio de SaaS.

**Nota:** es posible que haya otros modelos de facturación disponibles en el futuro.

Cada oferta de SaaS puede tener uno o varios planes (por ejemplo, Basic o Prémium). Cada plan tiene algunos metadatos básicos asociados, además del precio asociado con el plan.

Tiene control total sobre la definición de un plan. Por ejemplo, ¿qué constituye un plan Basic? Puede definir el plan y proporcionar el texto necesario para describirlo como parte de la publicación del plan.

El precio asociado con el plan es la tarifa plana mensual que los usuarios de Azure pagarán por usar el servicio.

### <a name="what-is-not-supported-today"></a>¿Qué no se admite hoy?

-   Facturación basada en unidades personalizadas; por ejemplo, un precio definido basado en el número de solicitudes.
-   Facturación basada en la asignación de puestos: por ejemplo, puede permitir que los usuarios de Azure compren licencias en función del número de usuarios.

<a name="end-to-end-flow"></a>Flujo integral
---------------

Flujo de oferta de suscripción de SaaS desde la perspectiva del usuario final

**Nota:** esta descripción del flujo asume que ha publicado su oferta de SaaS en Azure Marketplace con el nombre “SaaS de demostración de Contoso”.

![Flujo de usuario de suscripción de SaaS](media/saas-offer-subscription/saas-subscription-user-flow.png)

Un usuario de Azure puede tener las siguientes interacciones con su servicio de SaaS:

-   Detectar el servicio de SaaS en Azure Marketplace
-   Suscribirse a su servicio de SaaS en Azure
-   Navegar hasta el servicio de SaaS desde Azure Portal
-   Crear una cuenta en el servicio de SaaS y administrarla (cambiar el plan o eliminarla) en SaaS
-   Anular la suscripción al servicio de SaaS desde Azure Portal

<a name="discover-your-saas-service-in-azure-marketplace"></a>Detectar el servicio de SaaS en Azure Marketplace
-----------------------------------------------

Cuando los usuarios inicien Azure Marketplace en Azure Portal, verá una categoría denominada “Software como servicio (SaaS)”.

![Detectar SaaS mediante el menú de categoría](media/saas-offer-subscription/saas-category-menu.png)

Los usuarios también pueden buscar el servicio de SaaS.

![Detectar SaaS mediante la búsqueda](media/saas-offer-subscription/saas-cpp-search.png)

Los usuarios pueden ver los detalles del servicio y hacer clic en **Crear**.

![Crear una suscripción a SaaS](media/saas-offer-subscription/saas-create-subscription.png)

### <a name="subscribe-to-saas-service-in-azure"></a>Suscribirse a un servicio de SaaS en Azure

A continuación, el usuario puede suscribirse al servicio de SaaS de Azure.

![Suscribirse a un servicio de SaaS](media/saas-offer-subscription/saas-subscribe-signup.png)

Cuando el usuario se suscribe a su servicio SaaS, proporciona la siguiente información

-   Nombre: el nombre que los usuarios pueden detectar o utilizar para administrar esta instancia de la suscripción de SaaS en Azure.
-   Suscripción: el contexto de la suscripción para la que quieren asociar la facturación para el servicio de SaaS.
-   Plan: el plan de servicio de SaaS al que quieren suscribirse.

El documento Términos de uso proporcionado como parte de la publicación de la oferta también se muestra al usuario antes de que se suscriba al servicio de SaaS.

Ahora, el usuario puede suscribirse al servicio haciendo clic en **Suscribirse**.
Azure envía una notificación en el portal una vez completada la suscripción.

### <a name="navigate-to-the-saas-service-from-azure-portal"></a>Navegar hasta el servicio de SaaS desde Azure Portal

Los usuarios, a continuación, hacen clic en **Ir al recurso** para ver o administrar su instancia de la suscripción a SaaS.

![Ver o administrar la instancia de SaaS](media/saas-offer-subscription/saas-go-to-resource.png)

Se notifica al usuario que debe configurar la cuenta en el servicio de SaaS en primer lugar. Su facturación comenzará después de que el servicio de SaaS notifique a Azure que empiece a facturar, que es cuando el usuario crea una cuenta en el sitio del servicio de SaaS.

![Configurar la instancia de SaaS](media/saas-offer-subscription/saas-configure-account.png)

Cuando el usuario hace clic en **Configurar cuenta**, se le redirige a su punto de conexión de servicio de SaaS. Durante esta redirección, se pasa un token como parámetro de consulta. Por ejemplo: 

![Token de la cuenta de SaaS](media/saas-offer-subscription/saas-account-token.png)

Anote el valor de este token. Este token es de corta duración y debe resolverse para obtener un identificador de suscripción en Azure.

<a name="creating-a-saas-offer-subscription"></a>Crear una suscripción a una oferta de SaaS
----------------------------------

Para crear esta experiencia, se necesitan dos partes de trabajo:

-   Conéctese al sitio web del servicio de SaaS con API de SaaS de Microsoft. El documento [SaaS Sell through Azure - APIs](./cloud-partner-portal-saas-subscription-apis.md) (Venta de SaaS mediante Azure: API) explica cómo crear la conexión.  
-   Habilite la opción **Sell through Azure** (Vender mediante Azure) en Cloud Partner Portal en la sección **Información técnica** y proporcione todos los detalles de configuración.

![Información técnica sobre la nueva oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer-technical-info-2.png)

A continuación se incluye una explicación de todos los campos necesarios para la sección **Información técnica**:

|  **Campos de la oferta**                 |  **Descripción**                                   |
|  ----------------                 |  -------------------------------------             |
| Id. de suscripción de versión preliminar          | Todos los identificadores de suscripción de Azure que se usan para probar la oferta en una versión preliminar antes de que esté disponible públicamente. |
| Instrucciones de introducción      | Instrucciones que compartir con sus clientes para ayudarles a conectar con la aplicación SaaS. Se permite un uso básico de HTML, como las etiquetas siguientes: `<p>`, `<h1>`, `<li>`, etc.  |
| URL de página de aterrizaje                  | La dirección URL del sitio a la que se puede dirigir a los clientes después de hacer una adquisición en Azure Portal. Esta URL también será el punto de conexión que recibirá las API de conexión para facilitar el comercio con Microsoft.  |
| Webhook de conexión                | Para todos los eventos asincrónicos que Microsoft necesita para enviarle en nombre del cliente (por ejemplo, si la suscripción a Azure ya no es válida), requerimos que nos proporcione un webhook de conexión. Si aún no tiene un sistema de webhooks activo, la configuración más sencilla es una aplicación de lógica de punto de conexión HTTP que escuchará los eventos que se publiquen y los gestionará adecuadamente.  Para obtener más información, consulte [Llamada, desencadenamiento o anidamiento de flujos de trabajo con puntos de conexión HTTP en aplicaciones lógicas](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint). |
| Identificador del inquilino de Azure AD e ID de la aplicación     | En Azure Portal, es necesario crear una aplicación de Active Directory para que podamos validar que la conexión entre nuestros dos servicios usa una comunicación autenticada. Para estos campos, cree una aplicación de AD y péguela en el id. de inquilino y el id. de aplicación correspondientes. |
|  |  |


Por último, si selecciona **Sell through Azure** (Vender mediante Azure), hay una sección agregada denominada **Planes**. Los planes solo son necesarios si se selecciona Sell through Azure (Vender mediante Azure). En esta sección se enumeran los planes específicos y los precios correspondientes que admite la aplicación de SaaS. En este momento, se permiten los precios mensuales, con acceso libre para uno o tres meses. Estos planes y precios deben coincidir exactamente con los planes y los precios que tiene en su propio sitio de la aplicación de SaaS.
