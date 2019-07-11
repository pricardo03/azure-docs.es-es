---
title: Guía para instalar e implementar el agente basado en C# de Linux de Azure Security Center for IoT, versión preliminar | Microsoft Docs
description: Aprenda a instalar el agente de Azure Security Center for IoT en máquinas Linux de 32 bits y 64 bits.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: b0982203-c3c8-4a0b-8717-5b5ac4038d8c
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: 808ff912a997a4c09a22048ada7546daab895701
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618264"
---
# <a name="deploy-azure-security-center-for-iot-c-based-security-agent-for-linux"></a>Implementar el agente de seguridad basado en C# de Azure Security Center for IoT para Linux

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En esta guía se explica cómo instalar e implementar el agente de seguridad basado en C# de Azure Security Center (ASC) for IoT en Linux.

En esta guía, aprenderá a: 
> [!div class="checklist"]
> * Instalación
> * Comprobación de la implementación
> * Desinstalación del agente
> * Solución de problemas 

## <a name="prerequisites"></a>Requisitos previos

Para obtener información sobre otras plataformas y tipos de agente, vea [Elegir el agente de seguridad correcto](how-to-deploy-agent.md).

1. Para implementar el agente de seguridad, se necesitan derechos de administrador local en el equipo en el que se desee instalar. 

1. [Crear un módulo de seguridad](quickstart-create-security-twin.md) para el dispositivo.

## <a name="installation"></a>Instalación 

Para implementar el agente de seguridad, realice las siguientes operaciones:

1. Descargue en el equipo la versión más reciente desde [GitHub](https://aka.ms/iot-security-github-cs).

1. Extraiga el contenido del paquete y vaya a la carpeta _/Install_.

1. Agregue permisos de ejecución al **script de InstallSecurityAgent**, para lo que debe ejecutar `chmod +x InstallSecurityAgent.sh` 

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

       La ubicación del archivo de registro es: `/var/ASCIoTAgent/IotAgentLog.log`

       Cambie la ruta de acceso de la ubicación del archivo en función del nombre que eligió para el **logFilePath** en el paso 2. 

## <a name="next-steps"></a>Pasos siguientes

- Lea la [información general](overview.md) del servicio ASC for IoT
- Obtenga más información acerca de la [arquitectura](architecture.md) de ASC for IoT
- Habilite el [servicio](quickstart-onboard-iot-hub.md)
- Lea las [preguntas más frecuentes](resources-frequently-asked-questions.md)
- Obtenga información acerca de las [alertas](concept-security-alerts.md)