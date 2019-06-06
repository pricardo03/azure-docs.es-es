---
title: Planeación de la implementación de Unión a Azure Active Directory híbrido en Azure Active Directory (Azure AD) | Microsoft Docs
description: Aprenda a configurar dispositivos híbridos unidos a Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2019
ms.author: joflore
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64dd8067654246f7c9a077d027c068df820f439d
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66688704"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Instrucciones: Planeamiento de la implementación de la unión a Azure Active Directory híbrido

De forma similar a un usuario, un dispositivo es otra identidad principal que desea proteger y usarla para proteger los recursos en cualquier momento y desde cualquier ubicación. Puede lograr este objetivo mediante Traer y administración de identidades de dispositivo de Azure AD mediante uno de los métodos siguientes:

- Unión a Azure AD
- Unión a Azure AD híbrido
- Registro de Azure AD

Al traer sus dispositivos a Azure AD, está maximizando la productividad de los usuarios mediante un inicio de sesión único (SSO) en los recursos de nube y del entorno local. Al mismo tiempo, puede proteger el acceso a los recursos de nube y de entorno local con [acceso condicional](../active-directory-conditional-access-azure-portal.md).

Si tiene un entorno de Active Directory (AD) local y quiere unir los equipos unidos a un dominio de AD a Azure AD, puede hacerlo mediante la combinación de Azure AD híbrido. En este artículo se proporcionan los pasos relacionados con la implementación de una unión a Azure AD híbrido en su entorno. 

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por supuesto que está familiarizado con el [Introducción a la administración de identidades de dispositivo en Azure Active Directory](../device-management-introduction.md).

> [!NOTE]
> El mínimo requerido funcional de dominio y los niveles funcionales del bosque de Windows 10 Azure AD híbrido es Windows Server 2008 R2.

## <a name="plan-your-implementation"></a>Planeamiento de la implementación

Para planear la implementación de Azure AD híbrido, debe familiarizarse con:

|   |   |
| --- | --- |
| ![Comprobar][1] | Revisión de los dispositivos compatibles |
| ![Comprobar][1] | Revisión de los aspectos que debe conocer |
| ![Comprobar][1] | Revise la validación controlada de Azure AD híbrido |
| ![Comprobar][1] | Seleccione su escenario según la infraestructura de identidad |
| ![Comprobar][1] | Revisión de forma local UPN para AD compatibilidad híbrida unión a Azure AD |

## <a name="review-supported-devices"></a>Revisión de los dispositivos compatibles

La unión a Azure AD híbrido admite una amplia variedad de dispositivos Windows. Dado que la configuración para los dispositivos que ejecutan versiones anteriores de Windows requiere pasos adicionales o diferentes, los dispositivos compatibles se agrupan en dos categorías:

### <a name="windows-current-devices"></a>Dispositivos de Windows actuales

- Windows 10
- Windows Server 2016
- Windows Server 2019

