---
title: 'Configuración de una oferta de la aplicación Power BI: Azure Marketplace | Microsoft Docs'
description: Configurar opciones de la oferta de una oferta de aplicación de Power BI para el catálogo de soluciones de Microsoft AppSource.
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
ms.openlocfilehash: 49bbe5dcf17a9aa20cb47f477c59e7115d29dacc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60727952"
---
# <a name="power-bi-apps-offer-settings-tab"></a>Pestaña Configuración de oferta de aplicaciones Power BI

Al abrir el **nueva oferta** página para las aplicaciones de servicio, consulte primero la **configuración de la oferta** ficha. Proporcione los identificadores principales y el nombre de la oferta en esta pestaña. Un asterisco (*) indica un campo obligatorio.

![Pestaña Configuración de la oferta](./media/offer-settings-tab.png)


## <a name="offer-settings-fields"></a>Campos de Configuración de oferta 

En el **configuración de la oferta** ficha, debe escribir información en los siguientes campos obligatorios:

|  Campo        |  DESCRIPCIÓN                                                               |
|---------------|----------------------------------------------------------------------------|
| **Id. de oferta**  | Identificador único (en el perfil de un publicador) de la oferta. Este identificador será visible en las direcciones URL de producto, las plantillas de Azure Resource Manager y los informes de facturación. La longitud máxima es de 50 caracteres. Puede contener solo caracteres alfanuméricos en minúscula y guiones (-). Tampoco puede terminar con un guión. Este identificador no puede cambiarse una vez lanza una oferta. Si Contoso publica una oferta con Id. de oferta `sample-SvcApp`, la oferta se asigna la dirección URL de AppSource `https://appsource.microsoft.com/marketplace/apps/contoso.sample-SvcApp`.      |
| **Publicador** | Identificador único de la organización en [AppSource](https://appsource.microsoft.com). Todas las ofertas se deben asociar al identificador de publicador. No se puede cambiar este valor después de guardar la oferta.                         |
| **Nombre**      | Un nombre para mostrar de la oferta. Este nombre aparecerá en AppSource y en Cloud Partner Portal. La longitud máxima es de 50 caracteres. Use un nombre de marca que sea reconocible para su producto. No incluya aquí el nombre de su organización, a menos que la aplicación se comercializa con ese nombre. Si está proporcionando esta oferta en otros sitios Web y las publicaciones, use el mismo nombre en todas las publicaciones.    <br/>Si libera una oferta durante el período de vista previa para aplicaciones de Power BI, agregue la cadena `(Preview)` al final del nombre de la aplicación, similar al siguiente: `Sample Scv App (Preview)`. |
|     |     |


## <a name="next-steps"></a>Pasos siguientes

En la pestaña siguiente, especificará [información técnica](./cpp-technical-info-tab.md) para su oferta.
