---
title: Guía para instalar e implementar Linux C# agente de Azure Security Center para la versión preliminar de IoT | Microsoft Docs
description: Obtenga información sobre cómo instalar el agente de IoT de Azure Security Center en Linux de 32 bits y 64 bits.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: b0982203-c3c8-4a0b-8717-5b5ac4038d8c
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: be4c663d3a1e99ef67cbbbc2f39b315f1080125c
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758330"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Implementar Azure Security Center para IoT C#-basándose security agent para Linux

> [!IMPORTANT]
> Azure Security Center para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Esta guía explica cómo instalar e implementar Azure Security Center (ASC) para IoT C#-agente de seguridad de basados en Linux.

En esta guía, aprenderá a: 
> [!div class="checklist"]
> * Instalación
> * Comprobación de la implementación
> * Desinstalar el agente
> * Solución de problemas 

## <a name="prerequisites"></a>Requisitos previos

Para otras plataformas y versiones de agente, consulte [elegir el agente de seguridad adecuadas](how-to-deploy-agent.md).

1. Para implementar al agente de seguridad, se necesitan derechos de administrador local en el equipo que desea instalar en. 

1. [Crear un módulo de seguridad](quickstart-create-security-twin.md) para el dispositivo.

## <a name="installation"></a>Instalación 

Para implementar al agente de seguridad, haga lo siguiente:

1. Descargue la versión más reciente en el equipo de [Github](https://aka.ms/iot-security-github-cs).

1. Extraiga el contenido del paquete y navegue hasta la _/instalar_ carpeta.

1. Agregue permisos de ejecución a la **InstallSecurityAgent script** mediante la ejecución `chmod +x InstallSecurityAgent.sh` 

1. A continuación, ejecute: 

   ```
   ./InstallSecurityAgent.sh -i -aui <authentication identity>  -aum <authentication method> -f <file path> -hn <host name>  -di <device id> -cl <certificate location kind>
   ```
   
   Consulte [cómo configurar la autenticación](concept-security-agent-authentication-methods.md) para obtener más información acerca de los parámetros de autenticación.

El script hace lo siguiente:

- Instala los requisitos previos.

- Agrega un usuario de servicio (con deshabilitado el inicio de sesión interactivo).

- Instala el agente como un **Daemon** -esto supone que el dispositivo usa **systemd** para administración de servicios.

- Configura **sudoers** para permitir que el agente realizar ciertas tareas como raíz.

- Configura al agente con los parámetros de autenticación proporcionado.


Para obtener ayuda adicional, ejecute el script con el parámetro – help: `./InstallSecurityAgent.sh --help`

### <a name="uninstall-the-agent"></a>Desinstalar el agente

Para desinstalar el agente, ejecute el script con el parámetro – u: `./InstallSecurityAgent.sh -u`. 

> [!NOTE]
> Desinstalación no quita todos los requisitos previos que faltan se instalaron durante la instalación.

## <a name="troubleshooting"></a>solución de problemas  

1. Compruebe el estado de implementación mediante la ejecución:

    `systemctl status ASCIoTAgent.service`

2. Habilitar el registro.  
   Si no se puede iniciar el agente, activar el registro para obtener más información.

   Activar el registro por:

   1. Abra el archivo de configuración para su edición en cualquier editor de Linux:

        `vi /var/ASCIoTAgent/General.config`

   1. Edite los valores siguientes: 

      ```
      <add key="logLevel" value="Debug"/>
      <add key="fileLogLevel" value="Debug"/>
      <add key="diagnosticVerbosityLevel" value="Some" /> 
      <add key="logFilePath" value="IotAgentLog.log"/>
      ```
       El **logFilePath** valor es configurable. 

       > [!NOTE]
       > Se recomienda la activación del registro **desactivar** una vez finalizada la solución de problemas. Salir de registro **en** aumenta de uso de datos y del tamaño del archivo de registro.

   1. Reinicie al agente mediante la ejecución:

       `systemctl restart ASCIoTAgent.service`

   1. Ver el archivo de registro para obtener más información sobre el error.  

       Ubicación del archivo de registro es: `/var/ASCIoTAgent/IotAgentLog.log`

       Cambiar la ruta de acceso de ubicación de archivo según el nombre que eligió para el **logFilePath** en el paso 2. 

## <a name="next-steps"></a>Pasos siguientes

- Leer los procesos de ASC para el servicio IoT [información general](overview.md)
- Más información sobre los procesos de ASC para IoT [arquitectura](architecture.md)
- Habilitar la [servicio](quickstart-onboard-iot-hub.md)
- Leer el [preguntas más frecuentes](resources-frequently-asked-questions.md)
- Comprender [alertas](concept-security-alerts.md)