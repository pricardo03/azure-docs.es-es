---
title: Inicio, detención y eliminación de una aplicación de Azure Spring Cloud | Microsoft Docs
description: Inicio, detención y eliminación de una aplicación de Azure Spring Cloud
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: jeconnoc
ms.openlocfilehash: 9f537ab425428728137e04713e434d8dc09e065a
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607724"
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

Instale la extensión de Spring Cloud para la CLI de Azure:

```azurecli
az extension add --name spring-cloud
```

* Para iniciar la aplicación:
    ```azurecli
    az spring-cloud app start -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Para detener la aplicación:
    ```azurecli
    az spring-cloud app stop -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Para reiniciar la aplicación:
    ```azurecli
    az spring-cloud app restart -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```

* Para eliminar la aplicación:
    ```azurecli
    az spring-cloud app delete -n <application name> -g <resource group> -s <Azure Spring Cloud name>
    ```
