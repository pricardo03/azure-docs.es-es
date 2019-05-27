---
title: Crear oferta de aplicación SaaS de Azure | Azure Marketplace
description: Cómo crear una oferta de aplicación SaaS en Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: pbutlerm
ms.openlocfilehash: 646e9bf844477b3d8e1c4c42fb5956e015805433
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65833497"
---
# <a name="create-a-new-saas-application-offer"></a>Creación de una nueva oferta de aplicación SaaS

En este artículo se explica cómo crear y publicar una entrada de oferta de aplicación SaaS para Azure Marketplace.

> [!IMPORTANT] 
> SaaS ofrecen funcionalidad se está migrando a la [Microsoft Partner Center](https://partner.microsoft.com/dashboard/directory).  Todos los publicadores nuevo deben usar el centro de partners para crear nuevas ofertas de SaaS y administrar ofertas existentes.  Los editores actuales con ofertas de SaaS se están migrando batchwise de Cloud Partner Portal para el centro de partners.  Cloud Partner Portal mostrará mensajes de estado para indicar cuándo se han migrado las ofertas existentes específicas.


## <a name="offer-process"></a>Proceso de la oferta

En el siguiente diagrama se muestra el proceso para crear una oferta de aplicación SaaS.

![Proceso para crear una oferta SaaS](./media/saas-offer-process-overview.png)

## <a name="offer-components"></a>Componentes de la oferta

La oferta de aplicación SaaS consta de cinco secciones que se describen en la tabla siguiente:

|  **Grupo de recursos**   |  **Descripción**  |
|  ---------------   |  ---------------  |
|    Configuración de oferta  |  Use esta opción para configurar una identidad única para la aplicación SaaS.                 |
|  Información técnica    |  Use esta opción para configurar el tipo de solución SaaS y proporcionar los detalles de conexión de la aplicación.                |
|  Información del canal      |   Proporcione información de canal, como materiales GTM y contactos.                |
|  Versión de prueba        |   Sección opcional para definir un servicio que permitirán a los clientes probar su oferta antes de comprarla.                |
|  Detalles del escaparate       | Contiene recursos y especificaciones de marketing, legales y de administración de clientes potenciales.  <ul><li> Los recursos de marketing incluyen el nombre de la oferta, la descripción y los logotipos</li> <li> Los recursos legales incluyen una directiva de privacidad, términos de uso y otra documentación legal</li>  <li> La directiva de administración de clientes potenciales permite especificar cómo se controlan los clientes potenciales desde el portal del usuario final de Azure Marketplace.</li> </ul> |
| Contactos            | Contiene información de contacto y directivas de soporte técnico |

## <a name="new-offer-form"></a>Formulario Nueva oferta

Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/) y seleccione **+ New offer** (+ Nueva oferta) en la barra de menús de la izquierda. En el menú Nueva oferta, seleccione **Aplicaciones SaaS** para mostrar el formulario Nueva oferta y comenzar el proceso de definición de los recursos para una nueva oferta de aplicación SaaS.

![Menú Nueva oferta para aplicaciones SaaS](./media/azure-new-saas-offer.png)

## <a name="next-steps"></a>Pasos siguientes

La página Nueva oferta del tipo de oferta SaaS proporciona un conjunto de pestañas y campos de formulario que se usarán para crear una oferta. En cada uno de los siguientes artículos se explica cómo usar las pestañas para definir los grupos de recursos y servicios auxiliares de la nueva oferta.

- [Pestaña Configuración de la oferta](./cpp-offer-settings-tab.md)
- [Pestaña Información técnica](./cpp-technical-info-tab.md)
- [Pestaña Información del canal](./cpp-channel-info-tab.md)
- [Pestaña Versión de prueba](./cpp-testdrive-tab.md)
- [Pestaña Detalles del escaparate](./cpp-storefront-tab.md)
- [Pestaña Contactos](./cpp-contacts-tab.md)
