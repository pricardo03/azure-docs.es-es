---
title: Uso del almacenamiento persistente en Azure Spring Cloud | Microsoft Docs
description: Uso del almacenamiento persistente en Azure Spring Cloud
services: spring-cloud
author: v-vasuke
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: jeconnoc
ms.openlocfilehash: 8c57698471d1363438c10e5806f9ed6f1da5333f
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038454"
---
# <a name="how-to-use-persistent-storage-in-azure-spring-cloud"></a>Uso del almacenamiento persistente en Azure Spring Cloud

Azure Spring Cloud proporciona dos tipos de almacenamiento para las aplicaciones: persistente y temporal.  De forma predeterminada, Azure Spring Cloud habilita el almacenamiento temporal para cada instancia de aplicación. El almacenamiento temporal tiene una limitación de 5 GB y su ruta de montaje predeterminada es `/tmp`.

> [!WARNING]
> Al reiniciar una instancia de aplicación, se eliminará de forma permanente el almacenamiento temporal que tiene asociado.

El almacenamiento persistente es un contenedor de recursos compartidos de archivos administrado por Azure que se asigna por ámbito por aplicación. Los datos almacenados en el almacenamiento persistente se comparten entre todas las instancias de la aplicación. Las instancias de servicio de Azure Spring Cloud pueden tener un máximo de 10 aplicaciones con disco persistente habilitado y cada una de estas aplicaciones tiene 50 GB de almacenamiento persistente. La ruta de montaje predeterminada para el almacenamiento persistente es `/persistent`.

## <a name="enable-persistent-storage-using-the-azure-portal"></a>Habilitación del almacenamiento persistente mediante Azure Portal

1. En la página del servicio Azure Spring Cloud, seleccione **Panel de la aplicación** y, después, seleccione la aplicación que requiere almacenamiento persistente.
1. En la pestaña **Información general**, busque el atributo **Persistent Storage** (Almacenamiento persistente) y seleccione **Disabled** (Deshabilitado).
1. Haga clic en **Enable** (Habilitar) para habilitar el almacenamiento persistente y seleccione el botón **Aceptar** para aplicar el cambio.

Cuando se habilita el almacenamiento persistente, tanto su tamaño como su ruta de acceso se muestran en el atributo **Persistent Storage** (Almacenamiento persistente) de la página **Información general**.

> [!WARNING]
> Al *deshabilitar* el almacenamiento persistente, se desasignará el almacenamiento de la aplicación.  Se perderán todos los datos de esa cuenta de almacenamiento. 

## <a name="enable-persistent-storage-using-the-azure-cli"></a>Habilitación del almacenamiento persistente mediante la CLI de Azure

Cree una aplicación con un disco persistente habilitado:
 
```azurecli
az spring-cloud app create -n <app> -g <resource-group> -s <service-name> --enable-persistent-storage true
```

Habilite el almacenamiento persistente en una aplicación existente:

```azurecli
az spring-cloud app update -n <app> -g <resource-group> -s <service-name> --enable-persistent-storage true
```

Deshabilite el almacenamiento persistente en una aplicación existente:

> [!WARNING]
> Al deshabilitar el almacenamiento persistente, se desasignará el almacenamiento de la aplicación, con lo que se perderán permanentemente todos los datos almacenados en él. 

```azurecli
az spring-cloud app update -n <app> -g <resource-group> -s <service-name> --enable-persistent-storage false
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre la [aplicación y las cuotas de servicio](spring-cloud-quotas.md), o bien aprenda a [escalar manualmente su aplicación](spring-cloud-tutorial-scale-manual.md).