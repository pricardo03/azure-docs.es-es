---
title: Información general de la vista previa de los proveedores personalizados de Azure
description: Describe los conceptos para crear un proveedor de recursos personalizados con Azure Resource Manager
author: MSEvanhi
ms.service: managed-applications
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: evanhi
ms.openlocfilehash: bbfb10f612690af0f4fd3683e0f58986a21048d8
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159861"
---
# <a name="azure-custom-providers-preview-overview"></a>Introducción a Azure de vista previa de los proveedores personalizados

Con proveedores personalizados de Azure, puede ampliar Azure para trabajar con el servicio. Cree su propio proveedor de recursos, incluidos los tipos de recurso personalizado y las acciones. El proveedor personalizado se integra con Azure Resource Manager. Puede usar las características del Administrador de recursos, como las implementaciones de plantilla y control de acceso basado en roles, implementar y proteger el servicio.

En este artículo se proporciona información general de los proveedores personalizados y sus funcionalidades. La siguiente imagen muestra el flujo de trabajo para los recursos y las acciones definidos en un proveedor personalizado.

![Información general del proveedor personalizado](./media/custom-providers-overview/overview.png)

> [!IMPORTANT]
> Proveedores personalizados está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="define-your-custom-provider"></a>Definir su proveedor personalizado

Primero, ya que Azure Resource Manager permite saber acerca de su proveedor personalizado. Implementar en Azure un recurso del proveedor personalizado, que usa el tipo de recurso de **Microsoft.CustomProviders/resourceProviders**. En ese recurso, definir los recursos y las acciones para el servicio.

Por ejemplo, si el servicio necesita un tipo de recurso denominado **usuarios**, incluya ese tipo de recurso en la definición de proveedor personalizado. Para cada tipo de recurso, debe proporcionar un punto de conexión que ofrece las operaciones de REST (PUT, GET, DELETE) para ese tipo de recurso. El punto de conexión se puede hospedar en cualquier entorno, y contiene la lógica de cómo el servicio administra las operaciones en el tipo de recurso.

También puede definir las acciones personalizadas para el proveedor de recursos. Las acciones representan operaciones POST. Usar acciones para operaciones como iniciar, detener o reiniciar. Proporcionar un punto de conexión que controla la solicitud.

El ejemplo siguiente muestra cómo definir un proveedor personalizado con una acción y un tipo de recurso.

```json
{
  "apiVersion": "2018-09-01-preview",
  "type": "Microsoft.CustomProviders/resourceProviders",
  "name": "[parameters('funcName')]",
  "location": "[parameters('location')]",
  "properties": {
    "actions": [
      {
        "name": "ping",
        "routingType": "Proxy",
        "endpoint": "[concat('https://', parameters('funcName'), '.azurewebsites.net/api/{requestPath}')]"
      }
    ],
    "resourceTypes": [
      {
        "name": "users",
        "routingType": "Proxy,Cache",
        "endpoint": "[concat('https://', parameters('funcName'), '.azurewebsites.net/api/{requestPath}')]"
      }
    ]
  }
},
```

Para **routingType**, los valores aceptados son `Proxy` y `Cache`. Proxy significa que las solicitudes para el tipo de recurso o acción se controlan mediante el punto de conexión. La configuración de caché solo se admite para tipos de recursos, no acciones. Para especificar la memoria caché, también debe especificar a servidor proxy. Caché significa que almacena las respuestas desde el punto de conexión para optimizar las operaciones de lectura. Mediante la configuración de la memoria caché hace más fácil de implementar una API que sea coherente y compatible con otros servicios de Resource Manager.

## <a name="deploy-your-resource-types"></a>Implementar los tipos de recursos

Después de definir su proveedor personalizado, puede implementar los tipos de recursos personalizados. El ejemplo siguiente se muestra el JSON que que incluyen en la plantilla para implementar el tipo de recurso para el proveedor personalizado. Este tipo de recurso puede implementarse en la misma plantilla con otros recursos de Azure.

```json
{
    "apiVersion": "2018-09-01-preview",
    "type": "Microsoft.CustomProviders/resourceProviders/users",
    "name": "[concat(parameters('rpname'), '/santa')]",
    "location": "[parameters('location')]",
    "properties": {
        "FullName": "Santa Claus",
        "Location": "NorthPole"
    }
}
```

## <a name="manage-access"></a>Administración del acceso

Use Azure [control de acceso basado en roles](../role-based-access-control/overview.md) para administrar el acceso a su proveedor de recursos. Puede asignar [roles integrados](../role-based-access-control/built-in-roles.md) como propietario, Colaborador o lector a los usuarios. O bien, puede definir [roles personalizados](../role-based-access-control/custom-roles.md) que son específicas de las operaciones en el proveedor de recursos.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido acerca de los proveedores personalizados. Vaya al siguiente artículo para crear un proveedor personalizado.

> [!div class="nextstepaction"]
> [Tutorial: Crear un proveedor personalizado e implementar recursos personalizados](create-custom-provider.md)
