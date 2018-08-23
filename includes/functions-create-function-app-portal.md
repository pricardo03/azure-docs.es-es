---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 07/17/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a0f6ca72e6d3e487e85d16a6860e0d84003a9aa8
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "40260654"
---
1. Seleccione el botón **Nuevo** de la esquina superior izquierda de Azure Portal y, después, **Proceso** > **Function App**. 

    ![Creación de una aplicación de función en Azure Portal](./media/functions-create-function-app-portal/function-app-create-flow.png)

2. Use la configuración de Function App especificada en la tabla debajo de la imagen.

    ![Definir nueva configuración de Function App](./media/functions-create-function-app-portal/function-app-create-flow2.png)

    | Configuración      | Valor sugerido  | DESCRIPCIÓN                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nombre de la aplicación** | Nombre único globalmente | Nombre que identifica la nueva Function App. Los caracteres válidos son `a-z`, `0-9` y `-`.  | 
    | **Suscripción** | Su suscripción | Suscripción en la que se creará esta nueva aplicación de función. | 
    | **[Grupo de recursos](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Nombre para el nuevo grupo de recursos en el que se va a crear la Function App. | 
    | **SISTEMA OPERATIVO** | Windows | El hospedaje sin servidor actualmente solo está disponible cuando se ejecuta en Windows. Para el hospedaje en Linux, consulte [Creación de su primera función para ejecutarse en Linux con la CLI de Azure (versión preliminar)](../articles/azure-functions/functions-create-first-azure-function-azure-cli-linux.md). |
    | **[Plan de hospedaje](../articles/azure-functions/functions-scale.md)** | Plan de consumo | Plan de hospedaje que define cómo se asignan los recursos a la Function App. En el **Plan de consumo** predeterminado, los recursos se agregan dinámicamente según lo requieran sus funciones. En este hospedaje [sin servidor](https://azure.microsoft.com/overview/serverless-computing/), solo paga por el tiempo durante el cual se ejecutan las funciones. Si ejecuta un plan de App Service, debe administrar el [escalado de la aplicación de función](../articles/azure-functions/functions-scale.md).  |
    | **Ubicación** | Europa occidental | Elija una [región](https://azure.microsoft.com/regions/) cerca de usted o cerca de otros servicios a los que tendrán acceso las funciones. |
    | **[Cuenta de almacenamiento](../articles/storage/common/storage-quickstart-create-account.md)** |  Nombre único globalmente |  Nombre de la nueva cuenta de almacenamiento usada por Function App. Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula. También puede usar una cuenta existente que debe cumplir los [requisitos de la cuenta de almacenamiento](../articles/azure-functions/functions-scale.md#storage-account-requirements). |

3. Seleccione **Crear** para aprovisionar e implementar la aplicación de función.

4. Seleccione el icono de notificación de la esquina superior derecha del portal y observe el mensaje **Implementación correcta**. 

    ![Definir nueva configuración de Function App](./media/functions-create-function-app-portal/function-app-create-notification.png)

5. Seleccione **Ir al recurso** para ver la nueva aplicación de función.

> [!TIP]
> Si tiene problemas para encontrar las aplicaciones de función en el portal, intente [agregar Function App a favoritos en Azure Portal](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#favorite).   
