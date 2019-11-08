---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/17/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: cd118def8b9caf97f26aaa5ad5dc1f6fb015316d
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73469271"
---
1. En el menú de Azure Portal, seleccione **Crear un recurso**.

    ![Incorporación de recursos mediante el menú de Azure Portal](./media/functions-create-function-app-portal/create-function-app-resource.png)

1. En la página **Nuevo**, seleccione **Compute** > **Function App**.

1. Use la configuración de Function App especificada en la tabla debajo de la imagen.

    ![Aspectos básicos](./media/functions-create-function-app-portal/function-app-create-basics.png)

    | Configuración      | Valor sugerido  | DESCRIPCIÓN |
    | ------------ | ---------------- | ----------- |
    | **Suscripción** | Su suscripción | Suscripción en la que se creará esta nueva aplicación de función. |
    | **[Grupo de recursos](../articles/azure-resource-manager/resource-group-overview.md)** |  *myResourceGroup* | Nombre para el nuevo grupo de recursos en el que se va a crear la Function App. |
    | **Nombre de la aplicación de función** | Nombre único globalmente | Nombre que identifica la nueva Function App. Los caracteres válidos son `a-z` (no distingue mayúsculas de minúsculas), `0-9` y `-`.  |
    |**Publicar**| Código | Opción para publicar archivos de código o un contenedor de Docker. |
    | **Pila en tiempo de ejecución** | Lenguaje preferido | Elija un tiempo de ejecución que admita su lenguaje de programación de funciones preferido. Elija **.NET** para funciones de C# y F#. |
    |**Región**| Región preferida | Elija una [región](https://azure.microsoft.com/regions/) cerca de usted o cerca de otros servicios a los que tendrán acceso las funciones. |

    Seleccione el botón **Siguiente: Hospedaje >** .

1. Escriba la siguiente configuración para el hospedaje.

    ![Hospedaje](./media/functions-create-function-app-portal/function-app-create-hosting.png)

    | Configuración      | Valor sugerido  | DESCRIPCIÓN |
    | ------------ | ---------------- | ----------- |
    | **[Cuenta de almacenamiento](../articles/storage/common/storage-quickstart-create-account.md)** |  Nombre único globalmente |  Cree una cuenta de almacenamiento que use la aplicación de función. Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula. También puede usar una cuenta existente que debe cumplir los [requisitos de la cuenta de almacenamiento](../articles/azure-functions/functions-scale.md#storage-account-requirements). |
    |**Sistema operativo**| Sistema operativo preferido | Se preselecciona un sistema operativo en función de la selección de pila en tiempo de ejecución, pero puede cambiar esta configuración si es necesario. |
    | **[Plan](../articles/azure-functions/functions-scale.md)** | Plan de consumo | Plan de hospedaje que define cómo se asignan los recursos a la Function App. En el **Plan de consumo** predeterminado, los recursos se agregan dinámicamente según lo requieran sus funciones. En este hospedaje [sin servidor](https://azure.microsoft.com/overview/serverless-computing/), solo paga por el tiempo durante el cual se ejecutan las funciones. Si ejecuta un plan de App Service, debe administrar el [escalado de la aplicación de función](../articles/azure-functions/functions-scale.md).  |

    Seleccione el botón **Siguiente: Supervisión >** .

1. Escriba la siguiente configuración para la supervisión.

    ![Supervisión](./media/functions-create-function-app-portal/function-app-create-monitoring.png)

    | Configuración      | Valor sugerido  | DESCRIPCIÓN |
    | ------------ | ---------------- | ----------- |
    | **[Application Insights](../articles/azure-functions/functions-monitoring.md)** | Valor predeterminado | Crea un recurso de Application Insights con el mismo *nombre de aplicación* en la región más cercana que lo admita. Si expande esta configuración, puede cambiar el valor de **Nuevo nombre de recurso**  o elegir otro valor en **Ubicación** en la [ubicación geográfica de Azure](https://azure.microsoft.com/global-infrastructure/geographies/) donde desee almacenar los datos. |

    Seleccione **Revisar y crear** para revisar las selecciones de configuración de la aplicación.

1. Seleccione **Crear** para aprovisionar e implementar la aplicación de función.

1. Seleccione el icono de notificación de la esquina superior derecha del portal y observe el mensaje **Implementación correcta**.

    ![Notificación de implementación](./media/functions-create-function-app-portal/function-app-create-notification2.png)

1. Seleccione **Ir al recurso** para ver la nueva aplicación de función. También puede seleccionar **Anclar al panel**. Dicho anclaje facilita la vuelta a este recurso de aplicación de función desde el panel.
