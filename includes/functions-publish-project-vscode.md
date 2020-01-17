---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 04/16/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 66c100ecd40e9f0899f7686397339ab849315c3e
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "76021319"
---
## <a name="publish-the-project-to-azure"></a>Publicar el proyecto en Azure

Visual Studio Code le permite publicar el proyecto de Functions directamente en Azure. En el proceso, puede crear una aplicación de función y los recursos relacionados en su suscripción de Azure. La aplicación de función proporciona un contexto de ejecución para sus funciones. El proyecto se empaqueta e implementa en la nueva aplicación de función en su suscripción de Azure.

De forma predeterminada, Visual Studio Code crea todos los recursos de Azure necesarios para crear la aplicación de función. Los nombres de estos recursos se basan en el nombre de la aplicación de funciones que elija. Si necesita tener control total sobre los recursos creados, en su lugar puede [publicar con las opciones avanzadas](../articles/azure-functions/functions-develop-vs-code.md#enable-publishing-with-advanced-create-options).

En esta sección se da por supuesto que va a crear una nueva aplicación de funciones en Azure.

> [!IMPORTANT]
> La publicación en una aplicación de función existente sobrescribe el contenido de esa aplicación en Azure.

1. En Visual Studio Code, presione F1 para abrir la paleta de comandos. En la paleta de comandos, busque y seleccione `Azure Functions: Deploy to function app...`.

1. Se le pedirá que **inicie sesión en Azure** si todavía no lo ha hecho. También puede **crear una cuenta de Azure gratis**. Una vez que inicie sesión correctamente en el explorador, vuelva a Visual Studio Code. 

1. Si tiene varias suscripciones, **seleccione una suscripción** para la aplicación de función y, luego, elija **+ Create New Function App in Azure** (+ Crear nueva aplicación de función en Azure).

1. Escriba un nombre único global que identifique la aplicación de función y presione ENTRAR. Los siguientes son caracteres válidos para un nombre de aplicación de función: `a-z`, `0-9` y `-`.

    Cuando presione ENTRAR, se crearán los siguientes recursos de Azure en la suscripción:

    * **[Grupo de recursos](../articles/azure-resource-manager/management/overview.md)** : contiene todos los recursos de Azure creados. El nombre se basa en el nombre de la aplicación de función.
    * **[Cuenta de almacenamiento](../articles/storage/common/storage-account-create.md)** : se crea una cuenta de almacenamiento estándar con un nombre único en función del nombre de la aplicación de función.
    * **[Plan de hospedaje](../articles/azure-functions/functions-scale.md)** : se crea un plan de consumo en la región Oeste de EE. UU. para hospedar la aplicación de función sin servidor.
    * **Aplicación de función**: el proyecto se implementa y ejecuta en esta aplicación de función nueva.

    Una vez que se haya creado la aplicación de función se mostrará una notificación y se aplicará el paquete de implementación. Seleccione **View Output** (Ver salida) en esta notificación para ver la creación y los resultados de la implementación, incluidos los recursos de Azure que ha creado.

1. Vuelva al área **Azure: Functions** y expanda la aplicación de función nueva en su suscripción. Expanda **Funciones**, haga clic con el botón derecho en **HttpTrigger** y elija **Copiar la dirección URL de la función**.

    ![Copia de la dirección URL de la función para el nuevo desencadenador HTTP](./media/functions-publish-project-vscode/function-copy-endpoint-url.png)
