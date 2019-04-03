---
title: Guía para instalar e implementar Linux C# agente de Azure Security Center para la versión preliminar de IoT | Microsoft Docs
description: Obtenga información sobre cómo instalar el agente de IoT de Azure Security Center en Linux de 32 bits y 64 bits.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: b0982203-c3c8-4a0b-8717-5b5ac4038d8c
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: 4a53bcf77696d3aa53a4a404bfacd6f6d468885b
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862109"
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
> * Desinstalación del agente
> * Solución de problemas 

## <a name="prerequisites"></a>Requisitos previos

Para otras plataformas y versiones de agente, consulte [elegir el agente de seguridad adecuadas](how-to-deploy-agent.md).

1. Para implementar el agente de seguridad, se necesitan derechos de administrador local en el equipo en el que se desee instalar. 

1. [Crear un módulo de seguridad](quickstart-create-security-twin.md) para el dispositivo.

## <a name="installation"></a>Instalación 

Para implementar el agente de seguridad, realice las siguientes operaciones:

1. Descargue en el equipo la versión más reciente desde [GitHub](https://aka.ms/iot-security-github-cs).

1. Extraiga el contenido del paquete y vaya a la carpeta _/Install_.

1. Agregue permisos de ejecución a la **InstallSecurityAgent script** mediante la ejecución `chmod +x InstallSecurityAgent.sh` 

1. A continuación, ejecute: 

   ```
   ./InstallSecurityAgent.sh -i -aui <authentication identity>  -aum <authentication method> -f <file path> -hn <host name>  -di <device id> -cl <certificate location kind>
   ```
   
   Para más información acerca de los parámetros de autenticación, consulte [Procedimiento para configurar la autenticación](concept-security-agent-authentication-methods.md).

El script hace lo siguiente:

- Instala los requisitos previos.

- Agrega un usuario de servicio (con el inicio de sesión interactivo deshabilitado).

- Instala el agente como **daemon** (se da por hecho que el dispositivo usa **systemd** para la administración del servicio).

- Configura **sudoers** para permitir que el agente realice ciertas tareas como raíz.

- Configura al agente con los parámetros de autenticación proporcionados.


Para obtener ayuda adicional, ejecute el script con el parámetro – help: `./InstallSecurityAgent.sh --help`

### <a name="uninstall-the-agent"></a>Desinstalación del agente

Para desinstalar al agente, ejecute el script con el parámetro – u: `./InstallSecurityAgent.sh -u`. 

> [!NOTE]
> La desinstalación no quita los requisitos previos que falten que se hayan instalado durante la instalación.

## <a name="troubleshooting"></a>solución de problemas  

1. Compruebe el estado de implementación, para lo que debe ejecutar:

    `systemctl status ASCIoTAgent.service`

2. Habilite el registro.  
   Si el agente no se inicia el agente, active el registro para más información.

   Para activar el registro:

   1. Abra el archivo de configuración para poder editarlo en cualquier editor de Linux:

        `vi /var/ASCIoTAgent/General.config`

   1. Edite los valores siguientes: 

      ```
      <add key="logLevel" value="Debug"/>
      <add key="fileLogLevel" value="Debug"/>
      <add key="diagnosticVerbosityLevel" value="Some" /> 
      <add key="logFilePath" value="IotAgentLog.log"/>
      ```
       El valor **logFilePath** se puede configurar. 

       > [!NOTE]
       > Se recomienda **desactivar** el registro una vez que se complete la solución de problemas, ya que si se deja **activado** aumenta el tamaño del archivo de registro y el uso de datos.

   1. Para reiniciar el agente, ejecute:

       `systemctl restart ASCIoTAgent.service`

   1. Vea el archivo de registro para más información acerca del error.  

       Ubicación del archivo de registro es: `/var/ASCIoTAgent/IotAgentLog.log`

       Cambie la ruta de acceso de la ubicación del archivo en función del nombre que eligió para el **logFilePath** en el paso 2. 

## <a name="next-steps"></a>Pasos siguientes

- Lea la [información general](overview.md) del servicio ASC for IoT
- Obtenga más información acerca de la [arquitectura](architecture.md) de ASC for IoT
- Habilite el [servicio](quickstart-onboard-iot-hub.md)
- Lea las [preguntas más frecuentes](resources-frequently-asked-questions.md)
- Obtenga información acerca de las [alertas](concept-security-alerts.md)