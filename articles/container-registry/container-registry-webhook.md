---
title: Webhooks de Azure Container Registry
description: Aprenda a usar webhooks para desencadenar eventos cuando determinadas acciones tengan lugar en uno de los repositorios del registro.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/14/2019
ms.author: danlep
ms.openlocfilehash: 0a3d2d0e858dc052095c0a58287970d10c06f0ba
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58099855"
---
# <a name="using-azure-container-registry-webhooks"></a>Webhooks de Azure Container Registry

Un registro de contenedor de Azure almacena y administra imágenes privadas de contenedor de Docker, de una forma similar a la que Docker Hub almacena imágenes públicas. También pueden hospedar los repositorios para [gráficos de Helm](container-registry-helm-repos.md) (versión preliminar), dar formato a un paquete para implementar aplicaciones en Kubernetes. Puede usar webhooks para desencadenar eventos cuando determinadas acciones tienen lugar en uno de los repositorios de registro. Los webhooks pueden responder a eventos en el nivel de registro o pueden limitarse a una etiqueta de repositorio específica.

Para obtener información detallada sobre las solicitudes de webhook, consulte [Referencia de esquema de webhook de Azure Container Registry](container-registry-webhook-reference.md).

## <a name="prerequisites"></a>Requisitos previos

* Registro de contenedor de Azure: cree un registro de contenedor en la suscripción de Azure. Por ejemplo, use [Azure Portal](container-registry-get-started-portal.md) o la [CLI de Azure](container-registry-get-started-azure-cli.md). El [SKU de Azure Container Registry](container-registry-skus.md) tienen cuotas diferentes webhooks.
* CLI de docker: para configurar el equipo local como un host de Docker y tener acceso a los comandos de la CLI de Docker, instale [Docker Engine](https://docs.docker.com/engine/installation/).

## <a name="create-webhook---azure-portal"></a>Creación de webhook: portal de Azure

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Vaya al registro de contenedor en el que desea crear un webhook.
1. En **servicios**, seleccione **Webhooks**.
1. Seleccione **Agregar** en la barra de herramientas del webhook.
1. Complete el formulario *Crear webhook* con la siguiente información:

| Valor | DESCRIPCIÓN |
|---|---|
| NOMBRE | El nombre que desea dar al webhook. Puede contener solo letras y números y debe estar entre 5 y 50 caracteres de longitud. |
| URI de servicio | El identificador URI donde el webhook debe enviar notificaciones POST. |
| Encabezados personalizados | Los encabezados que van a pasar junto con la solicitud POST. Deben tener el formato "clave: valor". |
| Acciones de desencadenador | Acciones que desencadenan el webhook. Acciones incluyen la inserción de la imagen, eliminación de imágenes, inserción de gráfico de Helm, delete del gráfico de Helm y cuarentena de la imagen. Puede elegir una o varias acciones que desencadenan el webhook. |
| Status | El estado del webhook después de su creación. Esto está habilitada de manera predeterminada. |
| Ámbito | El ámbito en el que trabaja el webhook. Si no se especifica, el ámbito es para todos los eventos en el registro. Puede especificar para un repositorio o etiqueta con el formato "repositorio: etiqueta" o "repositorio: *" para todas las etiquetas en un repositorio. |

Formulario de webhook de ejemplo:

![Interfaz de usuario de creación de webhook ACR en Azure Portal](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook---azure-cli"></a>Creación de webhook: CLI de Azure

Para crear un webhook mediante la CLI de Azure, use el comando [az acr webhook create](/cli/azure/acr/webhook#az-acr-webhook-create). El siguiente comando crea un webhook para todos los eventos de eliminación en el registro de imagen *mycontainerregistry*:

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a>Webhook de prueba

### <a name="azure-portal"></a>Azure Portal

Antes de usar el webhook, puede probarlo con el **Ping** botón. El comando Ping envía una solicitud POST genérica al punto de conexión especificado y registra la respuesta. Mediante la característica de ping puede comprobar que ha configurado correctamente el webhook.

1. Seleccione el webhook que desea probar.
2. En la barra de herramientas superior, seleccione **Ping**.
3. Compruebe la respuesta del punto de conexión en la columna **ESTADO HTTP**.

![Interfaz de usuario de creación de webhook ACR en Azure Portal](./media/container-registry-webhook/webhook-02.png)

### <a name="azure-cli"></a>Azure CLI

Para probar un webhook de ACR con la CLI de Azure, use el comando [az acr webhook ping](/cli/azure/acr/webhook#az-acr-webhook-ping).

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

Para ver los resultados, use el comando [az acr webhook list-events](/cli/azure/acr/webhook).

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a>Eliminación del webhook

### <a name="azure-portal"></a>Azure Portal

Se puede eliminar cada webhook seleccionando uno y, después, con el botón **Eliminar** en Azure Portal.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```

## <a name="next-steps"></a>Pasos siguientes

### <a name="webhook-schema-reference"></a>Referencia del esquema de webhook

Para más información sobre el formato y las propiedades de las cargas del evento JSON emitidas por Azure Container Registry, consulte la referencia del esquema de webhook:

[Referencia de esquema de webhook de Azure Container Registry](container-registry-webhook-reference.md)

### <a name="event-grid-events"></a>Eventos de Event Grid

Además de los eventos de webhook de registro nativos descritos en este artículo, Azure Container Registry puede emitir eventos a Event Grid:

[Inicio rápido: Envío de eventos del registro de contenedor a Event Grid](container-registry-event-grid-quickstart.md)
