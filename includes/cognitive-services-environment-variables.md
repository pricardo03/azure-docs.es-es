---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 6d6451d50a00569eb1da8f5b0a0dc10d3c6b1115
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841479"
---
## <a name="configure-an-environment-variable-for-authentication"></a>Configuración de una variable de entorno para la autenticación

Las aplicaciones tienen que autenticar el acceso a los servicios Cognitive Services que usan. Para realizar la autenticación, se recomienda crear una variable de entorno para almacenar las claves de los recursos de Azure. 

Una vez que tenga la clave, escríbala en una variable de entorno nueva en la máquina local que ejecuta la aplicación. Para establecer la variable de entorno, abra una ventana de consola y siga las instrucciones de su sistema operativo. Reemplace `your-key` por una de las claves del recurso.

* Windows

    ```console
    setx COGNITIVE_SERVICE_KEY "your-key"
    ```

    Después de agregar la variable de entorno, puede que tenga que reiniciar todos los programas en ejecución que necesiten leer la variable de entorno, incluida la ventana de consola. Por ejemplo, si usa Visual Studio como editor, reinícielo antes de ejecutar el ejemplo.

* Linux
    
    ```bash
    export COGNITIVE_SERVICE_KEY=your-key
    ```
    
    Después de agregar la variable de entorno, ejecute `source ~/.bashrc` desde la ventana de consola para que los cambios surtan efecto.
    
* macOS
    
    Edite .bash_profile y agregue la variable de entorno:
    
    ```bash
    export COGNITIVE_SERVICE_KEY=your-key
    ```
    
    Después de agregar la variable de entorno, ejecute `source .bash_profile` desde la ventana de consola para que los cambios surtan efecto.
