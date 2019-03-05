---
title: Almacén de pares clave-valor de Azure App Configuration | Microsoft Docs
description: Una introducción sobre cómo se almacenan los datos de configuración en Azure App Configuration
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: f04067358b0b2bae745727a5dd7a1f5554f9f70e
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884986"
---
# <a name="key-value-store"></a>almacén de pares clave-valor

Azure App Configuration almacena los datos de configuración como pares clave-valor, una manera sencilla pero flexible de representar varios tipos de configuraciones de la aplicación con las que los desarrolladores están familiarizados.

## <a name="keys"></a>Claves

Las claves sirven como nombre de los pares clave-valor y se usan para almacenar y recuperar los valores correspondientes. Es una práctica habitual la organización de las claves en un espacio de nombres jerárquico mediante un delimitador de caracteres (como `/` o `:`) según la convención que sea más adecuada para la aplicación. Azure App Configuration trata las claves como un todo. No analiza las claves para averiguar cómo están estructurados sus nombres o para aplicar ninguna regla en ellas.

El uso del almacén de configuración dentro de los marcos de trabajo de la aplicación puede dictar esquemas de nombres específicos para los pares clave-valor. Por ejemplo, el marco de trabajo de Spring Cloud de Java define los recursos `Environment` que aportan la configuración a una aplicación de Spring que se va a parametrizar mediante variables entre las que se incluyen el *nombre de la aplicación* y el *perfil*. Las claves de configuración relacionadas con Spring Cloud empiezan normalmente por estos dos elementos, separados por un delimitador.

Las claves almacenadas en App Configuration distinguen entre mayúsculas y minúsculas y son cadenas basadas en Unicode. Las claves *app1* y *App1* se consideran diferentes en un almacén de App Configuration. Téngalo en cuenta al utilizar las opciones de configuración en una aplicación ya que algunos marcos de trabajo administran las claves de configuración sin hacer distinción entre mayúsculas y minúsculas. Por ejemplo, el sistema de configuración de ASP.NET Core trata las claves como cadenas sin distinción de mayúsculas y minúsculas. Para evitar comportamientos impredecibles al realizar consultas en App Configuration dentro de una aplicación de ASP.NET Core, no se deben utilizar claves que solo se distingan por el uso de mayúsculas o minúsculas.

