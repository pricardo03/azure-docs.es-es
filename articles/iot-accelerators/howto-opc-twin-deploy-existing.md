---
title: Cómo implementar el módulo de administración de dispositivos de Azure IoT OPC UA en un proyecto existente | Microsoft Docs
description: Cómo implementar a OPC gemelo a un proyecto existente.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 9b1dee8ea43d01825449cb6010fbfa62e4715cff
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2019
ms.locfileid: "59047269"
---
# <a name="deploy-opc-twin-to-an-existing-project"></a>Implementar a OPC gemelo a un proyecto existente

El módulo gemelo del dispositivo de OPC se ejecuta en IoT Edge y proporciona varios servicios de borde para el dispositivo gemelo de OPC y servicios de registro. 

El servicio de micro OPC dispositivo gemelo facilita la comunicación entre los operadores de fábrica y los dispositivos de servidor de OPC UA en la fábrica a través de un módulo gemelo IoT Edge de OPC. El servicio micro expone servicios de OPC UA (Examinar, lectura, escritura y ejecución) a través de su API de REST. 

El servicio de registro del dispositivo OPC UA micro proporciona acceso a las aplicaciones registradas de OPC UA y sus puntos de conexión. Operadores y administradores pueden registrar y anular el registro de nuevas aplicaciones de OPC UA y examinar los existentes, incluidos sus puntos de conexión. Además de la aplicación y administración de extremos, el servicio de registro también cataloga módulos registrados de dispositivo gemelo IoT Edge de OPC. Le ofrece la API del servicio de control de la funcionalidad de módulo para edge, por ejemplo, iniciar o detener la detección de servidores (servicios de análisis) o activar nuevos gemelos de punto de conexión que se pueden acceder mediante el servicio micro gemelo OPC.

El núcleo del módulo es la identidad de Supervisor. El supervisor administra a gemelo de punto de conexión, que corresponde a los puntos de conexión de servidor de OPC UA que se activan mediante el registro de OPC UA API correspondiente. Este gemelos extremo traducen OPC UA JSON recibido del servicio micro gemelo OPC que se ejecutan en la nube en los mensajes binarios de OPC UA, que se envían a través de un canal seguro con estado al punto de conexión administrado. El supervisor también proporciona servicios de detección que envían eventos de detección de dispositivo para el servicio de incorporación de dispositivo de OPC UA para su procesamiento, donde estos eventos son el resultado de las actualizaciones en el registro de OPC UA.  Este artículo muestra cómo implementar el módulo gemelo de OPC en un proyecto existente. 

> [!NOTE]
> Para obtener más información sobre los detalles de implementación e instrucciones, vea GitHub [repositorio](https://github.com/Azure/azure-iiot-opc-twin-module).

## <a name="prerequisites"></a>Requisitos previos

Asegúrese de que tiene PowerShell y [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps) extensiones instaladas.   Si no lo ha hecho todavía, clone este repositorio de GitHub.  Abra un símbolo del sistema o terminal y ejecute:

```bash
git clone --recursive https://github.com/Azure/azure-iiot-components 
cd azure-iiot-components
```

## <a name="deploy-industrial-iot-services-to-azure"></a>Implementar Servicios industriales de IoT en Azure

1. En el comando Abrir símbolo del sistema o terminal, ejecute:

   ```bash
   deploy
   ```

2. Siga las indicaciones para asignar un nombre al grupo de recursos de la implementación y un nombre para el sitio Web.   El script implementa los microservicios y sus dependencias de plataforma de Azure en el grupo de recursos en su suscripción de Azure.  El script también registra una aplicación en el inquilino de Azure Active Directory (AAD) para admitir la autenticación basada en OAUTH.  Implementación tardará varios minutos.  Un ejemplo de lo que vería una vez implementada correctamente la solución:

   ![Implementar el gemelo de OPC IoT industrial al proyecto existente](media/howto-opc-twin-deploy-existing/opc-twin-deploy-existing1.png)

   La salida incluye la dirección URL del punto de conexión público. 

3. Una vez que el script se completa correctamente, seleccione si desea guardar el archivo env.  Necesita el archivo de entorno .env si desea conectarse al punto de conexión en la nube con herramientas como la consola o implementar módulos para el desarrollo y depuración.

## <a name="troubleshooting-deployment-failures"></a>Solución de problemas de implementación

### <a name="resource-group-name"></a>Definición de un nombre de grupo de recursos

Asegúrese de que usar un nombre de grupo de recursos a corto y sencillo.  El nombre se usa también para recursos de nombre de tal modo que debe cumplir con los requisitos de nomenclatura de recursos.  

### <a name="website-name-already-in-use"></a>Nombre del sitio Web ya está en uso

Es posible que el nombre del sitio Web ya está en uso.  Si experimenta este error, deberá usar un nombre de aplicación diferente.

### <a name="azure-active-directory-aad-registration"></a>Registro de Azure Active Directory (AAD)

El script de implementación intenta registrar dos aplicaciones de AAD en Azure Active Directory.  Dependiendo de sus derechos para el inquilino de AAD seleccionado, puede producir un error en la implementación. Hay dos opciones:

1. Si ha elegido a un inquilino de AAD de una lista de los inquilinos, reinicie la secuencia de comandos y elija otro nombre de la lista.
2. Como alternativa, implementar a un inquilino AAD privado en otra suscripción, reinicie la secuencia de comandos y seleccione esta opción para usarlo.

> [!WARNING]
> NUNCA continúan sin autenticación.  Si decide hacerlo, cualquier usuario puede acceder a los puntos de conexión de administración de dispositivos de OPC desde Internet no autenticado.   Siempre puede elegir el [opción de implementación "local"](howto-opc-twin-deploy-dependencies.md) a comprar.

## <a name="deploy-an-all-in-one-industrial-iot-services-demo"></a>Implementar una demostración de servicios IoT industrial de all-in-one

En lugar de simplemente los servicios y las dependencias también puede implementar una demostración en uno.  Todos en una demostración contiene tres servidores OPC UA, el módulo gemelo OPC, todos los microservicios y un ejemplo de aplicación Web.  Se está diseñado para fines de demostración.

1. Asegúrese de que tiene un clon del repositorio (véase más arriba). Abra un símbolo del sistema o terminal en la raíz del repositorio y ejecute:

   ```bash
   deploy -type demo
   ```

2. Siga las indicaciones para asignar un nombre nuevo para el grupo de recursos y un nombre para el sitio Web.  Una vez que haya implementado correctamente, el script mostrará la dirección URL del extremo de aplicación web.

## <a name="deployment-script-options"></a>Opciones de script de implementación

El script toma los parámetros siguientes:

```bash
-type
```

El tipo de implementación (demostración de máquina virtual, local)

```bash
-resourceGroupName
```

Puede ser el nombre de un grupo de recursos nuevo o existente.

```bash
-subscriptionId
```

Opcional, el identificador de suscripción que se implementarán los recursos.

```bash
-subscriptionName
```

O el nombre de la suscripción.

```bash
-resourceGroupLocation
```

Opcional, una ubicación del grupo de recursos. Si se especifica, intentará crear un nuevo grupo de recursos en esta ubicación.

```bash
-aadApplicationName
```

Un nombre para la aplicación de AAD registrar con. 

```bash
-tenantId
```

Inquilino de AAD para usarlo.

```bash
-credentials
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido cómo implementar a OPC gemelo a un proyecto existente, aquí es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Protección de la comunicación entre OPC Client y OPC PLC ](howto-opc-vault-deploy-existing-client-plc-communication.md)
