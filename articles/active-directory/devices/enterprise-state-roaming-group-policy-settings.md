---
title: Configuración de MDM y directivas de grupo | Microsoft Docs
description: Proporciona información sobre la configuración de directiva de grupo y la administración de dispositivos móviles (MDM) que debe usarse en dispositivos de empresa. Estas directivas se aplican al dispositivo completo del usuario.
services: active-directory
keywords: ¿cuál es la configuración de directiva de grupo y MDM para Enterprise State Roaming, Enterprise State Roaming, nube de windows
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: curtand
ms.subservice: devices
ms.assetid: 6471a9b3-8dd4-4237-89d1-bfbeca9f8252
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: joflore
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f1b23862edeabbf8e03d4589fa3f22a3fab9f72
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60353166"
---
# <a name="group-policy-and-mdm-settings"></a>Configuración de MDM y directivas de grupo
Use esta configuración de directiva de grupo y de dispositivos móviles (MDM) de solo en dispositivos de empresa, dado que estas directivas se aplican en todo el dispositivo del usuario. Aplicar una directiva MDM para deshabilitar la sincronización de configuración para un dispositivo de usuario personal ejercerá un impacto negativo en el uso de ese dispositivo. Además, otras cuentas de usuario en el dispositivo también se verán afectadas por la directiva.

Las empresas que desean administrar la movilidad para sus dispositivos personales (no administrados) pueden usar el Portal de Azure para habilitar o deshabilitar la movilidad, en lugar de usar la directiva de grupo o MDM.
Las tablas siguientes describen la configuración de la directiva disponible.

## <a name="mdm-settings"></a>Configuración de MDM
La configuración de directiva MDM se aplica a Windows 10 y Windows 10 Mobile.  El soporte para Windows 10 Mobile existe solo para la itinerancia basada en cuentas de Microsoft a través de la cuenta OneDrive del usuario.  Consulte la sección [Dispositivos y puntos de conexión](enterprise-state-roaming-windows-settings-reference.md) para información detallada sobre qué dispositivos se admiten para la sincronización basada en Azure AD.

| NOMBRE | DESCRIPCIÓN |
| --- | --- |
| Permitir la conexión con una cuenta de Microsoft |Permite a los usuarios autenticarse con una cuenta de Microsoft en el dispositivo. |
| Permitir la sincronización de mi configuración |Permite a los usuarios usar un perfil itinerante para la configuración de Windows y los datos de la aplicación; si deshabilita esta directiva se deshabilitará la sincronización, así como las copias de seguridad en dispositivos móviles |

## <a name="group-policy-settings"></a>Configuración de directiva de grupo
La configuración de directiva de grupo se aplica a dispositivos de Windows 10 que están unidos a un dominio de Active Directory. La tabla incluye también la configuración heredada que aparecerá para administrar la configuración de sincronización, pero que no funciona para Enterprise State Roaming para Windows 10. Esto se indica con "No utilizar" en la descripción.

Esta configuración se encuentran en: `Computer Configuration > Administrative Templates > Windows Components > Sync your settings` 

| NOMBRE | DESCRIPCIÓN |
| --- | --- |
| Cuentas: Bloquear cuentas Microsoft |Esta configuración de directiva impide a los usuarios agregar nuevas cuentas de Microsoft en este equipo. |
| No sincronizar |Impide a los usuarios usar un perfil itinerante para los datos de la aplicación y la configuración de Windows. |
| No sincronizar personalización |Deshabilita la sincronización del grupo Temas. |
| No sincronizar la configuración del explorador |Deshabilita la sincronización del grupo Internet Explorer. |
| No sincronizar contraseñas |Deshabilita la sincronización del grupo Contraseñas. |
| No sincronizar otra configuración de Windows |Deshabilita la sincronización del grupo Otra configuración de Windows. |
| No sincronizar la personalización del escritorio |No se usa; no tiene efecto. |
| No sincronizar con conexiones de uso medido |Deshabilita la movilidad en conexiones limitadas, como 3G móvil. |
| No sincronizar aplicaciones |No se usa; no tiene efecto. |
| No sincronizar la configuración de aplicaciones |Deshabilita la movilidad de datos de la aplicación. |
| No sincronizar la configuración de inicio |No se usa; no tiene efecto. |

## <a name="next-steps"></a>Pasos siguientes

Para obtener información general, consulte [Introducción a Enterprise State Roaming](enterprise-state-roaming-overview.md).


