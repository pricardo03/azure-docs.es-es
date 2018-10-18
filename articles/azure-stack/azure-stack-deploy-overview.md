---
title: Evaluación del Kit de desarrollo de Azure Stack | Microsoft Docs
description: Aprenda a implementar el Kit de desarrollo de Azure Stack con fines de evaluación.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/15/2018
ms.author: jeffgilb
ms.custom: mvc
ms.openlocfilehash: bf07fe56c65e53f5485b9927e0d704f80842cf3a
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49338718"
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Inicio rápido: Evaluación del Kit de desarrollo de Azure Stack

El [Kit de desarrollo de Azure Stack](.\asdk\asdk-what-is.md) es un entorno de desarrollo y pruebas que se puede implementar para evaluar y probar las características y servicios de Azure Stack. Para empezar a utilizar el Kit de desarrollo de Azure Stack, debe preparar el hardware del equipo host y, después, ejecutar algunos scripts (la instalación tarda varias horas). Una vez finalizado, puede iniciar sesión en los portales del administrador o del usuario para empezar a usar Azure Stack.

## <a name="prerequisites"></a>Requisitos previos

### <a name="asdk-host-computer-requirements"></a>Requisitos del equipo host del Kit de desarrollo de Azure Stack

Antes de instalar el Kit de desarrollo de Azure Stack, es preciso preparar el equipo que lo va a hospedar. El equipo host del kit de desarrollo debe cumplir los requisitos de hardware, de software y de red que se describen en **[Consideraciones de planeación de la implementación de Azure Stack](.\asdk\asdk-deploy-considerations.md)**.

> [!TIP]
> Puede usar la [herramienta de comprobación de requisitos de implementación de Azure Stack](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) después de instalar el sistema operativo en el equipo host del kit de desarrollo para confirmar que el hardware cumple todos los requisitos.

### <a name="account-requirements"></a>Requisitos de cuenta

También debe elegir entre utilizar Azure Active Directory (Azure AD) o Active Directory Federation Services (AD FS) como la solución de identidad para su implementación. Revise los requisitos de cuenta en **[Consideraciones de planeación de la implementación](.\asdk\asdk-deploy-considerations.md#account-requirements)**

## <a name="download-and-extract-the-deployment-package"></a>Descargar y extraer el paquete de implementación

Después de preparar el equipo host del kit de desarrollo, descargue y extraiga el paquete de implementación del Kit de desarrollo de Azure Stack. Dicho paquete incluye el archivo Cloudbuilder.vhdx, que es una unidad de disco duro virtual (VHD) con un sistema operativo de arranque, y los archivos de instalación de Azure Stack.

Puede descargar el paquete de implementación en el host de kit de desarrollo o en otro equipo. Los archivos de implementación extraídos ocupan 60 GB de espacio del disco, por lo que el uso de otro equipo puede ayudar a reducir los requisitos de almacenamiento del host del kit de desarrollo.

**[Descarga y extracción de Kit de desarrollo de Azure Stack](.\asdk\asdk-download.md)**

## <a name="prepare-the-host-computer"></a>Preparación del equipo host

Para poder instalar el Kit de desarrollo de Azure Stack es preciso preparar el entorno del host y configurar el sistema para arrancar desde el disco duro virtual del kit de desarrollo. Cuando se reinicia el host, arranca desde CloudBuilder.vhdx y puede empezar a implementar el Kit de desarrollo de Azure Stack.

**[Preparación del equipo host del Kit de desarrollo de Azure Stack](.\asdk\asdk-prepare-host.md)**

## <a name="install-the-asdk-on-the-host-computer"></a>Instalar el Kit de desarrollo de Azure Stack en el equipo host

Después de que el equipo host arranque desde el disco duro virtual, puede implementar el kit de desarrollo en el entorno virtual de Cloudbuilder. El Kit de desarrollo de Azure Stack se puede implementar mediante la interfaz gráfica de usuario (GUI), que se proporciona al ejecutar el script de PowerShell asdk-installer.ps1, o desde la [línea de comandos de PowerShell](.\asdk\asdk-deploy-powershell.md)

> [!NOTE]
> Cuando el host haya arrancado desde la imagen de Cloudbuilder.vhdx, tiene la opción de configurar los [valores de telemetría de Azure Stack](.\asdk\asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *antes* de instalar el Kit de desarrollo de Azure Stack.

**[Instalación del Kit de desarrollo de Azure Stack](.\asdk\asdk-install.md)**

## <a name="perform-post-deployment-configurations"></a>Realización de configuraciones después de la implementación

Después de instalar el Kit de desarrollo de Azure Stack, se recomiendan algunas comprobaciones posteriores a la instalación y cambios de configuración que deben realizarse.

**Herramientas**

Instale las herramientas PowerShell y GitHub de Azure Stack y use el cmdlet test-AzureStack para comprobar que la instalación se ha realizado correctamente.

**Solución de identidad**

En el caso de una implementación que use Azure AD, debe activar los portales de administrador y de inquilino de Azure Stack. Esta activación concede los permisos correctos al portal de Azure Stack y al de Azure Resource Manager (permisos que se muestran en la página de consentimiento) para todos los usuarios del directorio.

**Expiración de la contraseña**

Debe restablecer la directiva de expiración de contraseña para asegurarse de que la contraseña del host de kit de desarrollo no expire antes de que termine el período de evaluación.

> [!NOTE]
> También tiene la opción de configurar los [valores de telemetría de Azure Stack](.\asdk\asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *después de* instalar el Kit de desarrollo de Azure Stack.

**[Tareas de implementación posteriores al Kit de desarrollo de Azure Stack](.\asdk\asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Registrarse en Azure

Debe registrar Azure Stack en Azure para que pueda [descargar elementos de Azure Marketplace](.\asdk\asdk-marketplace-item.md) a Azure Stack.

**[Registro de Azure Stack en Azure](.\asdk\asdk-register.md)**

## <a name="next-steps"></a>Pasos siguientes

Felicidades. Si completa los pasos de esta guía de inicio rápido, tendrá un entorno de ASDK con un portal de [administrador](https://adminportal.local.azurestack.external) y un portal de [usuario](https://portal.local.azurestack.external).
