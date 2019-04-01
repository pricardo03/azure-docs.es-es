---
title: Instalación de Windows de Azure Security Center para vista previa del agente de IoT | Microsoft Docs
description: Obtenga información sobre cómo instalar Azure Security Center para el agente de IoT en los dispositivos de Windows de 32 bits o 64 bits.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2019
ms.author: mlottner
ms.openlocfilehash: 7e3cb1f44711a8eedb248320db8bce5dfd0eaf0c
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58754605"
---
# <a name="deploy-an-azure-security-center-for-iot-c-based-security-agent-for-windows"></a>Implementar un centro de seguridad de Azure para IoT C#-basándose security agent para Windows

> [!IMPORTANT]
> Azure Security Center para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Esta guía explica cómo instalar Azure Security Center (ASC) para IoT C#-en función de agente de seguridad en Windows.

En esta guía, aprenderá a: 
> [!div class="checklist"]
> * Instalación
> * Comprobación de la implementación
> * Desinstalar el agente
> * Solución de problemas 

## <a name="prerequisites"></a>Requisitos previos

Para otras plataformas y tipos de agente, consulte [elegir el agente de seguridad adecuadas](how-to-deploy-agent.md).

1. Derechos de administrador local en el equipo que desea instalar en. 

1. [Crear un módulo de seguridad](quickstart-create-security-twin.md) para el dispositivo.

## <a name="installation"></a>Instalación 

Para instalar al agente de seguridad, haga lo siguiente:

1. Para instalar el ASC para Windows IoT C# agente en el dispositivo, descargue la versión más reciente para la máquina desde la ASC para IoT [repositorio de GitHub](https://github.com/Azure/Azure-IoT-Security-Agent-CS).

2. Extraiga el contenido del paquete y navegue hasta la carpeta/Install.

3. Abra Windows PowerShell como administrador. 
    1. Agregue permisos de ejecución al script InstallSecurityAgent mediante la ejecución ```Unblock-File .\InstallSecurityAgent.ps1```
    
        y ejecute:

    ```
    .\InstallSecurityAgent.ps1 -Install -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -cl <certificate location kind>
    ```
    
    Por ejemplo: 
    
    ```
    .\InstallSecurityAgent.ps1 -Install -aui Device -aum SymmetricKey -f c:\Temp\Key.txt -hn MyIotHub.azure-devices.net -di Mydevice1 -cl store
    ```
    
    Consulte [cómo configurar la autenticación](concept-security-agent-authentication-methods.md) para obtener más información acerca de los parámetros de autenticación.

El script hace lo siguiente:

- Instala los requisitos previos.

- Agrega un usuario de servicio (con deshabilitado el inicio de sesión interactivo).

- Instala el agente como un **servicio del sistema**.

- Configura al agente con los parámetros de autenticación proporcionado.


Para obtener ayuda adicional, use el comando Get-Help en PowerShell <br>Ejemplo de Get-Help:  
    ```Get-Help .\InstallSecurityAgent.ps1```

### <a name="verify-deployment-status"></a>Compruebe el estado de implementación

- Compruebe el estado de implementación del agente mediante la ejecución:<br>
    ```sc.exe query "ASC IoT Agent" ```

### <a name="uninstall-the-agent"></a>Desinstalar el agente

Para desinstalar al agente:

1. Ejecute el siguiente script de PowerShell con la **-modo** parámetro establecido en **desinstalar**.  

    ```
    .\InstallSecurityAgent.ps1 -Uninstall
    ``` 

## <a name="troubleshooting"></a>solución de problemas

Si no se puede iniciar el agente, activar el registro (el registro es *desactivar* de forma predeterminada) para obtener más información.

Para activar el registro:

1. Abra el archivo de configuración (General.config) para la edición con un editor de archivos estándar.

1. Edite los valores siguientes:

   ```xml
   <add key="logLevel" value="Debug" />
   <add key="fileLogLevel" value="Debug"/> 
   <add key="diagnosticVerbosityLevel" value="Some" /> 
   <add key="logFilePath" value="IoTAgentLog.log" />
   ```

    > [!NOTE]
    > Se recomienda la activación del registro **desactivar** una vez finalizada la solución de problemas. Salir de registro **en** aumenta de uso de datos y del tamaño del archivo de registro. 

1. Reinicie al agente mediante la ejecución de PowerShell o la línea de comandos siguiente:

    **PowerShell**
     ```
     Restart-Service "ASC IoT Agent"
     ```
     
   o

    **CMD**
     ```
     sc.exe stop "ASC IoT Agent" 
     sc.exe start "ASC IoT Agent" 
     ```

1. Revise el archivo de registro para obtener más información sobre el error.

   Ubicación del archivo de registro: `%WinDir%/System32/IoTAgentLog.log`


## <a name="next-steps"></a>Pasos siguientes
- Leer los procesos de ASC para el servicio IoT [información general](overview.md)
- Más información sobre los procesos de ASC para IoT [arquitectura](architecture.md)
- Habilitar la [servicio](quickstart-onboard-iot-hub.md)
- Leer el [preguntas más frecuentes](resources-frequently-asked-questions.md)
- Comprender [alertas](concept-security-alerts.md)