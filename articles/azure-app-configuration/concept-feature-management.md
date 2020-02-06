---
title: Administración de características de Azure App Configuration
description: Información general sobre cómo se puede usar Azure App Configuration para activar y desactivar las características de la aplicación a petición.
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 04/19/2019
ms.openlocfilehash: 85228854f6c106c68cedc3eeea81e15fd662774a
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76898738"
---
# <a name="feature-management-overview"></a>Introducción a la administración de características

Tradicionalmente, enviar una nueva característica de aplicación requiere una reimplementación completa de la propia aplicación. Para probar una característica, es probable que tenga que implementar su aplicación muchas veces para controlar cuándo se hace visible o quién puede verla.

La administración de características es una práctica moderna de desarrollo de software que separa el lanzamiento de las características de la implementación del código y permite hacer cambios rápidos relacionados con la disponibilidad de las características a petición. Usa una técnica denominada *marcas de características* (también conocida como *activación/desactivación de funcionalidad* o *modificador de características*, entre otros) para administrar el ciclo de vida de una característica dinámicamente.

La administración de características ayuda a los desarrolladores con los siguientes problemas:

* **Administración de la rama de código**: Las marcas de características se usan con frecuencia para ajustar la nueva funcionalidad de la aplicación que está en desarrollo. Esta funcionalidad se "oculta" de forma predeterminada. La característica se puede enviar de forma segura, aunque esté sin terminar, y permanecer inactiva en producción. Con este enfoque, denominado *implementación oscura*, se puede liberar todo el código al final de cada ciclo de desarrollo. Ya no es necesario mantener ninguna rama de código en varios ciclos porque una característica tarde más de un ciclo en completarse.
* **Realice pruebas en producción**: Puede utilizar las marcas de características para conceder acceso anticipado a la nueva funcionalidad en producción. Por ejemplo, puede limitar este acceso solo a los miembros del equipo o evaluadores beta internos. Estos usuarios obtendrán la experiencia de producción de fidelidad total, en lugar de una experiencia simulada o parcial en un entorno de prueba.
* **Lanzamiento como paquete piloto**: También puede usar las marcas de características para implementar una nueva funcionalidad en los usuarios finales de manera incremental. Puede dirigirse a un pequeño porcentaje de la población de usuarios en primer lugar y aumentar ese porcentaje gradualmente con el tiempo, una vez que haya adquirido más confianza en la aplicación.
* **Conmutador de eliminación instantánea**: Las marcas de características proporcionan una red de seguridad inherente para liberar la nueva funcionalidad. Con ellas, puede activar y desactivar fácilmente las características de la aplicación. Si es necesario, puede deshabilitar rápidamente una característica sin volver a generar e implementar la aplicación.
* **Activación selectiva**: Si bien la mayoría de las marcas de características solo existen hasta que sus funcionalidades asociadas se han publicado con éxito, algunas pueden durar mucho tiempo. Se pueden usar para segmentar los usuarios y ofrecer un conjunto específico de características a cada grupo. Por ejemplo, puede tener una característica que funcione solo en un determinado explorador web. Puede definir una marca de características para que solo los usuarios de ese explorador puedan ver y usar la característica. Con este enfoque, puede ampliar fácilmente la lista de exploradores admitidos más adelante sin tener que cambiar ningún código.

## <a name="basic-concepts"></a>Conceptos básicos

Estos son varios términos nuevos relacionados con la administración de características:

* **Marca de características**: una marca de características es una variable con un estado binario de *activado* o *desactivado*. La marca de características también tiene un bloque de código asociado. El estado de la marca de características se desencadena tanto si el bloque de código se ejecuta como si no.
* **Administrador de características**: Un administrador de características es un paquete de aplicación que controla el ciclo de vida de todas las marcas de características en una aplicación. El administrador de características normalmente proporciona funcionalidad adicional, como el almacenamiento en caché de las marcas de características y la actualización de sus estados.
* **Filtro**: Un filtro es una regla para evaluar el estado de una marca de características. Un grupo de usuarios, un tipo de explorador o dispositivo, una ubicación geográfica y un período de tiempo son todos ellos ejemplos de lo que puede representar un filtro.

Una implementación eficaz de la administración de características consta de al menos dos componentes que funcionan conjuntamente:

* Una aplicación que hace uso de las marcas de características.
* Un repositorio independiente que almacena las marcas de características y sus estados actuales.

En los ejemplos siguientes se muestra cómo interactúan estos componentes.

## <a name="feature-flag-usage-in-code"></a>Uso de la marca de características en el código

El patrón básico para implementar las marcas de características en una aplicación es sencillo. Puede considerar una marca de características como una variable de estado booleana usada con una declaración condicional `if` en su código:

```csharp
if (featureFlag) {
    // Run the following code
}
```

En este caso, si `featureFlag` está establecido en `True`, se ejecuta el bloque de código incluido; en caso contrario, se omite. Puede establecer el valor de `featureFlag` estáticamente, como se muestra en el ejemplo de código siguiente:

```csharp
bool featureFlag = true;
```

También puede evaluar el estado de la marca según ciertas reglas:

```csharp
bool featureFlag = isBetaUser();
```

Un patrón de marca de características un poco más complicado incluye también una instrucción `else`:

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
} else {
    // This following code will run if the featureFlag value is false
}
```

Sin embargo, puede volver a escribir este comportamiento en el patrón básico. En el artículo [Uso de marcas de características en una aplicación de ASP.NET Core](./use-feature-flags-dotnet-core.md) se muestran las ventajas de estandarizar un patrón de código simple. Por ejemplo:

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
}

if (!featureFlag) {
    // This following code will run if the featureFlag value is false
}
```

## <a name="feature-flag-repository"></a>Repositorio de marcas de características

Para usar marcas de características de forma eficaz, debe externalizar todas las marcas de características usadas en una aplicación. Ese enfoque le permite cambiar los estados de las marcas de características sin modificar y volver a implementar la propia aplicación.

Azure App Configuration está diseñado para ser un repositorio centralizado para las marcas de características. Puede usarla para definir distintos tipos de marcas de características y manipular sus estados con rapidez y confianza. Luego, puede usar las bibliotecas de App Configuration con diversos marcos de lenguajes de programación para acceder fácilmente a estas marcas de características desde su aplicación.

En [Use feature flags in an ASP.NET Core app](./use-feature-flags-dotnet-core.md) (Uso de marcas de características en una aplicación de ASP.NET Core) se muestra cómo se usan juntos el proveedor de App Configuration de .NET Core y las bibliotecas de administración de características para implementar las marcas de características para la aplicación web de ASP.NET.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Add feature flags to an ASP.NET Core web app](./quickstart-feature-flag-aspnet-core.md) (Adición de marcas de características a una aplicación web ASP.NET Core)  
