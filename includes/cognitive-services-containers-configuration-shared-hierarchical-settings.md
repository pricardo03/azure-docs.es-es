---
author: WenJason
ms.author: digimobile
ms.service: cognitive-services
ms.topic: include
origin.date: 01/02/2019
ms.date: 01/28/2019
ms.openlocfilehash: 03ec8740a4cf36bf3d09dade8a24b155c09d1299
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60327429"
---
La configuración del contenedor es jerárquica y todos los contenedores del equipo host usan una jerarquía compartida.

Puede usar cualquiera de las siguientes opciones para especificar la configuración:

* [Variables de entorno](#environment-variable-settings)
* [Argumentos de la línea de comandos](#command-line-argument-settings)

Los valores de variable de entorno invalidan los valores de argumento de la línea de comandos, que a su vez invalidan los valores predeterminados para la imagen de contenedor. Si especifica valores diferentes en una variable de entorno y un argumento de la línea de comandos para la misma opción de configuración, el contenedor del que se crea la instancia usa el valor de la variable de entorno.

|Prioridad|Ubicación de la configuración|
|--|--|
|1|Variable de entorno| 
|2|Línea de comandos|
|3|Valor predeterminado de la imagen de contenedor|

### <a name="environment-variable-settings"></a>Configuración de variables de entorno

Las ventajas del uso de variables de entorno son:

* Se pueden configurar varias opciones.
* Varios contenedores pueden usar la misma configuración.

### <a name="command-line-argument-settings"></a>Configuración de argumentos de la línea de comandos

La ventaja de utilizar argumentos de la línea de comandos es que cada contenedor puede usar una configuración diferente.
