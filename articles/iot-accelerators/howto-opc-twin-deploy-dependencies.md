---
title: Cómo implementar las dependencias en la nube de administración de dispositivos de OPC UA de Azure IoT | Microsoft Docs
description: Cómo implementar las dependencias de OPC gemelo Azure.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: cdd89293328da47c8e338bb6fca8c7b93dd84f97
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759658"
---
# <a name="deploying-dependencies-for-local-development"></a>Implementar dependencias para el desarrollo local

En este artículo se explica cómo implementar solo la necesidad de servicios de plataforma de Azure para realizar la depuración y desarrollo local.   Al final, tendrá implementado un grupo de recursos que contiene todo lo que necesita para la depuración y desarrollo local.

## <a name="deploy-azure-platform-services"></a>Implementar servicios de plataforma de Azure

1. Asegúrese de que tiene PowerShell y [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-1.1.0) extensiones instaladas.  Abra un símbolo del sistema o terminal y ejecute:

   ```bash
   git clone https://github.com/Azure/azure-iiot-components
   cd azure-iiot-components
   ```

   ```bash
   deploy -type local
   ```

2. Siga las indicaciones para asignar un nombre al grupo de recursos para la implementación.  El script implementa solo las dependencias para este grupo de recursos en su suscripción de Azure, pero no los microservicios.  El script también registra una aplicación en Azure Active Directory.  Esto es necesario para admitir la autenticación basada en OAUTH.  Implementación puede tardar varios minutos.

3. Una vez completado el script, puede seleccionar para guardar el archivo env.  El archivo .env del entorno es el archivo de configuración de todos los servicios y herramientas que desea ejecutar en el equipo de desarrollo.  

## <a name="troubleshooting-deployment-failures"></a>Solución de problemas de implementación

### <a name="resource-group-name"></a>Definición de un nombre de grupo de recursos

Asegúrese de que usar un nombre de grupo de recursos a corto y sencillo.  El nombre se usa también para recursos de nombre de tal modo que debe cumplir con los requisitos de nomenclatura de recursos.  

### <a name="azure-active-directory-aad-registration"></a>Registro de Azure Active Directory (AAD)

El script de implementación intenta registrar aplicaciones de AAD en Azure Active Directory.  Dependiendo de sus derechos para el inquilino de AAD seleccionado, esto podría dar error.   Hay 3 opciones:

1. Si ha elegido a un inquilino de AAD de una lista de los inquilinos, reinicie la secuencia de comandos y elija otro nombre de la lista.
2. Como alternativa, implementar a un inquilino AAD privado, reinicie la secuencia de comandos y seleccione esta opción para usarlo.
3. Continuar sin autenticación.  Puesto que los microservicios se ejecuta localmente, esto es aceptable, pero no imitar los entornos de producción.  

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha implementado correctamente los servicios de administración de dispositivos de OPC para un proyecto existente, aquí es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Obtenga información sobre cómo implementar módulos de administración de dispositivos de OPC](howto-opc-twin-deploy-modules.md)
