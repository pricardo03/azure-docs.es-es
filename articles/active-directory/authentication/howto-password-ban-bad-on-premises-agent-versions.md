---
title: Historial de versiones del agente de protección con contraseña de Azure AD local
description: Historial de cambios de comportamiento y versiones de los documentos
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 11/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: ccfe62e0002e3420303130840f1a0d393efb3420
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078770"
---
# <a name="preview--azure-ad-password-protection-agent-version-history"></a>Vista previa:  Historial de versiones del agente de protección con contraseña de Azure AD

|     |
| --- |
| La protección con contraseña de Azure AD es una característica en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="12250"></a>1.2.25.0

Fecha de lanzamiento: 01/11/2018

Correcciones:

* El agente de controlador de dominio y el servicio de proxy ya no deberían funcionar mal debido a errores de confianza de certificados.
* El agente de controlador de dominio y el servicio de proxy presentan correcciones adicionales para las máquinas compatibles con FIPS.
* El servicio de proxy funcionará ahora correctamente en un entorno de red solo TLS 1.2.
* Correcciones menores de rendimiento y estabilidad
* Registro mejorado

Cambios:

* El nivel de sistema operativo mínimo necesario para el servicio de proxy es ahora Windows Server 2012 R2. El nivel de sistema operativo mínimo necesario para el servicio de agente de controlador de dominio sigue siendo Windows Server 2012.
* El algoritmo de validación de contraseña usa una tabla de normalización de caracteres ampliada. Como consecuencia, contraseñas que se aceptaron en versiones anteriores podrían rechazarse.

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
* Correcciones menores de rendimiento y estabilidad
* Registro mejorado

## <a name="11103"></a>1.1.10.3

Fecha de lanzamiento: 15/6/2018

Versión preliminar pública inicial

## <a name="next-steps"></a>Pasos siguientes

[Implementación de la protección de contraseñas de Azure AD](howto-password-ban-bad-on-premises-deploy.md)
