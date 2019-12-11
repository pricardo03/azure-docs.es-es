---
title: ¿Qué es el aprovisionamiento en la nube de Azure AD Connect? | Microsoft Docs
description: Aquí se describe el aprovisionamiento en la nube de Azure AD Connect.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e88f6f344c77109ee01cd6b0332138518f70270d
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795339"
---
# <a name="what-is-azure-ad-connect-cloud-provisioning"></a>Qué es el aprovisionamiento en la nube de Azure AD Connect
El aprovisionamiento en la nube de Azure AD Connect es un nuevo agente de Microsoft diseñado para cumplir objetivos de identidad híbrida para la sincronización de usuarios, grupos y contactos en Azure AD.  Se puede usar junto con la sincronización de Azure AD Connect y proporciona las siguientes ventajas:
    
- Compatibilidad con la sincronización con un inquilino de Azure AD desde un entorno de varios bosques de Active Directory desconectado: Entre los escenarios comunes se incluyen la fusión y adquisición, donde los bosques de AD de la empresa adquirida están aislados de los bosques de AD de la empresa matriz, y empresas que históricamente tenían varios bosques de AD.
- Instalación simplificada con agentes de aprovisionamiento ligeros: Los agentes actúan como un puente de AD a Azure AD, con toda la configuración de sincronización administrada en la nube. 
- Se pueden usar varios agentes de aprovisionamiento para simplificar las implementaciones de alta disponibilidad, especialmente críticas para las organizaciones que dependen de la sincronización de hash de contraseñas de AD a Azure AD.


![Qué es Azure AD Connect](media/what-is-cloud-provisioning/architecture.png)

## <a name="how-is-azure-ad-connect-cloud-provisioning-different-from-azure-ad-connect-sync"></a>¿En qué se diferencia el aprovisionamiento en la nube de Azure AD Connect de la sincronización de Azure AD Connect?
Con el aprovisionamiento en la nube de Azure AD Connect, el aprovisionamiento de AD a Azure AD se organiza en Microsoft Online Services. Una organización solo tiene que implementar en su entorno local y hospedado en IaaS un agente ligero que actúa como puente entre Azure AD y AD. La configuración de aprovisionamiento se almacena en Azure AD y se administra como parte del servicio.

En la tabla siguiente se proporciona una comparación entre Azure AD Connect y el aprovisionamiento en la nube de Azure AD Connect:

| Característica | Sincronización de Azure Active Directory Connect| Aprovisionamiento en la nube de Azure Active Directory Connect |
|:--- |:---:|:---:|
|Conexión a un bosque de AD local|● |● |
| Conexión a varios bosques de AD locales |● |● |
| Conexión a varios bosques de AD locales desconectados | |● |
| Modelo de instalación de agente ligero | |● |
| Varios agentes activos para alta disponibilidad | |● |
| Conexión a directorios LDAP|●| | 
| Compatibilidad con objetos de usuario |● |● |
| Compatibilidad con objetos de grupo |● |● |
| Compatibilidad con objetos de contacto |● |● |
| Compatibilidad con objetos de dispositivo |● | |
| Permitir personalización básica para flujos de atributo |● |● |
| Sincronización de atributos de AD definidos por el cliente (extensiones de directorio) |● | |
| Compatibilidad con la sincronización de hash de contraseñas |●|●|
| Compatibilidad con la autenticación de paso a través |●||
| Compatibilidad con la federación |●|●|
| Inicio de sesión único de conexión directa|● |●|
| Admite la instalación de un controlador de dominio |● |● |
| Compatibilidad con Windows Server 2012 y Windows Server 2012 R2 |● |● |
| Filtrar por dominios/unidades organizativas/grupos |● |● |
| Filtrar según valores de atributo de los objetos |● | |
| Permitir sincronización de un conjunto mínimo de atributos (MinSync) |● |● |
| Permitir quitar atributos del flujo de AD a Azure AD |● |● |
| Permitir personalización avanzada para flujos de atributo |● | |
| Compatibilidad con la escritura diferida (contraseñas, dispositivos, grupos) |● | |

## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)
