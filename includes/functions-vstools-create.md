---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/05/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 3d93d3aa3e4e646f8e054f96f17bbe4a011d422d
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2020
ms.locfileid: "77211386"
---
La plantilla del proyecto de Azure Functions de Visual Studio crea un proyecto que se puede publicar en una aplicación de función en Azure. Una aplicación de funciones permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación, el escalado y el uso compartido de recursos.

1. En Visual Studio, en el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.

1. En el cuadro de diálogo **Crear un proyecto**, busque `functions`, elija la plantilla de **Azure Functions** y, a continuación, seleccione **Siguiente**.

1. Escriba un nombre para el proyecto y seleccione **Crear**. El nombre de la aplicación de función debe ser válido como espacio de nombres de C#, por lo que no debe usar guiones bajos, guiones u otros caracteres no alfanuméricos.

1. En **Crear una aplicación de Azure Functions**, utilice las siguientes opciones:

    + **Azure Functions v2 (.NET Core)**
    + **desencadenador HTTP**
    + **Storage Account** (Cuenta de almacenamiento): **Emulador de Storage**
    + **Nivel de autorización**: **Anonymous** 

    | Opción      | Valor sugerido  | Descripción                      |
    | ------------ |  ------- |----------------------------------------- |
    | **Entorno en tiempo de ejecución de Functions** | **Azure Functions 2. x <br />(.NET Core)** | Este valor crea un proyecto de función que usa la versión 2.x del entorno de ejecución de Azure Functions, que es compatible con .NET Core. Azure Functions 1.x admite .NET Framework. Para más información, consulte [Cómo seleccionar un destino para versiones en tiempo de ejecución de Azure Functions](../articles/azure-functions/functions-versions.md).   |
    | **Plantilla de función** | **desencadenador HTTP** | Con este valor se crea una función desencadenada por una solicitud HTTP. |
    | **Storage Account**  | **Emulador de Storage** | Un desencadenador HTTP no usa la conexión de la cuenta de Azure Storage. Todos los demás tipos de desencadenador requieren una cadena de conexión de cuenta de Storage válida. Dado que Azure Functions necesita una cuenta de Storage, se asigna una o se crea al publicar el proyecto en Azure. |
    | **Nivel de autorización** | **Anónimo** | Cualquier cliente puede desencadenar una función creada sin tener que proporcionar una clave. Esta configuración de autorización facilita probar la función nueva. Para más información sobre las claves y la autorización, consulte [Claves de autorización](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) en los [enlaces HTTP y de webhook](../articles/azure-functions/functions-bindings-http-webhook.md). |
    
    > [!NOTE]
    > Asegúrese de establecer el **Nivel de autorización** en `Anonymous`. Al elegir el nivel predeterminado de `Function`, tiene que presentar la [tecla de función](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) en las solicitudes para acceder al punto de conexión de la función.
    
4. Seleccione **Crear** para crear el proyecto de función y la función que se desencadena mediante HTTP.
