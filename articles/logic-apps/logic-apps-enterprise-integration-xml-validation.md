---
title: 'Validación XML para la integración empresarial B2B: Azure Logic Apps | Microsoft Docs'
description: Validación XML con esquemas para soluciones B2B en Azure Logic Apps con Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: d700588f-2d8a-4c92-93eb-e1e6e250e760
ms.date: 07/08/2016
ms.openlocfilehash: 8db0dbadd944007ff953f9ea69695bf988ffebb7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60996345"
---
# <a name="validate-xml-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Validación XML para la integración empresarial B2B en Azure Logic Apps con Enterprise Integration Pack

A menudo, en los escenarios B2B, los asociados de un contrato deben asegurarse que los mensajes que intercambian son válidos para que pueda comenzar el procesamiento de datos. En Enterprise Integration Pack, puede usar el conector de validación XML para validar documentos con un esquema predefinido.

## <a name="validate-a-document-with-the-xml-validation-connector"></a>Validación de un documento con el conector de validación XML

1. Cree una aplicación lógica y [vincúlela a la cuenta de integración](../logic-apps/logic-apps-enterprise-integration-accounts.md "Aprenda a vincular una cuenta de integración a una aplicación lógica") que contenga el esquema que utilizará para validar los datos XML.

2. Agregue un desencadenador **Request - When an HTTP request is received** (Solicitar: cuando se reciba una solicitud HTTP) a la aplicación lógica.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-1.png)

3. Agregue la acción **XML Validation** (Validación XML), pero seleccione antes **Agregar una acción**.

4. Escriba *xml* en el cuadro de búsqueda para filtrar todas las acciones hasta encontrar la que desea usar. Seleccione **XML Validation** (Validación XML).

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-2.png)

5. Para especificar el contenido XML que desea validar, seleccione **CONTENIDO**.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-1-5.png)

6. Elija la etiqueta Cuerpo como el contenido que quiere validar.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-3.png)

7. Para especificar el esquema que desea usar para validar la entrada de *contenido* anterior, elija **NOMBRE DE ESQUEMA**.

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-4.png)

8. Guarde el trabajo  

    ![](./media/logic-apps-enterprise-integration-xml-validation/xml-5.png)

Con esto, ha terminado de configurar el conector de validación. En una aplicación real, podría almacenar los datos validados en una aplicación de línea de negocio (LOB) como SalesForce. Para enviar la salida validada a Salesforce, agregue una acción.

Para probar la acción de validación realice una solicitud al punto de conexión HTTP.

## <a name="next-steps"></a>Pasos siguientes
[Más información sobre Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Información sobre Enterprise Integration Pack")   

