---
title: 'Creación de una oferta de aplicación de Power BI: Azure Marketplace | Microsoft Docs'
description: Cómo crear una oferta de aplicación de Power BI para Microsoft AppSource Marketplace.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
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
ms.date: 01/29/2019
ms.author: pbutlerm
ms.openlocfilehash: 6a4f7daa337618278c3652fad3053c20557a9e28
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62102057"
---
# <a name="create-a-power-bi-app-offer"></a>Creación de una oferta de aplicación de Power BI

En este artículo se enumeran los pasos necesarios para crear una oferta de aplicación de Power BI para [AppSource](https://appsource.microsoft.com). Cada oferta aparece como su propia entidad en AppSource. Al crear una oferta en [Cloud Partner Portal](https://cloudpartner.azure.com/), deben proporcionarse cuatro grupos de recursos para ella.

Los grupos de recursos se describen en la siguiente tabla:

|   Grupo de recursos      | DESCRIPCIÓN                                                                         |
| ----------------   | ----------------                                                                    |
| Configuración de oferta     | Identificaciones principales y nombre de la oferta.                                      |
| Información técnica     | Dirección URL del instalador usado para instalar la aplicación en el área de trabajo de Power BI del cliente. Para más información sobre cómo generar esta dirección URL, consulte la [documentación de la aplicación de Power BI](https://go.microsoft.com/fwlink/?linkid=2028636). |
| Detalles del escaparate | Contiene recursos de marketing, legales y de administración de clientes potenciales. Los recursos de marketing incluyen la descripción y los logotipos de la oferta. Los recursos legales incluyen una directiva de privacidad, términos de uso y otra documentación legal. La directiva de administración de clientes potenciales permite especificar cómo se controlan los clientes potenciales desde el portal del usuario de AppSource. |
| Contactos           | Contiene información de contacto y directivas de soporte técnico.                                     |

## <a name="new-offer-form"></a>Formulario Nueva oferta

Tras haber iniciado sesión en Cloud Partner Portal, seleccione **Nueva oferta** en el panel de la izquierda. Para mostrar el formulario Nueva oferta e iniciar el proceso de definición de recursos para una nueva oferta de aplicación, seleccione **Power BI Apps** (Aplicaciones de Power BI).

![Elemento de menú de oferta de Power BI](./media/new-offer-menu.png)

> [!NOTE] 
> Si no se muestra la opción **Power BI Apps** (Aplicaciones de Power BI) o no está habilitada, la cuenta no tiene permiso para crear este tipo de oferta. Asegúrese de que cumple todos los [requisitos previos](./cpp-prerequisites.md) para este tipo de oferta, incluido el registro para una cuenta de desarrollador.


## <a name="next-steps"></a>Pasos siguientes

En los siguientes artículos se explican las pestañas de la página **Nueva oferta** para un tipo de oferta Aplicación de Power BI. En cada artículo se describen los grupos de recursos y servicios de soporte técnico para la nueva oferta de aplicación de Power BI.

-  [Pestaña Configuración de la oferta](./cpp-offer-settings-tab.md)
-  [Pestaña Información técnica](./cpp-technical-info-tab.md)
-  [Pestaña Detalles del escaparate](./cpp-storefront-details-tab.md)
-  [Pestaña Contactos](./cpp-contacts-tab.md)
