---
title: Creación de un clúster y base de datos de Azure Data Explorer mediante la CLI
description: En este artículo se describe cómo crear un clúster y base de datos de Azure Data Explorer mediante la CLI de Azure
services: data-explorer
author: radennis
ms.author: radennis
ms.reviewer: orspod
ms.service: data-explorer
ms.topic: howto
ms.date: 1/31/2019
ms.openlocfilehash: a4c9156ef80f05e247b1cfef0acd56b601a2db65
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55812691"
---
# <a name="create-an-azure-data-explorer-cluster-and-a-database-using-cli"></a>Creación de un clúster y base de datos de Azure Data Explorer mediante la CLI

En este artículo se describe cómo crear un clúster y base de datos de Azure Data Explorer mediante la CLI de Azure.

## <a name="whats-the-difference-between-the-management-plane-and-data-plane-apis"></a>Cuál es la diferencia entre el plano de administración y las API de plano de datos

Hay dos bibliotecas de API diferentes: las API de plano de administración y las de plano de datos.
Las API de administración se usan para administrar los recursos. Por ejemplo, para crear un clúster o una base de datos, eliminar una conexión de datos, cambiar el número de instancias, etcétera. Las API de plano de datos se usan para interactuar con los datos, ejecutar consultas, ingerir datos, etcétera.

## <a name="configure-the-cli-parameters"></a>Configuración de los parámetros de la CLI

Inicie sesión en su cuenta.

```Bash
az login
```

Prepare la suscripción en la que quiere crear el clúster.

```Bash
az account set --subscription "your_subscription"
```

## <a name="create-the-azure-data-explorer-cluster"></a>Creación del clúster de Azure Data Explorer

Cree el clúster mediante el siguiente comando.

```Bash
az kusto cluster create --name azureclitest --sku D11_v2 --resource-group testrg
```

Proporcione los valores siguientes.

   |**Configuración** | **Valor sugerido** | **Descripción del campo**|
   |---|---|---|
   | Nombre | *azureclitest* | Nombre que quiere para el clúster.|
   | sku | *D13_v2* | SKU que se usará para el clúster. |
   | resource-group | *testrg* | Nombre del grupo de recursos en el que se creará el clúster. |

Si quiere, existen varios parámetros opcionales que puede usar, como la capacidad del clúster, etcétera.

Para comprobar si el clúster se creó correctamente, puede ejecutar lo siguiente:

```Bash
az kusto cluster show --name azureclitest --resource-group testrg
```

Si el resultado contiene "provisioningState" con el valor "Succeeded", significa que el clúster se creó correctamente.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Creación de la base de datos en el clúster de Azure Data Explorer

Cree la base de datos mediante el siguiente comando.

```Bash
az kusto database create --cluster-name azureclitest --name clidatabase --resource-group testrg --soft-delete-period 3650:00:00:00 --hot-cache-period 3650:00:00:00
```

Proporcione los valores siguientes.

   |**Configuración** | **Valor sugerido** | **Descripción del campo**|
   |---|---|---|
   | cluster-name | *azureclitest* | Nombre del clúster en el que se va a crear la base de datos.|
   | Nombre | *clidatabase* | Nombre que quiere para la base de datos.|
   | resource-group | *testrg* | Nombre del grupo de recursos en el que se creará el clúster. |
   | soft-delete-period | *3650:00:00:00* | Cantidad de tiempo durante la que se deben conservar los datos con el fin de que estén disponibles para las consultas. |
   | hot-cache-period | *3650:00:00:00* | Cantidad de tiempo durante la que se deben conservar los datos en la memoria caché. |

Puede ver la base de datos que creó si ejecuta el siguiente código:

```Bash
az kusto database show --name clidatabase --resource-group testrg --cluster-name azureclitest
```

Listo. Ahora cuenta con un clúster y una base de datos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si tiene previsto seguir nuestros tutoriales y guías de inicio rápido, conserve los recursos que creó.

Cuando se elimina un clúster, también se eliminan todas las bases de datos en él. Use el siguiente comando para eliminar el clúster.

```Bash
az kusto cluster delete --name azureclitest --resource-group testrg
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Inicio rápido: Ingesta de datos del centro de eventos a Azure Data Explorer](ingest-data-event-hub.md).