Para dispositivos que ejecutan el sistema operativo de escritorio Windows, versión admitido se enumeran en este artículo [información de versión de Windows 10](https://docs.microsoft.com/windows/release-information/). Como práctica recomendada, Microsoft recomienda que actualice a la versión más reciente de Windows 10.

### <a name="windows-down-level-devices"></a>Dispositivos de Windows de nivel inferior

- Windows 8.1
- Windows 7. Para obtener información de soporte técnico en Windows 7, consulte este artículo [está finalizando el soporte técnico para Windows 7](https://www.microsoft.com/en-us/windowsforbusiness/end-of-windows-7-support)
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2

Como primer paso del planeamiento, debe revisar el entorno y determinar si necesita admitir dispositivos Windows de nivel inferior.

## <a name="review-things-you-should-know"></a>Revisión de los aspectos que debe conocer

Combinación de Azure AD híbrido no se admite si el entorno consta de un único bosque de AD sincronizar los datos de identidad con más de un inquilino de Azure AD.

Combinación de Azure AD híbrido no se admite cuando se usa la infraestructura de escritorio virtual (VDI).

No se admite Azure AD híbrido para TPM compatible con FIPS. Si los dispositivos tienen TPM compatible con FIPS, debe deshabilitar antes de continuar con Azure AD híbrido. Microsoft no proporciona ninguna herramienta para deshabilitar el modo FIPS para TPM como depende del fabricante del TPM. Póngase en contacto con el hardware de OEM para obtener soporte técnico.

No se admite la combinación de Azure AD híbrido para Windows Server que ejecuta el rol de controlador de dominio (DC).

Azure AD híbrido no se admite en dispositivos de nivel inferior de Windows al usar las credenciales móviles o perfil de usuario móvil.

Si confía en la herramienta de preparación del sistema (Sysprep) y si está utilizando un **versiones anteriores de Windows 10 1809** imagen para la instalación, asegúrese de que esa imagen no corresponde a un dispositivo que ya está registrado con Azure AD como unión a Azure AD híbrido.

Si confía en una instantánea de máquina Virtual (VM) para crear máquinas virtuales adicionales, asegúrese de que esa instantánea no es de una máquina virtual que ya está registrada con Azure AD como unión a Azure AD híbrido.

Si los dispositivos de Windows 10 unidos a un dominio ya están [registrados en Azure AD](https://docs.microsoft.com/azure/active-directory/devices/overview#azure-ad-registered-devices) para el inquilino, se recomienda encarecidamente quitar ese estado antes de habilitar Unión a Azure AD híbrido. A partir de la versión Windows 10 1809, se han realizado los siguientes cambios para evitar este estado dual:

- Cualquier estado registrado en Azure AD existente se eliminará automáticamente cuando el dispositivo se una a Azure AD híbrido.
- Puede impedir que el dispositivo unido al dominio que se va a Azure AD registrada mediante la adición de esta clave del registro: HKLM\SOFTWARE\Policies\Microsoft\Windows\WorkplaceJoin, "BlockAADWorkplaceJoin" = dword: 00000001.
- Este cambio ahora está disponible para la versión de Windows 10 1803 con KB4489894 aplicado. Sin embargo, si tiene Windows Hello para empresas configurado, el usuario es necesario re-setup Windows Hello para empresas tras el estado dual de limpieza.


## <a name="review-controlled-validation-of-hybrid-azure-ad-join"></a>Revise la validación controlada de Azure AD híbrido

Cuando todos los requisitos previos están en su lugar, se registrarán automáticamente los dispositivos de Windows como dispositivos en su inquilino de Azure AD. El estado de estas identidades de dispositivo en Azure AD se conoce como combinación de Azure AD híbrido. Puede encontrar más información sobre los conceptos tratados en este artículo en los artículos [Introducción a la administración de identidades de dispositivo en Azure Active Directory](overview.md) y [planear su unión a Azure Active Directory de híbrido implementación](hybrid-azuread-join-plan.md).

Las organizaciones que desee realizar una validación controlada de combinación de Azure AD híbrido antes de habilitarlo en toda su organización a la vez. Consulte el artículo [controla la validación de Azure AD híbrido](hybrid-azuread-join-control.md) para comprender cómo llevarla a cabo.


## <a name="select-your-scenario-based-on-your-identity-infrastructure"></a>Seleccione su escenario según la infraestructura de identidad

Azure AD híbrido funciona con los entornos, federados y administrados.  

### <a name="managed-environment"></a>Entorno administrado

Puede ser un entorno administrado implementado mediante [sincronización de Hash de contraseñas (PHS)](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-phs) o [pasar a través de autenticación (PTA)](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta) con [perfecta Single Sign On](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso).

Estos escenarios no requieren que se configure un servidor de federación para la autenticación.

### <a name="federated-environment"></a>Entorno federado

Un entorno federado debe tener un proveedor de identidades que admite los siguientes requisitos:

- **Protocolo de WS-Trust:** Este protocolo se requiere para la autenticación de Windows Azure AD de actual híbrido unido dispositivos con Azure AD.
- **Notificación WIAORMULTIAUTHN:** Esta notificación se requiere para realizar la combinación de Azure AD híbrido para dispositivos de nivel inferior de Windows.

Si tiene un entorno federado mediante los servicios de federación de Active Directory (AD FS), ya se admiten los requisitos anteriores.

> [!NOTE]
> Azure AD no admite tarjetas inteligentes ni certificados en dominios administrados.

Desde la versión 1.1.819.0, Azure AD Connect proporciona un asistente para configurar la unión a Azure AD híbrido. El asistente permite simplificar considerablemente el proceso de configuración. Si no le es posible instalar la versión requerida de Azure AD Connect, consulte la información sobre [cómo configurar manualmente el registro de dispositivos](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-manual). 

Según el escenario que coincida con la infraestructura de identidad, vea:

- [Configurar la combinación de Azure Active Directory híbrido para un entorno federado](hybrid-azuread-join-federated-domains.md)
- [Configurar la combinación de Azure Active Directory híbrido para el entorno administrado](hybrid-azuread-join-managed-domains.md)



## <a name="review-on-premises-ad-upn-support-for-hybrid-azure-ad-join"></a>Revisión de compatibilidad on-premises AD UPN para Azure AD híbrido

A veces, los UPN de AD local podrían ser diferentes de los UPN de Azure AD. En tales casos, Unión a Azure AD híbrido para Windows 10 proporciona compatibilidad limitada para los UPN de AD local, según el tipo de [método de autenticación](https://docs.microsoft.com/azure/security/azure-ad-choose-authn), el tipo de dominio y la versión de Windows 10. Hay dos tipos de UPN de AD local que pueden existir en su entorno:

- UPN enrutable: Un UPN enrutable tiene un dominio verificado válido, que está registrado en un registrador de dominios. Por ejemplo, si contoso.com es el dominio principal, en Azure AD, contoso.org es el dominio principal en la instancia local de AD que pertenece a Contoso y que está [verificado en Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain)
- UPN no enrutable: Un UPN no enrutable no tiene un dominio verificado. Es aplicable solo dentro de la red privada de su organización. Por ejemplo, si contoso.com es el dominio principal en Azure AD, contoso.local es el dominio principal en la instancia local de AD, pero no es un dominio verificable en Internet, y solo se usa dentro de la red de Contoso.

En la tabla siguiente se proporcionan detalles sobre la compatibilidad de estos UPN de AD local en Unión a Azure AD híbrido para Windows 10.

| Tipo de UPN de AD local | Tipo de dominio | Versión de Windows 10 | DESCRIPCIÓN |
| ----- | ----- | ----- | ----- |
| Enrutable | Federado | A partir de la versión 1703 | Disponibilidad general |
| No enrutable | Federado | A partir de la versión 1803 | Disponibilidad general |
| Enrutable | Administrado | No compatible | |
| No enrutable | Administrado | No compatible | |

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Unión a Azure Active Directory híbrido de configurar un entorno federado](hybrid-azuread-join-federated-domains.md)
> [unión a Azure Active Directory de configurar híbrido para el entorno administrado](hybrid-azuread-join-managed-domains.md)

<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png
