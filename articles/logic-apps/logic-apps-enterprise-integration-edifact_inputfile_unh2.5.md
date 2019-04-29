---
title: 'Control de mensajes EDIFACT con segmentos UNH 2.5: Azure Logic Apps | Microsoft Docs'
description: Resuelva los documentos EDIFACT con segmentos UNH 2.5 en Azure Logic Apps con Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.date: 04/27/2017
ms.openlocfilehash: 926c9ebe8675d8b50d4544be813ae0b15492ae35
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60681685"
---
# <a name="handle-edifact-documents-with-unh25-segments-in-azure-logic-apps"></a>Control de documentos EDIFACT con segmentos UNH2.5 en Azure Logic Apps

Si UNH2.5 está presente en el documento EDIFACT, se va a usar para la búsqueda de esquema. 

Ejemplo: El campo UNH es **EAN008** en el mensaje EDIFACT  
UNH+SSDD1+ORDERS:D:03B:UN:**EAN008**'  

Pasos que deben seguirse para controlar el mensaje: 
1. Revisión del esquema
2. Comprobación de la configuración del acuerdo  

## <a name="update-the-schema"></a>Revisión del esquema
Para procesar el mensaje, debe implementar un esquema con el nombre del nodo raíz UNH2.5.  Para un ejemplo dado, el nombre de raíz de esquema sería **EFACT_D03B_ORDERS_EAN008**.  

Para cada D03B_ORDERS con un segmento UNH2.5 diferente, tendría que implementar un esquema individual.  

## <a name="add-schema-to-the-edifact-agreement"></a>Incorporación de un esquema al acuerdo de EDIFACT
### <a name="edifact-decode"></a>Descodificación EDIFACT
Para descodificar el mensaje entrante, configure el esquema en la configuración de recepción del acuerdo EDIFACT:
1. Agregue el esquema a la cuenta de integración.    
2. Configure el esquema en la configuración de recepción del acuerdo EDIFACT. 
3. Seleccione el acuerdo EDIFACT y haga clic en **Editar como JSON**.  Agregue un valor UNH2.5 en el acuerdo de recepción **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image1.png).

### <a name="edifact-encode"></a>Codificación EDIFACT
Para codificar el mensaje entrante, configure el esquema en la configuración de envío del acuerdo EDIFACT:
1. Agregue el esquema a la cuenta de integración.    
2. Configure el esquema en la configuración de envío del acuerdo EDIFACT. 
3. Seleccione el acuerdo EDIFACT y haga clic en **Editar como JSON**.  Agregue un valor UNH2.5 en el acuerdo de envío **schemaReferences**
![](./media/logic-apps-enterprise-integration-edifact_inputfile_unh2.5/image2.png).

## <a name="next-steps"></a>Pasos siguientes
* [Más información sobre los acuerdos de cuentas de integración](../logic-apps/logic-apps-enterprise-integration-agreements.md "Más información sobre los acuerdos de cuentas de integración")  