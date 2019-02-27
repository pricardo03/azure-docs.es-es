---
title: Versión preliminar de Protección con contraseña de Azure AD
description: Prohibición de contraseñas no seguras en una instancia de Active Directory local con la versión preliminar de Protección con contraseña de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/18/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1beae186f6eb276b9aa302d3d51f0ba8688e591
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56415755"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Vista previa: Aplicación de Protección con contraseña de Azure AD para Windows Server Active Directory

|     |
| --- |
| Protección con contraseña y Lista personalizada de contraseñas prohibidas de Azure AD son versiones preliminares públicas de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Protección con contraseña de Azure AD es la versión preliminar pública de una nueva característica diseñada por Azure Active Directory (Azure AD) para mejorar las directivas de contraseñas de una organización. La implementación local de Protección con contraseña de Azure AD usa listas globales y personalizadas de contraseñas prohibidas almacenadas en Azure AD, y realiza las mismas comprobaciones locales que los cambios basados en la nube de Azure AD.

## <a name="design-principles"></a>Principios de diseño

La Protección con contraseña de AD Azure para Active Directory está diseñada teniendo en cuenta los siguientes principios:

* Los controladores de dominio nunca tienen que comunicarse directamente con Internet
* No se abren nuevos puertos de red en los controladores de dominio.
* No se requiere ningún cambio de esquema de Active Directory. El software utiliza el contenedor de Active Directory existente y los objetos de esquema de serviceConnectionPoint.
* No hay ningún requisito mínimo de Dominio de Active Directory ni de nivel funcional del bosque (DFL/FFL).
* El software no crea ni requiere ninguna cuenta en los dominios de Active Directory que protege.
* Las contraseñas de usuario no cifradas nunca dejan el controlador de dominio (ya sea durante las operaciones de validación de contraseña o en cualquier otro momento).
* Se admite la implementación incremental a cambio de que la directiva de contraseñas se aplique solamente allí donde se haya instalado el agente de controlador de dominio.
* Se recomienda instalar el agente de controlador de dominio en todos los controladores de dominio para garantizar el cumplimiento de seguridad de protección con contraseña en todas partes.

## <a name="architectural-diagram"></a>Diagrama de arquitectura

Es importante comprender los conceptos funcionales y de subyacentes antes de implementar la Protección con contraseña de Azure AD en un entorno de Active Directory local. En el diagrama siguiente se muestra cómo los componentes de Protección con contraseña de Azure AD funcionan conjuntamente:

