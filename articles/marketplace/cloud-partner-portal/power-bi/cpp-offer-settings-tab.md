---
title: 'Configuración de una oferta de la aplicación Power BI: Azure Marketplace | Microsoft Docs'
description: Configure una oferta de la aplicación Power BI para Microsoft AppSource Marketplace.
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
ms.openlocfilehash: fef2455129d94913c5b51a08c04403834861bb48
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55665816"
---
# <a name="power-bi-apps-offer-settings-tab"></a>Pestaña Configuración de oferta de aplicaciones Power BI

La página **Nueva oferta** para las aplicaciones de servicio se abre por la primera pestaña denominada **Configuración de oferta**.  En esta pestaña, proporcionará los identificadores principales y el nombre de la oferta.  Un asterisco (*) anexo al nombre del campo indica que es obligatorio.

![Pestaña Configuración de la oferta](./media/offer-settings-tab.png)


## <a name="offer-settings-fields"></a>Campos de configuración de oferta 

En la pestaña **Configuración de oferta**, debe proporcionar los siguientes campos obligatorios.

|  Campo        |  DESCRIPCIÓN                                                               |
|---------------|----------------------------------------------------------------------------|
| **Id. de oferta**  | Identificador único (en el perfil de un publicador) de la oferta. Este identificador será visible en las direcciones URL de producto, las plantillas de Resource Manager y los informes de facturación. Tiene una longitud máxima de 50 caracteres, solo puede estar formado por caracteres alfanuméricos en minúscula y guiones (-), pero no puede terminar con un guión. Este campo no se puede modificar una vez lanzada una oferta. Por ejemplo, si Contoso publica una oferta con el Id. de oferta `sample-SvcApp`, se le asigna la dirección URL de AppSource `https://appsource.microsoft.com/marketplace/apps/contoso.sample-SvcApp`.      |
| **Publicador** | Identificador único de la organización en [AppSource](https://appsource.microsoft.com). Todas las ofertas se deben asociar al identificador de publicador. Este valor no se puede modificar una vez que se haya guardado la oferta.                         |
| **Nombre**      | Nombre para mostrar de la oferta. Este nombre se mostrará en AppSource y en Cloud Partner Portal. Puede tener un máximo de 50 caracteres. Aquí se recomienda incluir un nombre de marca que identifique el producto. No incluya aquí el nombre de la organización a menos que sea así como se comercializa la aplicación. Si comercializa esta oferta en otros sitios web y publicaciones, asegúrese de que el nombre sea el mismo en todas las publicaciones.    <br/>Si publica una oferta durante el período de versión preliminar de las aplicaciones Power BI, el modo de versión preliminar, anexe la cadena `(Preview)` al nombre de la aplicación, por ejemplo `Sample Scv App (Preview)`. |
|     |     |


## <a name="next-steps"></a>Pasos siguientes

En la pestaña siguiente, especificará [información técnica](./cpp-technical-info-tab.md) para su oferta.
