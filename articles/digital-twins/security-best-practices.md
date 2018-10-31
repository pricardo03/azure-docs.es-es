---
title: Procedimientos recomendados de seguridad de Azure Digital Twins | Microsoft Docs
description: Procedimientos recomendados de seguridad de Azure Digital Twins
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: adgera
ms.openlocfilehash: 28eb8b5dc0f75b5e031070803d35c8a1ceb1f000
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364351"
---
# <a name="security-best-practices"></a>Procedimientos recomendados de seguridad

La seguridad de Azure Digital Twins permite el acceso preciso a recursos y acciones específicos del grafo de IoT. Para ello, se usa la administración pormenorizada de roles y permisos que se conoce como control de acceso basado en rol.

Azure Digital Twins también aprovecha otras características de seguridad presentes en Azure IoT, como Azure Active Directory. Por ese motivo, la configuración de la aplicación de Azure Digital Twins implica el uso de muchos de los mismos [procedimientos de seguridad de Azure IoT](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/) actualmente recomendados.

En este artículo se resumen los principales procedimientos recomendados que deben seguirse.

> [!IMPORTANT]
> Revise los recursos de seguridad adicionales (incluidos los proveedores de dispositivos) para garantizar la seguridad máxima del espacio de IoT.

## <a name="iot-security-best-practices"></a>Procedimientos recomendados de seguridad de IoT

Algunos de los procedimientos principales para proteger los dispositivos IoT de forma segura son:

> [!div class="checklist"]
> * Proteja cada dispositivo que esté conectado al espacio de IoT con medios a prueba de manipulaciones.
> * Limite el rol de cada dispositivo, sensor y persona dentro del espacio de IoT. Si se pone en riesgo, el impacto será menor.
> * Posible uso de filtrado de dirección IP de dispositivo.
> * Limite el ancho de banda de E/S y dispositivo para mejorar el rendimiento. La limitación de velocidad puede mejorar la seguridad al impedir ataques por denegación de servicio.

Algunos de los procedimientos más importantes para proteger de forma segura un espacio de IoT son:

> [!div class="checklist"]
> * Cifre los datos persistentes, guardados o almacenados.
> * Exija el cambio o la actualización de claves y contraseñas de forma periódica.
> * Restrinja cuidadosamente el acceso y los permisos por rol (consulte a continuación los procedimientos recomendados de control de acceso basado en rol).
> * Use cifrado seguro. Esto significa exigir contraseñas largas, mediante protocolos seguros y autenticación en dos fases.

La supervisión de recursos de IoT para inspeccionar los valores atípicos, las amenazas o los parámetros de recursos que se encuentran fuera del intervalo de funcionamiento habitual se administra mediante Azure Analytics.

> [!NOTE]
> Para más información sobre el procesamiento y la supervisión de eventos, consulte nuestro artículo sobre [enrutamiento de eventos](./concepts-events-routing.md).

## <a name="azure-active-directory-best-practices"></a>Procedimientos recomendados de Azure Active Directory

Azure Digital Twins usa Azure Active Directory para autenticar a los usuarios y proteger las aplicaciones. Azure Active Directory admite la autenticación en varias arquitecturas modernas, todas ellas basadas en los protocolos OAuth 2.0 u OpenID Connect estándar del sector. Algunos de los principales procedimientos recomendados para proteger su espacio de IoT en Azure Active Directory son:

> [!div class="checklist"]
> * Almacene secretos y claves de aplicación de Azure Active Directory en una ubicación segura, como [Key Vault](https://azure.microsoft.com/services/key-vault/).
> * Para autenticarse, use un certificado emitido por [entidades emisoras de certificados](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-certificate-based-authentication-get-started) de confianza en lugar de secretos de aplicación.
> * Limite el ámbito de acceso de OAuth 2.0 para un token.
> * Compruebe la longitud de tiempo que es válido un token y si un token sigue siendo válido.
> * Establezca periodos de tiempo adecuados durante los cuales los tokens son válidos.
> * Actualice los tokens expirados.

## <a name="role-based-access-control-best-practices"></a>Procedimientos recomendados del control de acceso basado en roles

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los procedimientos recomendados de Azure IoT, lea [Procedimientos recomendados de seguridad de Internet de las cosas](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/).

Para información sobre el control de acceso basado en rol, lea [Control de acceso basado en rol](./security-role-based-access-control.md).

Para información sobre la autenticación, lea [Autenticación con API](./security-authenticating-apis.md).
