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
ms.openlocfilehash: e0e1311276b858f1ac16fe6f17ccad49dd9901c9
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55665936"
---
# <a name="create-a-power-bi-application-offer"></a>Creación de una oferta de aplicación de Power BI

En esta sección se enumeran los pasos necesarios para crear una nueva oferta de aplicación de Power BI para [AppSource](https://appsource.microsoft.com). Cada oferta aparece como su propia entidad en AppSource.  Cuando se crea una nueva oferta en [Cloud Partner Portal](https://cloudpartner.azure.com/), deben proporcionarse cuatro grupos de recursos para la oferta.

|   Grupo de recursos      | DESCRIPCIÓN                                                                         |
| ----------------   | ----------------                                                                    |
| Configuración de oferta     | Identificaciones principales y nombre de la oferta                                      |
| Información técnica     | Dirección URL del instalador usado para instalar la aplicación en el área de trabajo de Power BI del cliente. Para más información sobre cómo generar esta dirección URL, consulte la [documentación de la aplicación de Power BI](https://go.microsoft.com/fwlink/?linkid=2028636).   |
| Detalles del escaparate | Contiene recursos de marketing, legales y de administración de clientes potenciales. Los recursos de marketing incluyen la descripción y los logotipos de la oferta.  Los recursos legales incluyen una directiva de privacidad, términos de uso y otra documentación legal.  La directiva de administración de clientes potenciales permite especificar cómo se controlan los clientes potenciales desde el portal del usuario final de AppSource. |
| Contactos           | Contiene información de contacto y directivas de soporte técnico                                     |
|    |     |


## <a name="new-offer-form"></a>Formulario Nueva oferta

Una vez que inicie sesión en Cloud Partner Portal, haga clic en el elemento **+ Nueva oferta** de la barra de menús de la izquierda.  En el menú que aparece, haga clic en **Power BI Apps** (Aplicaciones de Power BI) para mostrar el formulario **Nueva oferta** e iniciar el proceso de definición de recursos para una nueva oferta de aplicación.

![Elemento de menú de oferta de Power BI](./media/new-offer-menu.png)

> [!WARNING] 
> Si no se muestra la opción **Power BI Apps** o no está habilitada, la cuenta no tiene permiso para crear este tipo de oferta. Compruebe que cumple todos los [requisitos previos](./cpp-prerequisites.md) para este tipo de oferta, incluido el registro para una cuenta de desarrollador.


## <a name="next-steps"></a>Pasos siguientes

En los artículos siguientes de esta sección se reflejan las pestañas de la página **Nueva oferta** (para un tipo de oferta de aplicación de Power BI). En cada artículo se explica cómo usar la pestaña asociada para definir los grupos de recursos y servicios complementarios para la nueva oferta de aplicación.

-  [Pestaña Configuración de la oferta](./cpp-offer-settings-tab.md)
-  [Pestaña Información técnica](./cpp-technical-info-tab.md)
-  [Pestaña Detalles del escaparate](./cpp-storefront-details-tab.md)
-  [Contactos](./cpp-contacts-tab.md)
