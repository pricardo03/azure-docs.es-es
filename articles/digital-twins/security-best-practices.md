---
title: Procedimientos recomendados de seguridad de Azure Digital Twins | Microsoft Docs
description: Procedimientos recomendados de seguridad de Azure Digital Twins.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: v-adgera
ms.openlocfilehash: 64a980ba8ed581b3cad369951220d47baf9e17ab
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849300"
---
# <a name="security-best-practices"></a>Prácticas recomendadas de seguridad

La seguridad de Azure Digital Twins permite el acceso preciso a recursos y acciones específicos del grafo de IoT. Para ello, se usa la administración pormenorizada de roles y permisos que se conoce como [control de acceso basado en rol](./security-role-based-access-control.md).

Azure Digital Twins usa también otras características de seguridad que están presentes en Azure IoT, incluido Azure Active Directory (Azure AD). Por ese motivo, la configuración y protección de aplicaciones compiladas en Azure Digital Twins implica el uso de muchos de los mismos [procedimientos de seguridad de Azure IoT](../iot-fundamentals/iot-security-best-practices.md) actualmente recomendados.

En este artículo se resumen los principales procedimientos recomendados que deben seguirse.

> [!IMPORTANT]
> Para garantizar la máxima seguridad para el espacio de IoT, revise los recursos de seguridad adicionales. Asegúrese de incluir los proveedores de dispositivos.

## <a name="iot-security-best-practices"></a>Procedimientos recomendados para la seguridad de IoT

Algunos de los procedimientos principales para proteger los dispositivos IoT de forma segura son:

> [!div class="checklist"]
> * Proteja cada dispositivo que esté conectado al espacio de IoT con medios a prueba de manipulaciones.
> * Limite el rol de cada dispositivo, sensor y persona dentro del espacio de IoT. Si se pone en riesgo, el efecto será menor.
> * Considere la posibilidad de usar el filtrado de direcciones IP de dispositivo y la restricción de puertos.
> * Limite el ancho de banda de E/S y dispositivo para mejorar el rendimiento. La limitación de velocidad puede mejorar la seguridad al impedir ataques por denegación de servicio.
> * Mantenga actualizado el firmware del dispositivo.

Algunos de los procedimientos más importantes para proteger de forma segura un espacio de IoT son:

> [!div class="checklist"]
> * Cifre los datos persistentes, guardados o almacenados.
> * Exija el cambio o la actualización de claves y contraseñas de forma periódica.
> * Restrinja cuidadosamente el acceso y los permisos por rol. Consulte la sección [Procedimientos recomendados del control de acceso basado en rol](#rbac).
> * Use cifrado seguro. Exija contraseñas largas y use protocolos seguros y autenticación en dos fases.

[Supervise](./how-to-configure-monitoring.md) los recursos de IoT para inspeccionar los valores atípicos, las amenazas o los parámetros de recursos que se encuentran fuera del intervalo de funcionamiento habitual. Use Azure Analytics para administrar la supervisión.

> [!NOTE]
> Para más información sobre el procesamiento y la supervisión de eventos, consulte [Enrutamiento de eventos y mensajes con Azure Digital Twins](./concepts-events-routing.md).

## <a name="azure-active-directory-best-practices"></a>Procedimientos recomendados de Azure Active Directory

Azure Digital Twins usa Azure Active Directory para autenticar a los usuarios y proteger las aplicaciones. Azure Active Directory admite la autenticación de diversas arquitecturas modernas. Todas se basan en protocolos estándar del sector como OAuth 2.0 u OpenID Connect. Algunos de los principales procedimientos recomendados para proteger su espacio de IoT en Azure Active Directory son:

> [!div class="checklist"]
> * Almacene secretos y claves de aplicación de Azure Active Directory en una ubicación segura, como [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).
> * Para autenticarse, use un certificado emitido por una [entidad emisora de certificados](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md) de confianza en lugar de secretos de aplicación.
> * Limite el ámbito de acceso de OAuth 2.0 para un token.
> * Compruebe la longitud de tiempo que es válido un token y si un token sigue siendo válido.
> * Establezca periodos de tiempo adecuados durante los cuales los tokens son válidos.
> * Actualice los tokens expirados.

<div id="rbac"></div>

## <a name="role-based-access-control-best-practices"></a>Procedimientos recomendados del control de acceso basado en roles

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los procedimientos recomendados de Azure IoT, lea [Procedimientos recomendados de seguridad de Internet de las cosas](../iot-fundamentals/iot-security-best-practices.md).

* Para información sobre el control de acceso basado en rol, lea [Control de acceso basado en rol](./security-role-based-access-control.md).

* Para información sobre la autenticación, lea [Autenticación con las API](./security-authenticating-apis.md).
