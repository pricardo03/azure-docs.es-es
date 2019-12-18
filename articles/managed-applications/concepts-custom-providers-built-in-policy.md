---
title: Implementación de asociaciones para proveedores personalizados de Azure mediante Azure Policy
description: Aprenda a implementar asociaciones para proveedores personalizados mediante el servicio Azure Policy.
author: msHich
ms.service: managed-applications
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: hich
ms.openlocfilehash: 94f7128a473266824242f52a9c50f09b2c24f1d4
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74932237"
---
# <a name="deploy-associations-for-a-custom-provider-using-azure-policy"></a>Implementación de asociaciones para proveedores personalizados mediante Azure Policy

Se pueden usar directivas de Azure para implementar asociaciones con el fin de asociar recursos a un proveedor personalizado. En este artículo se describe una directiva integrada que implementa asociaciones y el modo de usarla.

## <a name="built-in-policy-to-deploy-associations"></a>Directiva integrada para implementar asociaciones

La implementación de asociaciones para proveedores personalizados es una directiva integrada que se puede usar para asociar un recurso a un proveedor personalizado. La directiva acepta tres parámetros:

- Identificador del proveedor personalizado: es el identificador de recurso del proveedor personalizado al que se deben asociar los recursos.
- Tipos de recursos que se van a asociar: son la lista de tipos de recursos que se van a asociar al proveedor personalizado. Puede asociar varios tipos de recursos a un proveedor personalizado con la misma directiva.
- Prefijo de nombre de asociación: esta cadena es el prefijo que se va a agregar al nombre del recurso de asociación que se va a crear. El valor predeterminado es "DeployedByPolicy".

La directiva usa la evaluación DeployIfNotExists, que se ejecuta después de que un proveedor de recursos ha administrado una solicitud de creación o actualización de recursos y la evaluación ha devuelto un código de estado correcto. Luego, el recurso de asociación se implementa mediante una implementación de plantilla.
Para más información sobre las asociaciones, consulte [Información general sobre la incorporación de recursos de proveedores personalizados de Azure](./concepts-custom-providers-resourceonboarding.md).

## <a name="how-to-use-the-deploy-associations-built-in-policy"></a>Uso de la directiva integrada de implementación de asociaciones 

### <a name="prerequisites"></a>Requisitos previos
Si el proveedor personalizado necesitara permisos sobre el ámbito de la directiva para realizar una acción, la implementación del recurso de asociación de la directiva no funcionaría sin conceder los permisos.

### <a name="policy-assignment"></a>Asignación de directiva
Para usar la directiva integrada, cree una asignación de directiva y asigne las asociaciones de implementación a una directiva de proveedor personalizado. La directiva identifica entonces los recursos no compatibles e implementa la asociación de esos recursos.

![Asignación de la directiva integrada](media/builtin-policy/assign-builtin-policy-customprovider.png)

## <a name="getting-help"></a>Ayuda

Si tiene preguntas sobre el desarrollo de proveedores de recursos personalizados de Azure, haga su consulta en [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers). Puede que ya se haya respondido a una pregunta similar, así que compruébelo antes de publicar la suya. Agregue la etiqueta ```azure-custom-providers``` para obtener una respuesta rápida.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprenderá a usar la directiva integrada para implementar asociaciones. Para obtener más información, vea estos artículos:

- [Conceptos: Incorporación de recursos de proveedores personalizados de Azure](./concepts-custom-providers-resourceonboarding.md)
- [Tutorial: Incorporación de recursos con proveedores personalizados](./tutorial-custom-providers-resource-onboarding.md)
- [Tutorial: Creación de acciones y recursos personalizados en Azure](./tutorial-custom-providers-101.md)
- [Inicio rápido: Creación de un proveedor de recursos personalizados e implementación de recursos personalizados](./create-custom-provider.md)
- [Uso de Incorporación de acciones personalizadas a una API REST de Azure](./custom-providers-action-endpoint-how-to.md)
- [Uso de Incorporación de recursos personalizados a una API REST de Azure](./custom-providers-resources-endpoint-how-to.md)
