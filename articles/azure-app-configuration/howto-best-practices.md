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
ms.openlocfilehash: 3d9a597e7ced631627a121f3f0757e472f9a4bae
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393590"
---
# <a name="azure-app-configuration-best-practices"></a>Prácticas recomendadas de configuración de la aplicación de Azure

Este artículo describen patrones comunes y procedimientos recomendados cuando se usa la configuración de aplicación de Azure.

## <a name="key-groupings"></a>Agrupaciones de clave

Configuración de la aplicación proporciona dos opciones para organizar las claves:

* Prefijos de clave
* Etiquetas

Puede usar una o ambas opciones para agrupar sus claves.

*Los prefijos de clave* son las partes del principio de claves. Puede agrupar lógicamente un conjunto de claves utilizando el mismo prefijo en sus nombres. Los prefijos pueden contener varios componentes conectados por un delimitador, como `/`, de forma similar a una ruta de dirección URL, para formar un espacio de nombres. Estas jerarquías son útiles cuando se está almacenando las claves para muchas aplicaciones, servicios de componentes y entornos en un almacén de configuración de la aplicación.

Una cuestión importante que debe tener en cuenta es que las claves son lo que hace referencia el código de aplicación para recuperar los valores de la configuración correspondiente. No deberían cambiar las claves, o bien tendrá que modificar el código cada vez que ocurre.

*Las etiquetas* son un atributo en las claves. Se usan para crear variantes de una clave. Por ejemplo, puede asignar etiquetas a varias versiones de una clave. Una versión podría ser una iteración, un entorno o cualquier otra información contextual. La aplicación puede solicitar un conjunto completamente distinto de los valores de clave mediante la especificación de otra etiqueta. Como resultado, todas las referencias de clave permanecen sin cambios en el código.

## <a name="key-value-compositions"></a>Composiciones de pares clave-valor

Configuración de la aplicación trata todas las claves que se almacenan con él como entidades independientes. Configuración de la aplicación no intenta deducir ninguna relación entre las claves o para heredar valores de clave en función de su jerarquía. Sin embargo, es posible agregar varios conjuntos de claves, mediante el uso de etiquetas junto con la configuración correcta de apilamiento en el código de aplicación.

Veamos un ejemplo. Suponga que tiene una configuración denominada **Asset1**, cuyo valor puede variar según el entorno de desarrollo. Cree una clave denominada "Asset1" con una etiqueta vacía y una etiqueta denominada "Desarrollo". En la primera etiqueta, coloca el valor predeterminado de **Asset1**, and debe incluir un valor específico para el "Desarrollo" en el último.

En el código, primero recupere los valores de clave sin ninguna etiqueta y, a continuación, recuperar el mismo conjunto de valores de clave, una segunda vez con la etiqueta "Desarrollo". Cuando se recuperan los valores de la segunda vez, se sobrescriben los valores de las claves anteriores. El sistema de configuración de .NET Core permite "apilar" varios conjuntos de datos de configuración en la parte superior entre sí. Si existe una clave en más de un conjunto, se utiliza el último conjunto que lo contiene. Con un marco de programación modernos, como .NET Core, obtiene esta capacidad de apilamiento de forma gratuita si usa un proveedor de configuración nativo para tener acceso a la configuración de la aplicación. El fragmento de código siguiente muestra cómo puede implementar el apilamiento en una aplicación .NET Core:

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

Para obtener acceso a un almacén de configuración de la aplicación, puede usar su cadena de conexión, que está disponible en el portal de Azure. Dado que las cadenas de conexión contienen información de credenciales, se consideran secretos. Estos secretos deben almacenarse en Azure Key Vault y el código debe autenticarse en Key Vault para recuperarlos.

Una mejor opción es usar la característica de las identidades administradas en Azure Active Directory. Con identidades administradas, necesita solo en la URL del extremo de configuración de la aplicación para acceso de arranque a su almacén de configuración de la aplicación. Puede insertar la dirección URL en el código de aplicación (por ejemplo, en el *appsettings.json* archivo). Consulte [integrar con Azure administra las identidades](howto-integrate-azure-managed-service-identity.md) para obtener más información.

## <a name="app-or-function-access-to-app-configuration"></a>Función o aplicación acceso a la configuración de aplicación

Puede proporcionar acceso a la configuración de la aplicación para aplicaciones web o las funciones mediante cualquiera de los métodos siguientes:

* A través del portal de Azure, escriba la cadena de conexión en su almacén de configuración de la aplicación en la configuración de la aplicación de App Service.
* Store de la cadena de conexión en su almacén de configuración de la aplicación en Key Vault y [haga referencia a él desde App Service](https://docs.microsoft.com/azure/app-service/app-service-key-vault-references).
* Uso de Azure administra las identidades para acceder al almacén de configuración de la aplicación. Para obtener más información, consulte [integrar con Azure administra las identidades](howto-integrate-azure-managed-service-identity.md).
* Insertar configuración de la configuración de la aplicación en App Service. Configuración de la aplicación proporciona una función de exportación (en Azure portal y la CLI de Azure) que envía datos directamente a App Service. Con este método, no es necesario cambiar el código de aplicación en absoluto.

## <a name="next-steps"></a>Pasos siguientes

* [Las claves y valores](./concept-key-value.md)
