---
title: Prácticas recomendadas de configuración de la aplicación de Azure | Microsoft Docs
description: Obtenga información sobre cómo usar mejor configuración de la aplicación de Azure
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: d1275a48de5cad9321186ba20860d853b8ce55ad
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65413621"
---
# <a name="azure-app-configuration-best-practices"></a>Prácticas recomendadas de configuración de la aplicación de Azure

Este artículo describen las prácticas y patrones comunes al utilizar la configuración de la aplicación de Azure.

## <a name="key-groupings"></a>Agrupaciones de clave

Configuración de la aplicación proporciona dos opciones para organizar las claves: prefijos o etiquetas de clave. Puede usar uno o ambos.

Los prefijos de clave son las partes del principio de claves. Puede agrupar lógicamente un conjunto de claves utilizando el mismo prefijo en sus nombres. Los prefijos pueden contener varios componentes conectados entre sí por un delimitador, como `/`, de forma similar a una ruta de dirección URL, para formar un espacio de nombres. Estas jerarquías son útiles para almacenar las claves para muchas aplicaciones, servicios de componentes y entornos en un almacén de configuración de la aplicación. Una cuestión importante que debe tener en cuenta es que las claves son lo que hace referencia el código de aplicación para recuperar los valores de la configuración correspondiente. No debe cambiar una clave o bien tendrá que modificar el código cada vez que ocurre.

Las etiquetas es un atributo de claves. Se utilizan para crear variantes de una clave. Por ejemplo, puede asignar etiquetas a varias versiones de una clave. Una versión puede ser una iteración, el entorno o cualquier otra información contextual. La aplicación puede solicitar un conjunto completamente distinto de valores de clave especificando otra etiqueta. Todas las referencias de clave pueden permanecer sin cambios.

## <a name="key-value-compositions"></a>Composiciones de pares clave-valor

Configuración de la aplicación trata todas las claves que se almacenan con él como entidades independientes. No intente deducir ninguna relación entre las claves o heredar valores de clave en función de su jerarquía. Puede agregar varios conjuntos de claves, sin embargo, con las etiquetas junto con la configuración correcta de apilamiento en el código de aplicación.

Veamos un ejemplo. Tiene una configuración **Asset1** cuyo valor puede variar según el entorno de "Desarrollo". Puede crear una clave denominada "Asset1" con una etiqueta vacía y una etiqueta denominada "Desarrollo". Coloque el valor predeterminado de **Asset1** en el primero y cualquier valor específico para el "Desarrollo" en el último. En el código, primero hay que recuperar los valores de clave sin ninguna etiqueta y, a continuación, aquellos con una etiqueta de "Desarrollo" para sobrescribir los valores anteriores de las mismas claves. Si usa un marco de programación modernos, como .NET Core, puede obtener esta funcionalidad de apilamiento de forma gratuita si usa un proveedor de configuración nativo para tener acceso a la configuración de la aplicación. El fragmento de código siguiente muestra cómo puede implementar en una aplicación .NET Core de apilamiento.

```csharp
// Augment the ConfigurationBuilder with Azure App Configuration
// Pull the connection string from an environment variable
configBuilder.AddAzureAppConfiguration(options => {
    options.Connect(configuration["connection_string"])
           .Use(KeyFilter.Any, LabelFilter.Null)
           .Use(KeyFilter.Any, "Development");
});
```

## <a name="app-configuration-bootstrap"></a>Bootstrap de configuración de aplicación

Para obtener acceso a un almacén de configuración de la aplicación, puede usar su cadena de conexión, que está disponible en el portal de Azure. Las cadenas de conexión contienen información de credenciales y se consideran como secretos. Deben almacenarse en un almacén de claves. Una mejor opción es usar Azure identidad administrada. Con este método, solo necesita dirección URL del extremo para arrancar el acceso al almacén de configuración de la configuración de la aplicación. Puede insertar la dirección URL en el código de aplicación (por ejemplo, en el *appsettings.json* archivo). Consulte [integrar con Azure administra las identidades](howto-integrate-azure-managed-service-identity.md) para obtener más detalles.

## <a name="web-app-or-function-access-to-app-configuration"></a>Aplicación Web o función acceso a la configuración de aplicación

Puede escribir la cadena de conexión en su almacén de configuración de la aplicación en la configuración de la aplicación de App Service a través del portal de Azure. También puede almacenar en Key Vault y [haga referencia a él desde App Service](https://docs.microsoft.com/azure/app-service/app-service-key-vault-references). También puede usar Azure para acceder al almacén de configuración de la identidad administrada. Consulte [integrar con Azure administra las identidades](howto-integrate-azure-managed-service-identity.md) para obtener más detalles.

Como alternativa, puede insertar la configuración de la configuración de la aplicación en App Service. Configuración de la aplicación proporciona una función de exportación (en Azure portal y CLI) que envía datos directamente a App Service. Con este método, no es necesario cambiar el código de aplicación en absoluto.

## <a name="next-steps"></a>Pasos siguientes

* [Las claves y valores](./concept-key-value.md)
