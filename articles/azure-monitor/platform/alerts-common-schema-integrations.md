---
title: Cómo integrar el esquema común de alerta con Logic Apps
description: Obtenga información sobre cómo crear una aplicación lógica que aprovecha el esquema común de alerta para controlar todas las alertas.
author: ananthradhakrishnan
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/27/2019
ms.author: anantr
ms.subservice: alerts
ms.openlocfilehash: 13cb3880662e1665b03dd63f009645acbe97fc75
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734881"
---
# <a name="how-to-integrate-the-common-alert-schema-with-logic-apps"></a>Cómo integrar el esquema común de alerta con Logic Apps

Este artículo muestra cómo crear una aplicación lógica que aprovecha el esquema común de alerta para controlar todas las alertas.

## <a name="overview"></a>Información general

El [esquema común de alerta](https://aka.ms/commonAlertSchemaDocs) proporciona un esquema JSON estandarizado y extensible a través de todos los diferentes tipos de alerta. El esquema común de alerta es especialmente útil cuando se aprovechan mediante programación: a través de webhooks, runbooks y las aplicaciones lógicas. En este artículo, se muestra cómo se puede crear una única aplicación lógica para controlar todas las alertas. Los mismos principios se pueden aplicar a otros métodos de programación. La aplicación lógica se describe en este artículo crea variables bien definidas para el [campos 'esenciales'](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#essentials-fields)y también se describe cómo puede controlar [tipo de alerta](/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields) lógica específica.


## <a name="prerequisites"></a>Requisitos previos 

En este artículo se da por supuesto que el lector está familiarizado con 
* Configurar reglas de alerta ([métrica](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric), [registro](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log), [registro de actividad](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log))
* Configurar [grupos de acciones](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)
* Habilitar la [esquema común de alerta](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema#how-do-i-enable-the-common-alert-schema) desde dentro de grupos de acciones

## <a name="create-a-logic-app-leveraging-the-common-alert-schema"></a>Crear una aplicación lógica, aprovechando el esquema común de alerta

1. Siga el [describen los pasos para crear la aplicación lógica](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups-logic-app). 

1.  Seleccione el desencadenador: **Cuando se recibe una solicitud HTTP**.

    ![Desencadenadores de aplicación lógica](media/action-groups-logic-app/logic-app-triggers.png "Desencadenadores de aplicación lógica")

1.  Seleccione **Editar** para cambiar el desencadenador de solicitud HTTP.

    ![Desencadenadores de solicitud HTTP](media/action-groups-logic-app/http-request-trigger-shape.png "Desencadenadores de solicitud HTTP")


1.  Copie y pegue el siguiente esquema:

    ```json
        {
            "type": "object",
            "properties": {
                "schemaId": {
                    "type": "string"
                },
                "data": {
                    "type": "object",
                    "properties": {
                        "essentials": {
                            "type": "object",
                            "properties": {
                                "alertId": {
                                    "type": "string"
                                },
                                "alertRule": {
                                    "type": "string"
                                },
                                "severity": {
                                    "type": "string"
                                },
                                "signalType": {
                                    "type": "string"
                                },
                                "monitorCondition": {
                                    "type": "string"
                                },
                                "monitoringService": {
                                    "type": "string"
                                },
                                "alertTargetIDs": {
                                    "type": "array",
                                    "items": {
                                        "type": "string"
                                    }
                                },
                                "originAlertId": {
                                    "type": "string"
                                },
                                "firedDateTime": {
                                    "type": "string"
                                },
                                "resolvedDateTime": {
                                    "type": "string"
                                },
                                "description": {
                                    "type": "string"
                                },
                                "essentialsVersion": {
                                    "type": "string"
                                },
                                "alertContextVersion": {
                                    "type": "string"
                                }
                            }
                        },
                        "alertContext": {
                            "type": "object",
                            "properties": {}
                        }
                    }
                }
            }
        }
    ```

1. Seleccione **+** **Nuevo paso** y luego elija **Agregar una acción**.

    ![Agregar una acción](media/action-groups-logic-app/add-action.png "Agregar una acción")

1. En esta fase, puede agregar una variedad de conectores (Microsoft Teams, Slack, Salesforce, etc.) en función de sus requisitos empresariales específicos. Puede usar el 'campos esenciales'-de-fábrica. 

    ![Campos esenciales](media/alerts-common-schema-integrations/logic-app-essential-fields.png "campos esenciales")
    
    Como alternativa, puede crear lógica condicional según el tipo de alerta mediante la opción 'Expression'.

    ![Expresión de aplicación lógica](media/alerts-common-schema-integrations/logic-app-expressions.png "expresión de aplicación lógica")
    
     El ['monitoringService' campo](/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields) permite identificar de forma única el tipo de alerta en función de que se puede crear la lógica condicional.

    
    Por ejemplo, el siguiente fragmento de código comprueba si la alerta es una alerta de registro basadas en Application Insights y, en caso afirmativo, imprime los resultados de búsqueda. En caso contrario, imprime "NA".

    ```text
      if(equals(triggerBody()?['data']?['essentials']?['monitoringService'],'Application Insights'),triggerBody()?['data']?['alertContext']?['SearchResults'],'NA')
    ```
    
     Obtenga más información sobre [escribir lógica de las expresiones de la aplicación](https://docs.microsoft.com/azure/logic-apps/workflow-definition-language-functions-reference#logical-comparison-functions).

    


## <a name="next-steps"></a>Pasos siguientes

* [Más información acerca de los grupos de acción](../../azure-monitor/platform/action-groups.md).
* [Más información sobre el esquema común de alerta](https://aka.ms/commonAlertSchemaDocs).

