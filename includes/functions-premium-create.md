---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: jeffhollan
ms.service: azure-functions
ms.topic: include
ms.date: 04/01/2019
ms.author: jehollan, glenga
ms.custom: include file
ms.openlocfilehash: 0f3303e7bc87ca0bd29f367405372568ed6da7a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66131560"
---
1. Vaya a [Azure Portal](https://portal.azure.com).

1. Seleccione **+ Crear un recurso** en el lado izquierdo y, a continuación, elija **Function App**.

1. En **Plan de hospedaje**, elija **Plan de App Service** y, a continuación, seleccione **Plan de App Service/Ubicación**.

    ![Creación de una aplicación de función](./media/functions-premium-create/create-function-app-resource.png)

1. Seleccione **Crear nuevo**, escriba un nombre de **Plan de App Service**, elija una **Ubicación** en una [región](https://azure.microsoft.com/regions/) cerca de usted o cerca de otros servicios a los que accedan sus funciones y, a continuación, seleccione **Plan de tarifa**.

    ![Creación de un plan de App Service](./media/functions-premium-create/new-app-service-plan.png)

1. Elija el plan **EP1** (Premium elástico) y, a continuación, seleccione **Aplicar**.

    ![Selección del plan Premium](./media/functions-premium-create/hosting-plan.png) 

1. Seleccione **Aceptar** para crear el plan y, a continuación, utilice las opciones de configuración restantes de la aplicación de función tal y como se especifica en la tabla que encontrará debajo de la imagen. 

    ![Plan de App Service finalizado](./media/functions-premium-create/create-function-app.png)  

    | Configuración      | Valor sugerido  | DESCRIPCIÓN                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nombre de la aplicación** | Nombre único globalmente | Nombre que identifica la nueva Function App. Los caracteres válidos son `a-z`, `0-9` y `-`.  | 
    | **Suscripción** | Su suscripción | Suscripción en la que se creará esta nueva aplicación de función. |
    | **[Grupo de recursos](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Nombre para el nuevo grupo de recursos en el que se va a crear la Function App. También puede usar el valor sugerido. |
    | **SISTEMA OPERATIVO** | Windows | El plan Premium actualmente no admite Linux. |
    | **Pila en tiempo de ejecución** | Lenguaje preferido | Elija un tiempo de ejecución que admita su lenguaje de programación de funciones preferido. Elija **.NET** para funciones de C# y F#. Solo se muestran los idiomas admitidos en el **SO** elegido. |
    | **[Storage](../articles/storage/common/storage-quickstart-create-account.md)** |  Nombre único globalmente |  Cree una cuenta de almacenamiento que use la aplicación de función. Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula. También puede usar una cuenta existente que debe cumplir los [requisitos de la cuenta de almacenamiento](../articles/azure-functions/functions-scale.md#storage-account-requirements). |
    | **[Application Insights](../articles/azure-functions/functions-monitoring.md)** | Valor predeterminado | Crea un recurso de Application Insights con el mismo *nombre de aplicación* en la región más cercana que lo admita. Si expande esta configuración, puede cambiar el valor de **Nuevo nombre de recurso**  o elegir otro valor en **Ubicación** en la [ubicación geográfica de Azure](https://azure.microsoft.com/global-infrastructure/geographies/) donde desee almacenar los datos. |

1. Una vez validada la configuración, seleccione **Crear**.

1. Seleccione el icono de notificación de la esquina superior derecha del portal y observe el mensaje **Implementación correcta**.

    ![Definir nueva configuración de Function App](./media/functions-premium-create/function-app-create-notification.png)

1. Seleccione **Ir al recurso** para ver la nueva aplicación de función. También puede seleccionar **Anclar al panel**. Dicho anclaje facilita la vuelta a este recurso de aplicación de función desde el panel.