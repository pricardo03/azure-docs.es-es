---
title: Implementación de dependencias en la nube de OPC Twin en Azure | Microsoft Docs
description: Implementación de dependencias de OPC Twin en Azure.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: ae9f2b05bfc6ea6315022d04c8d267d916cf282e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61451606"
---
# <a name="deploying-dependencies-for-local-development"></a>Implementación de dependencias para el desarrollo local

En este artículo se explica la necesidad de operaciones de desarrollo y depuración locales para implementar solo los servicios de la plataforma Azure.   Al final, tendrá un grupo de recursos implementado con todo lo necesario para el desarrollo y la depuración locales.

## <a name="deploy-azure-platform-services"></a>Implementación de servicios de la plataforma Azure

1. Asegúrese de que tiene las extensiones de PowerShell y [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-1.1.0) instaladas.  Abra un símbolo del sistema o un terminal y ejecute:

   ```bash
   git clone https://github.com/Azure/azure-iiot-components
   cd azure-iiot-components
   ```

   ```bash
   deploy -type local
   ```

2. Siga las indicaciones para asignar un nombre al grupo de recursos para la implementación.  El script implementa solo las dependencias para este grupo de recursos en su suscripción de Azure, pero no los microservicios.  El script también registra una aplicación en Azure Active Directory.  Esto es necesario para admitir la autenticación basada en OAUTH.  La implementación puede tardar varios minutos.

3. Una vez completado el script, puede optar por guardar el archivo .env.  El archivo de entorno .env es el archivo de configuración de todos los servicios y herramientas que quiere ejecutar en el equipo de desarrollo.  

## <a name="troubleshooting-deployment-failures"></a>Errores de solución de problemas de implementación

### <a name="resource-group-name"></a>Definición de un nombre de grupo de recursos

Asegúrese de usar un nombre de grupo de recursos corto y sencillo.  El nombre se usa también para denominar recursos como si tuviera que cumplir con los requisitos de nomenclatura de recursos.  

### <a name="azure-active-directory-aad-registration"></a>Registro de Azure Active Directory (AAD)

El script de implementación intenta registrar aplicaciones de AAD en Azure Active Directory.  En función de sus derechos para el inquilino de AAD seleccionado, esto podría generar un error.   Existen tres opciones:

1. Si ha elegido un inquilino de AAD de una lista de los inquilinos, reinicie el script y elija otro de la lista.
2. Como alternativa, implemente un inquilino de AAD privado, reinicie el script y seleccione la opción para usarlo.
3. Continúe con la autenticación.  Puesto que los microservicios se ejecutan localmente, esta operación es aceptable, pero no imita los entornos de producción.  

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha implementado correctamente los servicios de OPC Twin en un proyecto existente, este es el siguiente paso que se le sugiere:

> [!div class="nextstepaction"]
> [Obtener información sobre cómo implementar módulos de OPC Twin](howto-opc-twin-deploy-modules.md)
