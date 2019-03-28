---
title: ¿Qué es Enterprise State Roaming en Azure Active Directory? | Microsoft Docs
description: Proporciona información sobre la configuración de Enterprise State Roaming en dispositivos de Windows. Enterprise State Roaming proporciona a los usuarios una experiencia unificada a través de sus dispositivos de Windows y reduce el tiempo necesario para configurar un nuevo dispositivo.
services: active-directory
keywords: qué es Enterprise State Roaming, sincronización empresarial, nube de windows
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: curtand
ms.subservice: devices
ms.assetid: 83b3b58f-94c1-4ab0-be05-20e01f5ae3f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: joflore
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3a2a81bd8aa3fc99d033564e8a8782c79261305
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521608"
---
# <a name="what-is-enterprise-state-roaming"></a>¿Qué es Enterprise State Roaming?

Con Windows 10, los usuarios de [Azure Active Directory (Azure AD)](../fundamentals/active-directory-whatis.md) obtienen la capacidad de sincronizar de forma segura su configuración de usuario y los datos de configuración de la aplicación en la nube. Enterprise State Roaming proporciona a los usuarios una experiencia unificada a través de sus dispositivos de Windows y reduce el tiempo necesario para configurar un nuevo dispositivo. Enterprise State Roaming funciona de forma similar a la [sincronización de configuración de consumidor](https://go.microsoft.com/fwlink/?linkid=2015135) estándar que se presentó en Windows 8. Además, Enterprise State Roaming ofrece:

* **Separación de datos del consumidor y de empresa** : las organizaciones controlan sus datos y no hay ninguna combinación de datos corporativos en una cuenta de nube de consumidor o datos de consumidores en una cuenta de nube de la empresa.
* **Seguridad mejorada** : los datos se cifran automáticamente antes de salir del dispositivo Windows 10 del usuario mediante Azure Rights Management (Azure RMS) y los datos permanecen cifrados almacenados en la nube. Todo el contenido permanece cifrado en la nube, excepto los espacios de nombres, como nombres de configuración y nombres de aplicación de Windows.  
* **Mejor administración y supervisión** : proporciona control y visibilidad sobre quién sincroniza la configuración en su organización y en qué dispositivos a través de la integración en el portal de Azure AD. 

Enterprise State Roaming está disponible en varias regiones de Azure. En la página [Regiones de Azure](https://azure.microsoft.com/regions/#services) de Azure Active Directory encontrará la lista actualizada de las regiones disponibles.

| Artículo | DESCRIPCIÓN |
| --- | --- |
| [Habilitación de Enterprise State Roaming en Azure Active Directory](enterprise-state-roaming-enable.md) |Enterprise State Roaming está disponible para cualquier organización con una suscripción Premium de Azure Active Directory (Azure AD). Para obtener más información acerca de cómo obtener una suscripción de Azure AD, consulte la página de [productos de Azure AD](https://azure.microsoft.com/services/active-directory) . |
| [Preguntas más frecuentes sobre itinerancia de datos y configuración](enterprise-state-roaming-faqs.md) |Este tema responde a algunas preguntas que los administradores de TI podrían tener sobre la sincronización de datos de aplicación y la configuración. |
| [Configuración de MDM y directivas de grupo](enterprise-state-roaming-group-policy-settings.md) |Windows 10 proporciona configuración de la directiva de directiva de grupo y de administración de dispositivos móviles (MDM) para limitar la sincronización de configuración. |
| [Referencia de la configuración de movilidad de Windows 10](enterprise-state-roaming-windows-settings-reference.md) |Lo siguiente es una lista completa de todas las opciones que se movilizan o se copian en Windows 10. |
| [Solución de problemas](enterprise-state-roaming-troubleshooting.md) |En este tema se llevan a cabo algunos pasos básicos de solución de problemas y en él se incluye una lista de problemas conocidos. |

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo habilitar Enterprise State Roaming, consulte cómo [habilitar Enterprise State Roaming](enterprise-state-roaming-enable.md).