![Funcionamiento conjunto de los componentes de Protección con contraseña de Azure AD](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

En el diagrama anterior se muestran los tres componentes de software básicos que forman la Protección con contraseña de Azure AD:

* El servicio de proxy de Protección con contraseña de Azure AD se ejecuta en cualquier máquina unida a un dominio del bosque de Active Directory actual. Su principal finalidad es reenviar las solicitudes de descarga de la directiva de contraseña de los controladores de dominio a Azure AD y devolver la respuesta de Azure AD al controlador de dominio.
* La DLL de filtro de contraseña del agente de controlador de dominio de Protección con contraseña de Azure AD recibe solicitudes de validación de contraseñas del sistema operativo y las reenvía al servicio de agente de controlador de dominio de Protección con contraseña de Azure AD que se ejecuta localmente en el controlador de dominio.
* El servicio de agente de controlador de dominio de Protección con contraseña de Azure AD recibe solicitudes de validación de contraseñas de la DLL de filtro de contraseñas de este agente, las procesa mediante la directiva de contraseñas actual (disponible localmente) y devuelve el resultado (sin errores\con errores).

## <a name="theory-of-operations"></a>Teoría de operaciones

Teniendo en cuenta el diagrama y los principios de diseño anteriores, ¿cómo funciona realmente la Protección con contraseña de Azure AD?

Cada servicio de proxy de Protección con contraseña de Azure AD se anuncia a sí mismo a controladores de dominio del bosque mediante la creación de un objeto serviceConnectionPoint en Active Directory.

Cada servicio de agente de controlador de dominio de Protección de contraseña de Azure AD también crea un objeto serviceConnectionPoint en Active Directory. Sin embargo, este se utiliza principalmente para los informes y diagnósticos.

El servicio de agente de controlador de dominio de Protección de contraseña de Azure AD es el responsable de iniciar la descarga de una directiva de contraseña de Azure AD. El primer paso es localizar un servicio de proxy de Protección con contraseña de Azure AD mediante consultas al bosque para objetos serviceConnectionPoint del proxy. Una vez que se encuentra un servicio de proxy disponible, se envía una solicitud de descarga de la directiva de contraseña desde el servicio de agente de controlador de dominio para el servicio de proxy que, a su vez, se envía a Azure AD y luego devuelve la respuesta al servicio del agente de controlador de dominio. Después de recibir una directiva de contraseña de Azure AD, el servicio de agente de controlador de dominio almacena la directiva en una carpeta dedicada en la raíz de su recurso compartido sysvol de dominio. El servicio de agente de controlador de dominio también supervisa esta carpeta en caso de que las directivas más recientes se repliquen desde otros servicios de agente de controlador de dominio en el dominio.

El servicio de agente de controlador de dominio de Protección con contraseña de Azure AD siempre solicitará una nueva directiva en el inicio del servicio. Una vez que se inicia el servicio de agente de controlador de dominio, periódicamente (una vez cada hora) comprobará la antigüedad de la directiva actual localmente disponible; si la directiva actual es anterior a una hora, el servicio del agente de controlador de dominio solicitará una nueva directiva de Azure AD como se describió anteriormente; en caso contrario, el agente de controlador de dominio seguirá usando la directiva actual.

El servicio de agente de controlador de dominio de Protección con contraseña de Azure AD se comunica con el servicio de proxy de Protección con contraseña de Azure AD mediante RPC (llamada a procedimiento remoto) a través de TCP. El servicio de Proxy escucha estas llamadas en un puerto RPC dinámico o estático (según esté configurado).

El agente de controlador de dominio de protección de contraseña de Azure AD nunca escucha en un puerto disponible de red y el servicio de proxy nunca intenta llamar al servicio de agente de controlador de dominio.

El servicio de proxy de Protección con contraseña de Azure AD no tiene estado; nunca almacena en caché las directivas ni ningún otro estado descargado desde Azure.

El servicio de agente de controlador de dominio de Protección con contraseña de Azure AD solo evaluará la contraseña de un usuario mediante la directiva de contraseña localmente disponible más reciente. Si no hay ninguna directiva de contraseña disponible en el controlador de dominio local, la contraseña se aceptarán automáticamente y se registrará un mensaje de registro de eventos para advertir al administrador.

Protección con contraseña de Azure AD no es un motor de aplicación de directivas en tiempo real. Puede que se produzca un retraso desde que se realiza un cambio en la configuración de la directiva de contraseñas en Azure AD hasta que llega a todos los controladores de dominio y se aplica en todos ellos.

## <a name="foresttenant-binding-for-azure-ad-password-protection"></a>Enlace bosque\inquilino para Protección con contraseña de Azure AD

La implementación de Protección de contraseña de Azure AD en un bosque de Active Directory requiere el registro de dicho bosque, así como todos los servicios implementados de proxy de Protección con contraseña de Azure AD, con Azure AD. Ambos registros (de bosque y proxy) están asociados a un determinado inquilino de Azure AD que se identifica implícitamente a través de las credenciales utilizadas durante el registro. Cada vez que se descarga una directiva de contraseña de Protección con contraseña de Azure AD, siempre es específica para este inquilino (es decir, esa directiva siempre será una combinación de la lista de contraseñas prohibida global de Microsoft y la lista de contraseña prohibida personalizada por inquilino). No se admite para configurar diferentes dominios o proxies en un bosque para vincularlos a diferentes inquilinos de Azure AD.

## <a name="license-requirements"></a>Requisitos de licencia

Las ventajas de la lista global de contraseñas prohibidas se aplican a todos los usuarios de Azure Active Directory (Azure AD).

La lista personalizada de contraseñas prohibidas requiere licencias básicas de Azure AD.

Protección con contraseña de Azure AD para Windows Server Active Directory requiere licencias prémium de Azure AD.

Puede encontrar información adicional sobre licencias, incluidos los costos, en la [página de precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>Descargar

Los dos instaladores de agente requeridos para Protección con contraseña de Azure AD se pueden descargar del [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=57071).

## <a name="next-steps"></a>Pasos siguientes

[Implementación de la protección de contraseñas de Azure AD](howto-password-ban-bad-on-premises-deploy.md)
