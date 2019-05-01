---
title: Crear y administrar acuerdos entre socios comerciales - Azure Logic Apps
description: Crear y administrar acuerdos entre socios comerciales mediante el uso de Azure Logic Apps y Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 447ffb8e-3e91-4403-872b-2f496495899d
ms.date: 04/05/2019
ms.openlocfilehash: 26d653b873e959f0804e0456ed87ee68c39413e5
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64720675"
---
# <a name="create-and-manage-trading-partner-agreements-by-using-azure-logic-apps-and-enterprise-integration-pack"></a>Crear y administrar acuerdos entre socios comerciales mediante el uso de Azure Logic Apps y Enterprise Integration Pack

Un [socio comercial](../logic-apps/logic-apps-enterprise-integration-partners.md) 
*acuerdo* ayuda a las organizaciones y empresas se comunican perfectamente entre sí definiendo el protocolo estándar del sector específico que se usará al intercambiar mensajes de negocio a negocio (B2B). Los contratos proporcionan las ventajas comunes, por ejemplo:

* Permiten a las organizaciones intercambiar información con un formato conocido.
* Mejorar la eficacia al realizar transacciones B2B.
* Son fáciles de crear, administrar y usar para crear soluciones de integración de enterprise.

En este artículo se muestra cómo crear un AS2, EDIFACT o X12 contrato que puede usar al compilar soluciones de integración para escenarios B2B de enterprise mediante el uso de la [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) y [deAzureLogicApps](../logic-apps/logic-apps-overview.md). Después de crear un acuerdo, a continuación, puede utilizar AS2, EDIFACT o X12 conectores para intercambiar mensajes B2B.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si aún no tiene ninguna suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Un [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) para almacenar el contrato y otros artefactos B2B. Esta cuenta de integración debe asociarse con su suscripción de Azure.

* Al menos dos [socios comerciales](../logic-apps/logic-apps-enterprise-integration-partners.md) que ya ha creado en la cuenta de integración. Un contrato requiere un asociado del host y un socio comercial invitado. Ambos asociados deben usar el mismo calificador "identidad de negocio" como el acuerdo que desea crear, como AS2, X 12 o EDIFACT.

* Opcional: La aplicación lógica que desea usar el contrato y un desencadenador que inicia el flujo de trabajo de la aplicación lógica. Para crear su cuenta de integración y los artefactos B2B, no necesita una aplicación lógica. Sin embargo, antes de la aplicación lógica puede usar los artefactos B2B en la cuenta de integración, debe vincular la cuenta de integración a la aplicación lógica. Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) e [Inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-agreements"></a>Crear acuerdos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
En el menú principal de Azure, seleccione **Todos los servicios**. En el cuadro de búsqueda, escriba "integración" como filtro. En los resultados, seleccione este recurso: **Cuentas de integración**

   ![Búsqueda de la cuenta de integración](./media/logic-apps-enterprise-integration-agreements/find-integration-accounts.png)

1. En **cuentas de integración**, seleccione la cuenta de integración donde desea crear el acuerdo.

   ![Selección de la cuenta de integración donde desea crear el contrato](./media/logic-apps-enterprise-integration-agreements/select-integration-account.png)

1. En el panel derecho, bajo **componentes**, elija el **acuerdos** icono.

   ![Elija "Contratos"](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

1. En **Contratos**, elija **Agregar**. En el **agregar** panel, proporcionan información sobre su contrato, por ejemplo:

   ![Elección de "Agregar"](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)

   | Propiedad | Obligatorio | Value | DESCRIPCIÓN |
   |----------|----------|-------|-------------|
   | **Nombre** | Sí | <*agreement-name*> | El nombre del contrato |
   | **Tipo de contrato** | Sí | **AS2**, **X12**, o **EDIFACT** | El tipo de protocolo para el acuerdo. Cuando se crea el archivo de contrato, el contenido de ese archivo debe coincidir con el tipo de contrato. | |  
   | **Asociado del host** | Sí | <*host-partner-name*> | El partner anfitrión representa la organización que especifica el contrato |
   | **Identidad del host** | Sí | <*host-partner-identifier*> | Identificador del asociado del host |
   | **Socio comercial invitado** | Sí | <*guest-partner-name*> | El asociado invitado representa la organización que está haciendo negocios con el asociado del host. |
   | **Identidad del invitado** | Sí | <*guest-partner-identifier*> | Identificador del socio invitado |
   | **Configuración de recepción** | Varía | Varía | Estas propiedades especifican cómo controlar todos los mensajes entrantes recibidos por el contrato. Para obtener más información, vea el tipo de contrato respectivos: <p>- [Configuración del mensaje AS2](../logic-apps/logic-apps-enterprise-integration-as2-message-settings.md) <br>- [Configuración de mensajes EDIFACT](logic-apps-enterprise-integration-edifact.md) <br>- [Configuración del mensaje X12](logic-apps-enterprise-integration-x12.md) |
   | **Configuración de envío** | Varía | Varía | Estas propiedades especifican cómo controlar todos los mensajes salientes enviados por el acuerdo. Para obtener más información, vea el tipo de contrato respectivos: <p>- [Configuración del mensaje AS2](../logic-apps/logic-apps-enterprise-integration-as2-message-settings.md) <br>- [Configuración de mensajes EDIFACT](logic-apps-enterprise-integration-edifact.md) <br>- [Configuración del mensaje X12](logic-apps-enterprise-integration-x12.md) |
   |||||

1. Cuando haya terminado la creación de su contrato, en el **agregar** página, elija **Aceptar**y volver a la cuenta de integración.

   El **acuerdos** lista muestra ahora el nuevo contrato.

## <a name="edit-agreements"></a>Editar contratos

1. En el [portal Azure](https://portal.azure.com), en el menú principal de Azure, seleccione **todos los servicios**.

1. En el cuadro de búsqueda, escriba "integración" como filtro. En los resultados, seleccione este recurso: **Cuentas de integración**

1. En **cuentas de integración**, seleccione la cuenta de integración que tenga el acuerdo que desea editar.

1. En el panel derecho, bajo **componentes**, elija el **acuerdos** icono.

1. En **acuerdos**, seleccione el contrato y elija **editar**.

1. Realizar y, a continuación, guarde los cambios.

## <a name="delete-agreements"></a>Eliminar contratos

1. En el [portal Azure](https://portal.azure.com), en el menú principal de Azure, seleccione **todos los servicios**.

1. En el cuadro de búsqueda, escriba "integración" como filtro. En los resultados, seleccione este recurso: **Cuentas de integración**

1. En **cuentas de integración**, seleccione la cuenta de integración que tenga el acuerdo que desea eliminar.

1. En el panel derecho, bajo **componentes**, elija el **acuerdos** icono.

1. En **acuerdos**, seleccione el contrato y elija **eliminar**.

1. Confirme que desea eliminar el contrato seleccionado.

## <a name="next-steps"></a>Pasos siguientes

* [Intercambio de mensajes AS2](logic-apps-enterprise-integration-as2.md)
* [Intercambio de mensajes EDIFACT](logic-apps-enterprise-integration-edifact.md)
* [Mensajes de Exchange X12](logic-apps-enterprise-integration-x12.md)
