---
title: Inicio, detención y eliminación de una aplicación de Azure Spring Cloud | Microsoft Docs
description: Inicio, detención y eliminación de una aplicación de Azure Spring Cloud
services: spring-cloud
author: v-vasuke
manager: jeconnoc
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: v-vasuke
ms.openlocfilehash: f7f76644d13c20704d2c3bd908176ac452df2a20
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038444"
---
# <a name="how-to-start-stop-and-delete-your-azure-spring-cloud-application"></a>Inicio, detención y eliminación de una aplicación de Azure Spring Cloud

En esta guía se explica cómo cambiar el estado de una aplicación en Azure Spring Cloud mediante Azure Portal o la CLI.

## <a name="using-the-azure-portal"></a>Uso de Azure Portal

Una vez que la aplicación esté implementada, puede **iniciarla**, **detenerla**y **eliminarla** desde Azure Portal.

1. Vaya a la instancia de servicio de Azure Spring Cloud en Azure Portal.
1. Seleccione la pestaña **Panel de la aplicación**.
1. Seleccione la aplicación cuyo estado desea cambiar.
2. En la página **Información general** de la aplicación, busque los botones para **iniciar/detener**, **reiniciar** y **eliminar** la aplicación.

## <a name="using-the-azure-cli"></a>Uso de la CLI de Azure

> [!NOTE]
> Puede usar parámetros opcionales y configurar los valores predeterminados con la CLI de Azure. Para más información, lea [la documentación de referencia](spring-cloud-cli-reference.md).

* Para iniciar la aplicación:
    ```Azure CLI
    az spring-cloud app start -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Para detener la aplicación:
    ```Azure CLI
    az spring-cloud app stop -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Para reiniciar la aplicación:
    ```Azure CLI
    az spring-cloud app restart -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Para eliminar la aplicación:
    ```Azure CLI
    az spring-cloud app delete -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```
