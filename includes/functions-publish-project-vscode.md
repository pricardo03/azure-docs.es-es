---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 04/16/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: ec0425ff2188ecf1816d5f5841394c8e32f301d2
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132220"
---
## <a name="publish-the-project-to-azure"></a>Publicar el proyecto en Azure

Visual Studio Code le permite publicar el proyecto de Functions directamente en Azure. En el proceso, puede crear una aplicación de función y los recursos relacionados en su suscripción de Azure. La aplicación de función proporciona un contexto de ejecución para sus funciones. El proyecto se empaqueta e implementa en la nueva aplicación de función en su suscripción de Azure.

En este artículo se da por supuesto que va a crear una nueva aplicación de función. 

> [!IMPORTANT]
> La publicación en una aplicación de función existente sobrescribe el contenido de esa aplicación en Azure.

1. En el área **Azure: Functions**, seleccione el icono de implementación en Function App.

    ![Configuración de Function App](./media/functions-publish-project-vscode/function-app-publish-project.png)

1. Se le pedirá que **inicie sesión en Azure** si todavía no lo ha hecho. También puede **crear una cuenta de Azure gratis**. Una vez que inicie sesión correctamente en el explorador, vuelva a Visual Studio Code. 

1. Si tiene varias suscripciones, **seleccione una suscripción** para la aplicación de función y, luego, elija **+ Create New Function App in Azure** (+ Crear nueva aplicación de función en Azure).

1. Escriba un nombre único global que identifique la aplicación de función y presione ENTRAR. Los siguientes son caracteres válidos para un nombre de aplicación de función: `a-z`, `0-9` y `-`.

1. Elija **+ Crear un nuevo grupo de recursos**, escriba un nombre para el grupo de recursos, como `myResourceGroup` y presione ENTRAR. También puede usar un grupo de recursos existente.

1. Elija **+ Crear nueva cuenta de almacenamiento**, escriba un nombre único global para la nueva cuenta de almacenamiento que va a usar la aplicación de función y presione ENTRAR. Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula. También puede usar una cuenta existente.

1. Elija una ubicación en una [región](https://azure.microsoft.com/regions/) cercana a usted o a otros servicios a los que accedan las funciones.

    Cuando presione ENTRAR, se crearán los siguientes recursos de Azure en la suscripción:

    * **[Grupo de recursos](../articles/azure-resource-manager/resource-group-overview.md)** : contiene todos los recursos de Azure creados. El nombre se basa en el nombre de la aplicación de función.
    * **[Cuenta de almacenamiento](../articles/storage/common/storage-quickstart-create-account.md)** : se crea una cuenta de almacenamiento estándar con un nombre único en función del nombre de la aplicación de función.
    * **[Plan de hospedaje](../articles/azure-functions/functions-scale.md)** : se crea un plan de consumo en la región Oeste de EE. UU. para hospedar la aplicación de función sin servidor.
    * **Aplicación de función**: el proyecto se implementa y ejecuta en esta aplicación de función nueva.

    Una vez que se haya creado la aplicación de función se mostrará una notificación y se aplicará el paquete de implementación. Seleccione **View Output** (Ver salida) en esta notificación para ver la creación y los resultados de la implementación, incluidos los recursos de Azure que ha creado.

1. Vuelva al área **Azure: Functions** y expanda la aplicación de función nueva en su suscripción. Expanda **Funciones**, haga clic con el botón derecho en **HttpTrigger** y elija **Copiar la dirección URL de la función**.

    ![Copia de la dirección URL de la función para el nuevo desencadenador HTTP](./media/functions-publish-project-vscode/function-copy-endpoint-url.png)
