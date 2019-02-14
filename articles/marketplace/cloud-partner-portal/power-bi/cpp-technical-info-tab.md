---
title: 'Información técnica para una oferta de la aplicación Power BI: Azure Marketplace | Microsoft Docs'
description: Configure los campos de información técnica para una oferta de la aplicación Power BI para Microsoft AppSource Marketplace.
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
ms.openlocfilehash: d96ef2fd318d6164e1b7dfc5c4b72d6957af0f3e
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55744441"
---
# <a name="power-bi-apps-technical-info-tab"></a>Pestaña Información técnica de aplicaciones de Power BI

La pestaña **Información técnica** de la página **Nueva oferta** es donde debe proporcionar la dirección URL del paquete del instalador de Power BI y toda la información adicional necesaria para la validación de la nueva oferta.  Para la versión inicial, todas las aplicaciones de Power BI son gratuitas, están disponibles para su descarga desde AppSource sin ningún cargo adicional. Como consecuencia, no podrá definir ninguna referencia de almacén (SKU) para este tipo de oferta.

![Pestaña Información técnica](./media/technical-info-tab.png)


## <a name="technical-info-fields"></a>Campos de Información técnica 

En la pestaña **Información técnica**, debe proporcionar los siguientes campos.  Un asterisco (*) anexo a la etiqueta del campo indica que es obligatorio.

|        Campo          |  DESCRIPCIÓN                                                                 |
|    ---------------    |  ----------------------------------------------------------------------------|
| **Installer URL** (Dirección URL del instalador)     | La dirección generada por Power BI al publicar la aplicación y promoverla a producción.  Para más información sobre cómo generar la dirección URL, consulte cómo [publicar aplicaciones de servicio en Power BI](https://docs.microsoft.com/power-bi/service-create-distribute-apps).  |
|  **Validation instructions** (Instrucciones de validación)  |  Instrucciones de texto opcional (máx. de 3000 caracteres) para el equipo de validación de Microsoft con el fin de ayudar a configurar, conectar y probar la aplicación, incluidas: opciones de configuración típica, cuentas de prueba o parámetros que pueden usarse para probar la opción "Conectar datos", etc. Esta información solo estará visible para el equipo de validación y solo se usará para la validación.  |
| **¿Se ha creado esta aplicación como un paquete de contenido de Power BI?** | Actualmente, este es un campo que se usa internamente. Deje el valor establecido en su valor predeterminado, `No`; de lo contrario, el cambio de este campo a `Yes` podría impedir la publicación.  |  
|  |  |


## <a name="next-steps"></a>Pasos siguientes

En la pestaña [Detalles del escaparate digital](./cpp-storefront-details-tab.md) siguiente, proporcionará la información de marketing y legal sobre la aplicación.

