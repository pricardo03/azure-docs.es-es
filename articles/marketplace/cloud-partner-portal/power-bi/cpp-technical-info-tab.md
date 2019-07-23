---
title: Información técnica para una oferta de aplicación de Power BI | Azure Marketplace
description: Configure los campos de información técnica para una oferta de la aplicación Power BI para Microsoft AppSource Marketplace.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: pabutler
ms.openlocfilehash: 15f4e2a76724a70c15411dea767cc9bc433e4d4a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64943229"
---
# <a name="power-bi-apps-technical-info-tab"></a>Pestaña Información técnica de aplicaciones de Power BI

En la página **Nueva oferta**, usa la pestaña **Información técnica** para proporcionar la dirección URL del paquete del instalador de Power BI y otra información que necesita para validar la nueva oferta.  Para la versión inicial, todas las aplicaciones de Power BI son gratuitas y están disponibles para su descarga desde AppSource. Por este motivo, no puede definir unidades de referencia de almacén (SKU) para este tipo de oferta.

![Pestaña Información técnica](./media/technical-info-tab.png)


## <a name="technical-info-fields"></a>Campos de Información técnica 

En la pestaña **Información técnica**, complete los campos descritos en la tabla siguiente. Un asterisco (*) al final de la etiqueta de un campo indica que el campo es obligatorio.

|        Campo          |  DESCRIPCIÓN                                                                 |
|    ---------------    |  ----------------------------------------------------------------------------|
| **Installer URL\*** (Dirección URL del instalador)     | Power BI genera esta dirección URL cuando se publica la aplicación y se promueve a producción.  Para más información, consulte [Publicación de aplicaciones con paneles e informes en Power BI](https://docs.microsoft.com/power-bi/service-create-distribute-apps).  |
|  **Validation instructions** (Instrucciones de validación)  |  Si lo desea, agregue instrucciones (hasta 3000 caracteres) para ayudar al equipo de validación de Microsoft a configurar, conectar y probar la aplicación. Incluya opciones de configuración típicas, cuentas, parámetros u otra información que pueda usarse para probar la opción de conexión de datos. Esta información solo está visible para el equipo de validación y solo se usa para validación.  |
| **¿Se ha creado esta aplicación como un paquete de contenido de Power BI?** | Actualmente, este campo se usa solo internamente. Deje la configuración predeterminada de **No**. Si cambia la configuración a **Sí**, podría detener el proceso de publicación.  |  
|  |  |


## <a name="next-steps"></a>Pasos siguientes

En la pestaña [Detalles del escaparate digital](./cpp-storefront-details-tab.md), proporcione la información de marketing y legal sobre la aplicación.

