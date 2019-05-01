---
title: 'Incorporación de socios comerciales para integraciones de B2B: Azure Logic Apps | Microsoft Docs'
description: Crear socios comerciales para la cuenta de integración en Azure Logic Apps con Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.date: 07/08/2016
ms.openlocfilehash: 137ed89c276338b534cad8fdf81ec31b5e5610b5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60846078"
---
# <a name="add-trading-partners-for-integration-accounts-in-azure-logic-apps-with-enterprise-integration-pack"></a>Incorporación de socios comerciales para cuentas de integración en Azure Logic Apps con Enterprise Integration Pack

Los asociados son entidades que participan en transacciones de negocio a negocio (B2B) e intercambian mensajes entre ellos. Antes de poder crear asociados que le representen a usted y a otra organización en estas transacciones, debe compartir la información que identifica y valida los mensajes enviados por cada uno. Después de analizar estos detalles y prepararse para comenzar la relación de negocios, podrá crear asociados en la cuenta de integración para que los represente a ambos.

## <a name="what-roles-do-partners-play-in-your-integration-account"></a>¿Qué roles desempeñan los asociados en la cuenta de integración?

Para definir los detalles acerca de los mensajes intercambiados entre los asociados, debe crear contratos entre ellos. No obstante, antes de crear uno, hay que agregar, como mínimo, dos asociados a la cuenta de integración. La organización debe formar parte del contrato como **asociado del host**. El otro asociado, o **asociado invitado**, representa la organización que intercambia mensajes con su organización. El asociado invitado puede ser otra compañía o incluso un departamento de su organización.

Después de agregar a estos asociados, puede crear un contrato.

La configuración de recepción y de envío afecta exclusivamente al partner anfitrión. Por ejemplo, la configuración de recepción de un contrato determina cómo el asociado anfitrión recibe los mensajes enviados de un asociado invitado. Del mismo modo, la configuración de envío del contrato indica cómo el asociado anfitrión envía mensajes al asociado invitado.

## <a name="create-partner"></a>Creación de un asociado

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. En el menú principal de Azure, seleccione **Todos los servicios**. En el cuadro de búsqueda, escriba "integración" y, después, seleccione **Cuentas de integración**.

   ![Búsqueda de la cuenta de integración](./media/logic-apps-enterprise-integration-partners/account-1.png)

3. En **Cuentas de integración**, seleccione la cuenta de integración en la que va a agregar los asociados.

   ![Seleccionar cuenta de integración](./media/logic-apps-enterprise-integration-partners/account-2.png)

4. Elija el icono de **Asociados**.

   ![Elección de "Asociados"](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. En **Asociados**, elija **Agregar**.

   ![Elección de "Agregar"](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. Escriba un nombre para el asociado y, a continuación, seleccione un **Calificador**. Escriba un **valor** para identificar los documentos que las aplicaciones reciben. Cuando termine, elija **Aceptar**.

   ![Incorporación de los detalles del asociado](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. Elija de nuevo el icono de **Asociados**.

   ![Elección del icono "Asociados"](./media/logic-apps-enterprise-integration-partners/partner-5.png)

   Ahora aparece el nuevo asociado. 

   ![Visualización del nuevo asociado](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="edit-partner"></a>Edición del asociado

1. En [Azure Portal](https://portal.azure.com), busque y seleccione la cuenta de integración. Elija el icono de **Asociados**.

   ![Elección del icono "Asociados"](./media/logic-apps-enterprise-integration-partners/edit.png)

2. En **Asociados**, seleccione el asociado que quiera editar.

   ![Selección del asociado para eliminar](./media/logic-apps-enterprise-integration-partners/edit-1.png)

3. En **Actualizar asociado**, realice los cambios.
Cuando termine, seleccione **Guardar**. 

   ![Haga los cambios y guárdelos](./media/logic-apps-enterprise-integration-partners/edit-2.png)

   Para cancelar los cambios, seleccione **Descartar**.

## <a name="delete-partner"></a>Eliminación del asociado

1. En [Azure Portal](https://portal.azure.com), busque y seleccione la cuenta de integración. Elija el icono de **Asociados**.

   ![Elección del icono "Asociados"](./media/logic-apps-enterprise-integration-partners/delete.png)

2. En **Asociados**, seleccione el asociado que desee eliminar.
Elija **Eliminar**.

   ![Eliminación del asociado](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre los contratos](../logic-apps/logic-apps-enterprise-integration-agreements.md "Información sobre los contratos de integración de empresas")  

