---
title: Versión preliminar de Protección con contraseña de Azure AD
description: Prohibición de contraseñas no seguras en una instancia de Active Directory local con la versión preliminar de Protección con contraseña de Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: ff349c480ca14b4242fb7597751b4eb6acb0ee78
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2018
ms.locfileid: "42145927"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Versión preliminar: Aplicación de Protección con contraseña de Azure AD para Windows Server Active Directory

|     |
| --- |
| Protección con contraseña y Lista personalizada de contraseñas prohibidas de Azure AD son versiones preliminares públicas de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Protección con contraseña de Azure AD es la versión preliminar pública de una nueva característica diseñada por Azure Active Directory (Azure AD) para mejorar las directivas de contraseñas de una organización. La implementación local de Protección con contraseña de Azure AD usa listas globales y personalizadas de contraseñas prohibidas almacenadas en Azure AD, y realiza las mismas comprobaciones locales que los cambios basados en la nube de Azure AD.

Hay tres componentes de software que forman Protección con contraseña de Azure AD:

* El servicio de proxy de Protección con contraseña de Azure AD se ejecuta en cualquier máquina unida a un dominio del bosque de Active Directory actual. Este servicio reenvía las solicitudes de los controladores de dominio a Azure AD y devuelve la respuesta de Azure AD al controlador de dominio.
* El servicio de agente de controlador de dominio de Protección con contraseña de Azure AD recibe solicitudes de validación de contraseñas de la DLL de filtro de contraseñas de este agente, las procesa mediante la directiva de contraseñas actual disponible localmente y devuelve el resultado (sin errores\con errores). Este servicio es responsable de llamar periódicamente (cada hora) al servicio de proxy de Protección con contraseña de Azure AD para recuperar las nuevas versiones de la directiva de contraseñas. Las comunicaciones hacia el servicio de proxy de Protección con contraseña de Azure AD y desde este servicio se controlan mediante RPC (Llamada a procedimiento remoto) a través de TCP. Tras la recuperación, las nuevas directivas se almacenan en una carpeta sysvol desde la que se pueden replicar en otros controladores de dominio. El servicio del agente de controlador de dominio también supervisa la carpeta sysvol para detectar cambios en caso de que otros controladores de dominio hayan escrito nuevas directivas de contraseñas allí. Si ya hay una directiva adecuadamente reciente disponible, se omitirá la comprobación del servicio de proxy de Protección con contraseña de Azure AD.
* La DLL de filtro de contraseña del agente de controlador de dominio recibe solicitudes de validación de contraseñas del sistema operativo y las reenvía al servicio de agente de controlador de dominio de Protección con contraseña de Azure AD que se ejecuta localmente en el controlador de dominio.

![Funcionamiento conjunto de los componentes de Protección con contraseña de Azure AD](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

## <a name="requirements"></a>Requisitos

* Todas las máquinas en las que estén instalados los componentes de Protección con contraseña de Azure AD, incluidos los controladores de dominio, deben ejecutar Windows Server 2012 o posterior.
* Todas las máquinas en las que haya componentes de protección con contraseña de Azure AD instalados, incluidos los controladores de dominio, deben tener instalado Universal C Runtime. Si es posible, esto se logra mediante la aplicación de revisiones completas a la máquina a través de Windows Update. De lo contrario, es posible que se instale un paquete de actualizaciones específico del sistema operativo: vea [Actualización para Universal C RunTime en Windows ](https://support.microsoft.com/help/2999226/update-for-universal-c-runtime-in-windows).
* Debe existir conectividad de red entre al menos un controlador de dominio de cada dominio y un servidor que hospede el servicio de proxy de Protección con contraseña de Azure AD.
* Todos los dominios de Active Directory que ejecuten el software del servicio de agente de controlador de dominio deben usar DFSR para la replicación de sysvol.
* Una cuenta de administrador global para registrar el servicio de proxy de Protección con contraseña de Azure AD.
* Una cuenta con privilegios de administrador de dominio de Active Directory en el dominio raíz del bosque.

### <a name="license-requirements"></a>Requisitos de licencia

Las ventajas de la lista global de contraseñas prohibidas se aplican a todos los usuarios de Azure Active Directory (Azure AD).

La lista personalizada de contraseñas prohibidas requiere licencias básicas de Azure AD.

Protección con contraseña de Azure AD para Windows Server Active Directory requiere licencias premium de Azure AD.

Puede encontrar información adicional sobre licencias, incluidos los costos, en la [página de precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>Descargar

Se necesitan dos instaladores para Protección con contraseña de Azure AD que se pueden descargar del [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=57071)

## <a name="answers-to-common-questions"></a>Respuestas a preguntas comunes

* No se necesita conectividad de Internet de los controladores de dominio. Las máquinas que ejecutan el servicio de proxy de Protección con contraseña de Azure AD son las únicas máquinas que requieren conectividad de Internet.
* No se abren puertos de red en los controladores de dominio.
* No se requiere ningún cambio de esquema de Active Directory.
* El software utiliza el contenedor de Active Directory existente y los objetos de esquema de serviceConnectionPoint.
* No hay ningún requisito mínimo de dominio de Active Directory ni de nivel funcional del bosque (DFL/FFL).
* El software no crea ni requiere ninguna cuenta en los dominios de Active Directory que protege.
* Se admite la implementación incremental a cambio de que la directiva de contraseñas se aplique solamente allí donde se haya instalado el agente de controlador de dominio.
* Protección con contraseña de Azure AD no es un motor de aplicación de directivas en tiempo real. Puede que se produzca un retraso entre un cambio en la configuración de la directiva de contraseñas y el momento en el que este alcanza y se aplica a todos los controladores de dominio.

## <a name="next-steps"></a>Pasos siguientes

[Implementación de la protección de contraseñas de Azure AD](howto-password-ban-bad-on-premises.md)