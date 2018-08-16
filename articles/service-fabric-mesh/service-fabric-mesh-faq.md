---
title: Preguntas comunes sobre Azure Service Fabric Mesh | Microsoft Docs
description: Obtenga información sobre las preguntas más frecuentes y respuestas sobre Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: chackdan
ms.author: chackdan
ms.date: 06/25/2018
ms.topic: troubleshooting
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: d0ae7fbb22f6d98662f83968158182d447a75394
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2018
ms.locfileid: "39501974"
---
# <a name="commonly-asked-service-fabric-mesh-questions"></a>Preguntas frecuentes sobre Service Fabric Mesh
Azure Service Fabric Mesh es un servicio totalmente administrado que permite a los desarrolladores implementar aplicaciones de microservicios sin necesidad de administrar máquinas virtuales, almacenamiento o redes. Este artículo contiene respuestas a las preguntas más habituales.

## <a name="how-do-i-report-an-issue-or-ask-a-question"></a>¿Cómo notifico un problema o formulo una pregunta?

Formule preguntas, obtenga respuestas de los ingenieros de Microsoft e informe de problemas en el [repositorio de GitHub de service-fabric-mesh-preview](https://aka.ms/sfmeshissues).

## <a name="quota-and-cost"></a>Cuota y costo

**¿Cuál es el costo de participar en la versión preliminar?**

No hay ningún cargo para implementar aplicaciones o contenedores en la versión preliminar de Mesh. En cambio, le recomendamos que elimine los recursos que implemente y que no los deje ejecutándose, a menos que los esté probando activamente.

**¿Hay un límite de cuota del número de núcleos y RAM?**

Sí, las cuotas para cada suscripción son:

- Número de aplicaciones: 5 
- Número de núcleos por aplicación: 12 
- Total de memoria RAM por cada aplicación: 48 GB 
- Número de puntos de conexión de red y de entrada: 5  
- Número de volúmenes de Azure que puede adjuntar: 10 
- Número de réplicas de servicio: 3 
- El contenedor más grande que puede implementar está limitado a 4 núcleos, 16 GB de RAM.
- Puede asignar núcleos parciales a los contenedores en incrementos de 0,5 núcleos hasta un máximo de 6 núcleos.

**¿Puedo dejar mi aplicación ejecutándose durante la noche?**

Sí, puede, pero le recomendamos que elimine los recursos que implemente y que no los deje ejecutándose, a menos que los esté probando activamente. Esta directiva puede cambiar en el futuro y se pueden eliminar los recursos si se están usando incorrectamente.

## <a name="supported-container-os-images"></a>Imágenes del sistema operativo de contenedor compatibles
Las siguientes imágenes del sistema operativo de contenedor pueden usarse al implementar servicios.

- Windows: windowsservercore y nanoserver
    - Windows Server 2016
    - Windows Server versión 1709
- Linux
    - Ninguna limitación conocida

## <a name="features-gaps-and-known-issues"></a>Lagunas de características y problemas conocidos

**Después de implementar mi aplicación, el recurso de red asociado con ella no parece tener una dirección IP**

Hay un problema conocido hoy en día por el cual la dirección IP aparece después de un retraso. Compruebe el estado del recurso de red en unos minutos para ver la dirección IP asociada.

**Mi aplicación no puede tener acceso al recurso de red o por volumen correcto**

En el modelo de aplicación deberá usar el identificador de recurso completo para que las redes y volúmenes puedan tener acceso al recurso asociado. Este es el aspecto en el ejemplo de inicio rápido:

```json
"networkRefs": [
    {
    "name":  "[resourceId('Microsoft.ServiceFabric/networks', 'SbzVotingNetwork')]" 
    }
]
```

**No veo el modelo de aplicación actual que admite una manera de cifrar los secretos**

Sí, el cifrado de secretos no se admite en la versión preliminar privada actual. 

**El DNS no funciona del mismo modo en el clúster de desarrollo de Service Fabric y en Mesh**

Hay un problema conocido por el cual es posible que deba hacer referencia a servicios de manera diferente en el clúster de desarrollo local y en Azure Mesh. En el clúster de desarrollo local, use {serviceName}.{applicationName}. En Azure Service Fabric Mesh, use {servicename}. Azure Mesh no admite actualmente la resolución de DNS en todas las aplicaciones.

Puede consultar otros problemas conocidos de DNS con la ejecución de un clúster de desarrollo de Service Fabric en Windows 10 aquí: [Depuración de contenedores de Windows](/azure/service-fabric/service-fabric-how-to-debug-windows-containers).

**Obtengo este error cuando uso el módulo de la CLI ImportError: no se puede importar el nombre 'sdk_no_wait'**

Si usa una versión de la CLI anterior a 2.0.30, es posible que obtenga este error:

```
cannot import name 'sdk_no_wait'
Traceback (most recent call last):
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\knack\cli.py", line 193, in invoke cmd_result = self.invocation.execute(args)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core\commands_init_.py", line 241, in execute self.commands_loader.load_arguments(command)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 201, in load_arguments self.command_table[command].load_arguments() # this loads the arguments via reflection
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core\commands_init_.py", line 142, in load_arguments super(AzCliCommand, self).load_arguments()
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\knack\commands.py", line 76, in load_arguments cmd_args = self.arguments_loader()
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 332, in default_arguments_loader op = handler or self.get_op_handler(operation)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 375, in get_op_handler op = import_module(mod_to_import)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\importlib_init_.py", line 126, in import_module return _bootstrap._gcd_import(name[level:], package, level)
File "", line 978, in _gcd_import
File "", line 961, in _find_and_load
File "", line 950, in _find_and_load_unlocked
File "", line 655, in _load_unlocked
File "", line 678, in exec_module
File "", line 205, in _call_with_frames_removed
File "C:\Users\annayak.azure\cliextensions\azure-cli-sbz\azext_sbz\custom.py", line 18, in 
from azure.cli.core.util import get_file_json, shell_safe_json_parse, sdk_no_wait
ImportError: cannot import name 'sdk_no_wait'.
```

**Obtengo un error de coincidencia de nombre de la distribución al instalar el paquete de extensión de la CLI**

Esto no significa que no se haya instalado la extensión. Todavía debería poder usar los comandos de la CLI sin problemas.

**Al escalar horizontalmente, veo que están afectados todos los contenedores, incluidos los que se están ejecutando.**

Se trata de un error y se corregirá en la siguiente actualización del entorno de ejecución.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre Service Fabric Mesh, lea la [introducción](service-fabric-mesh-overview.md).
