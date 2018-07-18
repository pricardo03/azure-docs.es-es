---
title: Agregar instrucciones condicionales a los flujos de trabajo - Azure Logic Apps | Microsoft Docs
description: Cómo crear condiciones que controlen las acciones de los flujos de trabajo en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: e8d84944d44588602593c762c4f60c375e480343
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298175"
---
# <a name="create-conditional-statements-that-control-workflow-actions-in-azure-logic-apps"></a>Crear instrucciones condicionales que controlen las acciones de los flujo de trabajo en Azure Logic Apps

Para ejecutar acciones específicas en la aplicación lógica solo después de pasar una condición especificada, agregue una *instrucción condicional*. Esta estructura compara los datos de un flujo de trabajo con valores o campos concretos. A continuación, se pueden definir las distintas acciones que se ejecutan en función de si los datos cumplen la condición, o no. Las condiciones se pueden anidar unas dentro de otras.

Por ejemplo, suponga que tiene una aplicación lógica que envía demasiados correos electrónicos cuando aparecen elementos nuevos en la fuente RSS de un sitio web. Puede agregar una instrucción condicional para que se envíe correo electrónico solo cuando el elemento nuevo incluya una cadena específica. 

> [!TIP]
> Para ejecutar pasos diferentes para distintos valores concretos, use una [*instrucción switch*](../logic-apps/logic-apps-control-flow-switch-statement.md) en su lugar.

## <a name="prerequisites"></a>requisitos previos

* Una suscripción de Azure. Si aún no tiene una, [regístrese para obtener una cuenta de Azure gratuita](https://azure.microsoft.com/free/).

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Para seguir el ejemplo de este artículo, [cree una aplicación lógica de ejemplo](../logic-apps/quickstart-create-first-logic-app-workflow.md) con una cuenta de Outlook.com o de Office 365 Outlook.

## <a name="add-a-condition"></a>Agregar una condición

1. En <a href="https://portal.azure.com" target="_blank">Azure Portal</a>, abra la aplicación lógica en Diseñador de aplicación lógica.

2. Agregue una condición en la ubicación que desee. 

   Para agregar una condición entre los pasos, mueva el puntero sobre la flecha en la que desee agregar la condición. Haga clic en el **signo más** (**+**) que aparece y seleccione **Agregar una condición**. Por ejemplo: 

   ![Agregar condición entre pasos](./media/logic-apps-control-flow-conditional-statement/add-condition.png)

   Si desea agregar una condición al final del flujo de trabajo actual, en la parte inferior de la aplicación lógica y elija **+ Nuevo paso** > **Agregar una condición**.

3. En **Condición**, cree la condición. 

   1. En el cuadro izquierdo, especifique los datos o el campo que desee comparar.

      En la lista **Agregar contenido dinámico**, puede seleccionar los campos existentes desde la aplicación lógica.

   2. En la lista central, seleccione la operación que desee que se realice. 
   3. En el cuadro de la derecha, especifique el valor o campo que se va a usar como criterio.

   Por ejemplo: 

   ![Edición de una condición en modo básico](./media/logic-apps-control-flow-conditional-statement/edit-condition-basic-mode.png)

   Esta es la condición completa:

   ![Condición completa](./media/logic-apps-control-flow-conditional-statement/edit-condition-basic-mode-2.png)

   > [!TIP]
   > Para crear una condición más avanzada o usar expresiones, elija **Editar en el modo avanzado**. Puede usar expresiones definidas por el [lenguaje de definición de flujo de trabajo](../logic-apps/logic-apps-workflow-definition-language.md).
   > 
   > Por ejemplo: 
   >
   > ![Edición de la condición en el código](./media/logic-apps-control-flow-conditional-statement/edit-condition-advanced-mode.png)

5. En **EN CASO AFIRMATIVO** y **EN CASO NEGATIVO**, agregue los pasos que se van a realizar en función de que la condición se cumpla. Por ejemplo: 

   ![Condición con las trayectoria en caso de AFIRMATIVO y NEGATIVO](./media/logic-apps-control-flow-conditional-statement/condition-yes-no-path.png)

   > [!TIP]
   > Puede arrastrar acciones existentes a las trayectorias **EN CASO AFIRMATIVO** y **EN CASO NEGATIVO**.

6. Guarde la aplicación lógica.

Ahora esta aplicación lógica solo envía correo electrónico cuando los nuevos elementos de la fuente RSS cumplen una condición.

## <a name="json-definition"></a>Definición JSON

Ahora que ha creado una aplicación lógica mediante una instrucción condicional, examinaremos la definición de código de alto nivel que hay detrás de ella.

``` json
"actions": {
  "myConditionName": {
    "type": "If",
    "expression": "@contains(triggerBody()?['summary'], 'Microsoft')",
    "actions": {
      "Send_an_email": {
        "inputs": { },
        "runAfter": {}
      }
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar características y sugerencias o votar las que ya se han enviado, visite el [sitio web de comentarios de los usuarios de Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* [Realización de pasos en función de los diferentes valores (instrucciones switch)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Ejecución y repetición de pasos (bucles)](../logic-apps/logic-apps-control-flow-loops.md)
* [Ejecución o combinación de pasos en paralelo (ramas)](../logic-apps/logic-apps-control-flow-branches.md)
* [Ejecución de pasos en función del estado de las acciones agrupadas (ámbitos)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
