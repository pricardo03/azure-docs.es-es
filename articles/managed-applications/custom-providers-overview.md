---
title: Introducción a los proveedores personalizados de Azure en versión preliminar
description: Se describen los conceptos para crear un proveedor de recursos personalizado con Azure Resource Manager
author: MSEvanhi
ms.service: managed-applications
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: evanhi
ms.openlocfilehash: bbfb10f612690af0f4fd3683e0f58986a21048d8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65159861"
---
# <a name="azure-custom-providers-preview-overview"></a>Introducción a los proveedores personalizados de Azure en versión preliminar

Con los proveedores personalizados de Azure, puede ampliar Azure para trabajar con el servicio. Puede crear un proveedor de recursos propio que incluya tipos de recurso y acciones personalizados. El proveedor personalizado se integra con Azure Resource Manager. Puede usar las características de Resource Manager, como las implementaciones de plantilla y el control de acceso basado en rol, para implementar y proteger el servicio.

En este artículo se proporciona información general de los proveedores personalizados y sus funcionalidades. En la imagen siguiente se muestra el flujo de trabajo para los recursos y las acciones que se definen en un proveedor personalizado.

![Información general del proveedor personalizado](./media/custom-providers-overview/overview.png)

> [!IMPORTANT]
> La función de proveedores personalizados está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="define-your-custom-provider"></a>Definición del proveedor personalizado

Para empezar, Azure Resource Manager debe ser consciente de la presencia del proveedor personalizado. Implemente un recurso de proveedor personalizado en Azure, que usa el tipo de recurso de **Microsoft.CustomProviders/resourceProviders**. En ese recurso, defina los recursos y las acciones para el servicio.

Por ejemplo, si el servicio necesita un tipo de recurso denominado **usuarios**, inclúyalo en la definición del proveedor personalizado. Para cada tipo de recurso, debe proporcionar un punto de conexión que ofrezca las operaciones de REST (PUT, GET, DELETE) para ese tipo de recurso. El punto de conexión se puede hospedar en cualquier entorno, y contiene la lógica de cómo administra el servicio las operaciones en el tipo de recurso.

También puede definir acciones personalizadas para el proveedor de recursos. Las acciones representan operaciones POST. Use acciones para operaciones como iniciar, detener o reiniciar. Proporcione un punto de conexión que controle la solicitud.

En el ejemplo siguiente se muestra cómo definir un proveedor personalizado con una acción y un tipo de recurso.

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

Para **routingType**, los valores aceptados son `Proxy` y `Cache`. Proxy significa que las solicitudes para el tipo de recurso o la acción las controla el punto de conexión. La configuración de caché solo se admite para tipos de recursos, no para acciones. Para especificar la caché, también debe especificar el proxy. La caché significa que las respuestas desde el punto de conexión se almacenan para optimizar las operaciones de lectura. Mediante la configuración de caché es más fácil implementar una API que sea coherente y compatible con otros servicios de Resource Manager.

## <a name="deploy-your-resource-types"></a>Implementación de los tipos de recursos

Después de definir el proveedor personalizado, puede implementar los tipos de recursos personalizados. En el ejemplo siguiente se muestra el código JSON que se incluye en la plantilla para implementar el tipo de recurso para el proveedor personalizado. Este tipo de recurso se puede implementar en la misma plantilla con otros recursos de Azure.

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

Use el [control de acceso basado en rol](../role-based-access-control/overview.md) de Azure para administrar el acceso al proveedor de recursos. Puede asignar [roles integrados](../role-based-access-control/built-in-roles.md) como Propietario, Colaborador o Lector a los usuarios. O bien, puede definir [roles personalizados](../role-based-access-control/custom-roles.md) que sean específicos de las operaciones en el proveedor de recursos.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha obtenido información sobre los proveedores personalizados. Vaya al artículo siguiente para crear un proveedor personalizado.

> [!div class="nextstepaction"]
> [Tutorial: Creación de un proveedor de recursos e implementación de recursos personalizados](create-custom-provider.md)