Puede usar cualquier carácter unicode en los nombres de clave que se especifican en App Configuration, excepto `*`, `,` y `\` que están reservados. Si tiene que incluir un carácter reservado, deberá especificar un carácter de escape como `\{Reserved Character}`. Hay un límite de tamaño combinado de diez mil caracteres en un par clave-valor. Esto incluye todos los caracteres de la clave, su valor y todos los atributos opcionales asociados. Dentro de este límite, puede tener múltiples niveles jerárquicos para las claves.

### <a name="designing-key-namespaces"></a>Diseño de espacios de nombres de clave

Hay dos enfoques generales en relación con la nomenclatura de las claves que se usa en los datos de configuración: plano o jerárquico. Estos métodos son muy similares desde la perspectiva de uso de la aplicación, pero el segundo enfoque ofrece una serie de ventajas:

* Es más fácil de leer. En lugar de una secuencia larga de caracteres, los delimitadores de un nombre de clave jerárquico funcionan como los espacios de una frase y proporcionan pausas naturales entre palabras.
* Es más fácil de administrar. Un nombre de clave jerárquico representa grupos lógicos de datos de configuración.
* Es más fácil de usar. Resulta más fácil escribir una consulta cuyos patrones coinciden con las claves de una estructura jerárquica y que recupera solo una porción de los datos de configuración. Además, muchas plataformas de programación más recientes tienen una compatibilidad nativa para datos de configuración jerárquicos tal que la aplicación puede hacer uso de conjuntos específicos de configuración.

Puede organizar jerárquicamente las claves de App Configuration de muchas formas. Puede pensar en dichas claves como [identificadores URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier). Cada clave jerárquica es una *ruta* de recurso que consta de uno o varios componentes unidos por delimitadores. Puede elegir qué carácter se debe usar como delimitador en función de qué aplicación, lenguaje o plataforma de programación necesita. Puede utilizar varios delimitadores para diferentes claves de App Configuration.

Estos son algunos ejemplos de cómo se pueden estructurar los nombres de claves en una jerarquía:

* Según los entornos.

        AppName:Test:DB:Endpoint
        AppName:Staging:DB:Endpoint
        AppName:Production:DB:Endpoint

* Según los servicios de los componentes.

        AppName:Service1:Test:DB:Endpoint
        AppName:Service1:Staging:DB:Endpoint
        AppName:Service1:Production:DB:Endpoint
        AppName:Service2:Test:DB:Endpoint
        AppName:Service2:Staging:DB:Endpoint
        AppName:Service2:Production:DB:Endpoint

* Según las regiones de implementación.

        AppName:Production:Region1:DB:Endpoint
        AppName:Production:Region2:DB:Endpoint

### <a name="versioning-key-values"></a>Control de versiones de pares clave-valor

Los pares clave-valor de App Configuration pueden tener opcionalmente un atributo **label**. Las etiquetas se utilizan para diferenciar los pares clave-valor con la misma clave. Una clave *app1* con las etiquetas *v1* y *v2* forman dos pares clave-valor independientes en un almacén de App Configuration. De forma predeterminada, la etiqueta de un par clave-valor está vacía (o es `null`).

App Configuration no realiza un control de versiones de pares clave-valor automáticamente en caso de que estas se modifiquen. Puede usar etiquetas como forma de crear varias versiones de un par clave-valor. Por ejemplo, puede escribir un número de versión de la aplicación o un identificador de confirmación de Git en las etiquetas para identificar los pares clave-valor asociados con una compilación de software concreta.

Puede usar cualquier carácter unicode en las etiquetas, excepto `*`, `,` y `\` que están reservados. Si tiene que incluir un carácter reservado, deberá especificar un carácter de escape como `\{Reserved Character}`.

### <a name="querying-key-values"></a>Realización de consultas de pares clave-valor

Cada par clave-valor se identifica de forma exclusiva mediante su clave más una etiqueta que puede ser `null`. Puede consultar un almacén de App Configuration para pares clave-valor mediante la especificación de un patrón. Este almacén devolverá todos los pares clave-valor que coincidan con el patrón y sus correspondientes valores y atributos. Puede usar los siguientes patrones de claves en las llamadas de API REST a App Configuration:

| Clave | |
|---|---|
| se omite `key` o `key=*` | Coincide con todas las claves |
| `key=abc` | Coincide con el nombre de clave **abc** exactamente. |
| `key=abc*` | Coincide con los nombres de clave que empiezan por **abc** |
| `key=*abc` | Coincide con los nombres de clave que terminan en **abc** |
| `key=*abc*` | Coincide con los nombres de clave que contienen **abc** |
| `key=abc,xyz` | Coincide con los nombres de clave **abc** o **xyz**, se limita a 5 archivos .CSV |

También puede incluir los siguientes patrones de etiquetas:

| Etiqueta | |
|---|---|
| se omite `label` o `label=*` | Coincide con cualquier etiqueta, incluida `null` |
| `label=%00` | Coincide con la etiqueta `null`. |
| `label=1.0.0` | Coincide exactamente con la etiqueta **1.0.0** |
| `label=1.0.*` | Coincide con las etiquetas que empiezan por **1.0.** |
| `label=*.0.0` | Coincide con las etiquetas que terminan en **.0.0** |
| `label=*.0.*` | Coincide con las etiquetas que contienen **.0.** |
| `label=%00,1.0.0` | Coincide con las etiquetas `null` o **1.0.1**, se limita a 5 archivos .CSV |

## <a name="values"></a>Valores

Los valores asignados a las claves también son cadenas unicode. Puede usar todos los caracteres unicode para los valores. Hay un **tipo de contenido** opcional definido por el usuario asociado a cada valor. Puede usar este atributo para almacenar información (por ejemplo, un esquema de codificación) sobre un valor que ayuda a la aplicación a procesarlo correctamente.

Los datos de configuración almacenados en un almacén de App Configuration, incluidos todos los pares clave-valor, están cifrados en reposo y en tránsito. No obstante, App Configuration no constituye una solución de reemplazo de Azure Key Vault. No debería almacenar secretos de aplicación en ella.

## <a name="next-steps"></a>Pasos siguientes

* [Concepto: Instantánea en un momento dado](concept-point-time-snapshot.md)  
