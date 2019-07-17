---
title: Creación de acciones y recursos personalizados en Azure
description: En este tutorial se explica cómo crear acciones personalizadas y recursos en Azure Resource Manager y cómo integrarlos en flujos de trabajo personalizados para plantillas de Azure Resource Manager, la CLI de Azure, Azure Policy y el registro de actividad.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 4bbfcf070611e3df5c0fe47070f2ab6961111e07
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799194"
---
# <a name="create-custom-actions-and-resources-in-azure"></a>Creación de acciones y recursos personalizados en Azure

Los proveedores personalizados le permiten personalizar los flujos de trabajo en Azure. Un proveedor personalizado es un contrato entre Azure y un `endpoint`. Permite la adición de nuevas API personalizadas en Azure Resource Manager para habilitar las nuevas funcionalidades de implementación y administración. Este tutorial le guiará por un ejemplo sencillo de cómo agregar nuevas acciones y recursos a Azure y cómo integrarlos.

Este tutorial se divide en los pasos siguientes:

- Configuración de Azure Functions para los proveedores personalizados de Azure
- Creación de un punto de conexión de RESTful para proveedores personalizados
- Creación y uso del proveedor personalizado

## <a name="setup-azure-functions-for-azure-custom-providers"></a>Configuración de Azure Functions para los proveedores personalizados de Azure

En esta parte del tutorial se describe con detalle cómo configurar una función de Azure para que funcione con proveedores personalizados. Los proveedores personalizados pueden funcionar con cualquier dirección URL pública.

- [Configuración de Azure Functions para los proveedores personalizados de Azure](./tutorial-custom-providers-function-setup.md)

## <a name="authoring-a-restful-endpoint-for-custom-providers"></a>Creación de un punto de conexión de RESTful para proveedores personalizados

En esta parte del tutorial encontrará información detallada sobre la creación de un punto de conexión de RESTful para proveedores personalizados.

- [Creación de un punto de conexión de RESTful para proveedores personalizados](./tutorial-custom-providers-function-authoring.md)

## <a name="creating-and-utilizing-the-custom-provider"></a>Creación y uso del proveedor personalizado

En esta parte del tutorial se describe con detalle cómo crear un proveedor personalizado y cómo usar sus acciones y recursos personalizado.

- [Creación y uso de proveedores personalizados de Azure](./tutorial-custom-providers-create.md)

## <a name="next-steps"></a>Pasos siguientes

En este artículo, hemos aprendido sobre los proveedores personalizados y cómo crear uno. Para continuar con el siguiente paso del tutorial:

- [Tutorial: Configuración de Azure Functions para los proveedores personalizados de Azure](./tutorial-custom-providers-function-setup.md)

Si busca referencias o un inicio rápido, estos son algunos vínculos útiles:

- [Inicio rápido: Creación de un proveedor de recursos personalizados de Azure e implementación de recursos personalizados](./create-custom-provider.md)
- [How To: Adding Custom Actions to Azure REST API](./custom-providers-action-endpoint-how-to.md) (Procedimientos: Adición de acciones personalizadas a la API REST de Azure)
- [How To: Adding Custom Resources to Azure REST API](./custom-providers-resources-endpoint-how-to.md) (Procedimientos: Adición de recursos personalizados a la API REST de Azure)
