---
title: Funcionamiento del inicio de sesión único en recursos locales de dispositivos unidos a Azure AD | Microsoft Docs
description: Aprenda a configurar dispositivos híbridos unidos a Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 49fab25d772dba431bb2eb1ccac8a5e6daf41667
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "52276969"
---
# <a name="how-sso-to-on-premises-resources-works-on-azure-ad-joined-devices"></a>Funcionamiento del inicio de sesión único en recursos locales de dispositivos unidos a Azure AD

Probablemente no sea una sorpresa le sorprenda que un dispositivo unido a Azure Active Directory (Azure AD) le ofrezca una experiencia de inicio de sesión único (SSO) para aplicaciones de nube de su inquilino. Si su entorno tiene un entorno local de Active Directory (AD), puede ampliar la experiencia de inicio de sesión único en estos dispositivos con ello.

En este artículo se explica cómo funciona.

## <a name="how-it-works"></a>Cómo funciona 

Como debe recordar un solo nombre de usuario y contraseña, el inicio de sesión único simplifica el acceso a los recursos y mejora la seguridad del entorno. Con un dispositivo unido a Azure AD, los usuarios ya tienen una experiencia de inicio de sesión único para las aplicaciones de nube en el entorno. Si el entorno tiene una instancia de Azure AD y una instancia de AD local, probablemente desee ampliar el alcance de la experiencia de inicio de sesión único a sus aplicaciones de línea de negocio (LOB) locales, recursos compartidos de archivos e impresoras.  


Los dispositivos unidos a Azure AD no tienen ningún conocimiento acerca del entorno de AD porque no están unidos a él. Sin embargo, puede proporcionar información adicional acerca de su instancia de AD local a estos dispositivos con Azure AD Connect.
Un entorno con ambos, Azure AD y AD local, también se conoce como entorno híbrido. Si tiene un entorno híbrido, es probable que tenga ya implementado Azure AD Connect para sincronizar la información de identidad local con la de la nube. Como parte del proceso de sincronización, Azure AD Connect sincroniza la información de dominio local con Azure AD. Cuando un usuario inicia sesión en un dispositivo de Azure AD en un entorno híbrido:

1. Azure AD envía el nombre del dominio local del usuario como miembro de vuelta al dispositivo. 

2. El servicio de la autoridad de seguridad local (LSA) permite la autenticación de Kerberos en el dispositivo.

Durante un intento de acceso a un recurso en el dominio del usuario local, el dispositivo:

1. Usa la información de dominio para localizar un controlador de dominio (DC). 

2. Envía las credenciales de usuario y la información de dominio local al controlador de dominio encontrado para autenticar al usuario.

3. Recibe un [vale de concesión de vales (TGT)](https://docs.microsoft.com/windows/desktop/secauthn/ticket-granting-tickets) de Kerberos que se utiliza para acceder a los recursos unidos a AD.

Todas las aplicaciones que están configuradas para la **autenticación integrada de Windows** tienen un inicio de sesión único perfecto al intentar el usuario acceder.  

Windows Hello para empresas requiere configuración adicional para habilitar el inicio de sesión único de forma local desde un dispositivo unido a Azure AD. Para más información, consulte [Configure Azure AD joined devices for On-premises Single-Sign On using Windows Hello for Business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-aadj-sso-base) (Configuración de los dispositivos unidos a Azure AD para el inicio de sesión único local con Windows Hello para empresas). 

## <a name="what-you-get"></a>Lo que obtiene

Con el inicio de sesión único, en un dispositivo unido a Azure AD puede: 

- Acceder a una ruta de acceso UNC de un servidor miembro de AD

- Acceder a un servidor web miembro de AD configurado para la seguridad integrada de Windows 



Si desea administrar su AD local desde un dispositivo Windows, instale las [Herramientas de administración remota del servidor para Windows 10](https://www.microsoft.com/en-us/download/details.aspx?id=45520).

Puede usar:

- El complemento Usuarios y equipos de Active Directory (ADUC) para administrar todos los objetos de AD. Sin embargo, deberá especificar el dominio al que desea conectarse manualmente.

- El complemento DHCP para administrar un servidor DHCP unido a AD. Sin embargo, deberá especificar el nombre del servidor DHCP o la dirección.

 
## <a name="what-you-should-know"></a>Qué debería saber

Es posible que tenga que ajustar el [filtrado basado en dominios](../hybrid/how-to-connect-sync-configure-filtering.md#domain-based-filtering) en Azure AD Connect para asegurarse de que se sincronizan los datos sobre los dominios necesarios.

Las aplicaciones y los recursos que dependen de la autenticación de la máquina en Active Directory no funcionan, ya que los dispositivos unidos a Azure AD no tienen un objeto de equipo en AD. 

No se pueden compartir archivos con otros usuarios en un dispositivo unido a Azure AD.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte [¿Qué es la administración de dispositivos en Azure Active Directory?](overview.md) 
