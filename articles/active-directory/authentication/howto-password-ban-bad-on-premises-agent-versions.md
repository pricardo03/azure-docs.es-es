---
title: Historial de versiones del agente de protección con contraseña de Azure AD local
description: Historial de cambios de comportamiento y versiones de los documentos
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 10/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: c52c84a1311c30c19356bb8a1287b203faf476fc
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "50742910"
---
# <a name="preview--azure-ad-password-protection-agent-version-history"></a>Versión preliminar: Historial de versiones del agente de protección con contraseña de Azure AD

|     |
| --- |
| La protección con contraseña de Azure AD es una característica en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="12100"></a>1.2.10.0

Fecha de lanzamiento: 17/8/2018

Correcciones:

* Register-AzureADPasswordProtectionProxy y Register-AzureADPasswordProtectionForest ya admiten la autenticación multifactor.
* Register-AzureADPasswordProtectionProxy requiere un controlador de dominio WS2012 o posterior en el dominio para evitar los errores de cifrado.
* El servicio del agente del controlador de dominio es más confiable en cuanto a la solicitud de una directiva de contraseñas nueva de Azure en el inicio.
* El servicio del agente del controlador de dominio solicitará una directiva de contraseñas nueva de Azure cada hora si es necesario, pero ahora lo hará en una hora de inicio seleccionada aleatoriamente.
* El servicio del agente del controlador de dominio ya no producirá un retraso indefinido en el nuevo anuncio del controlador de dominio cuando se instale en un servidor antes de su promoción como una réplica.
* El servicio del agente del controlador de dominio ahora admite la opción de configuración "Habilitar protección con contraseña en Windows Server Active Directory".
* Ahora, los instaladores de proxy y agente del controlador de dominio son compatibles con la actualización local al realizar la actualización a versiones futuras.

> [!WARNING]
> La actualización local de la versión 1.1.10.3 no se admite y producirá un error de instalación. Para actualizar a la versión 1.2.10 o posterior, primero deberá desinstalar por completo el software del servicio de proxy y el agente de controlador de dominio y, luego, deberá instalar la nueva versión desde cero. Es necesario eliminar el registro del servicio de proxy de protección con contraseña de Azure AD.  No es necesario volver a registrar el bosque.

> [!NOTE]
> Las actualizaciones locales del software del agente del controlador de dominio requerirán un reinicio.

* El servicio de proxy y de agente del controlador de dominio ahora se pueden ejecutar en un servidor configurado para usar solo algoritmos conformes a FIPS.
* Registro mejorado
* Correcciones menores de rendimiento y estabilidad

## <a name="11103"></a>1.1.10.3

Fecha de lanzamiento: 15/6/2018

Versión preliminar pública inicial

## <a name="next-steps"></a>Pasos siguientes

[Implementación de la protección de contraseñas de Azure AD](howto-password-ban-bad-on-premises-deploy.md)
