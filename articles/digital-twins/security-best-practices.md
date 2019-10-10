---
title: 'Procedimientos recomendados de seguridad: Azure Digital Twins | Microsoft Docs'
description: Aprenda los procedimientos recomendados de seguridad para Azure Digital Twins y el Internet de las cosas.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 09/30/2019
ms.openlocfilehash: ba10a6105972eddc8474841b234d2eeddc994f5d
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2019
ms.locfileid: "71948945"
---
# <a name="security-best-practices"></a>Prácticas recomendadas de seguridad

La seguridad de Azure Digital Twins permite el acceso preciso a recursos y acciones específicos del grafo de IoT. Para ello, se usa la administración pormenorizada de roles y permisos que se conoce como [control de acceso basado en rol](./security-role-based-access-control.md).

Azure Digital Twins usa también otras características de seguridad que están presentes en Azure IoT, incluido Azure Active Directory (Azure AD). Por ese motivo, la configuración y protección de aplicaciones compiladas en Azure Digital Twins implica el uso de muchos de los mismos [procedimientos de seguridad de Azure IoT](../iot-fundamentals/iot-security-best-practices.md) actualmente recomendados.

En este artículo se resumen los principales procedimientos recomendados que deben seguirse.

> [!IMPORTANT]
> Para garantizar la máxima seguridad para el espacio de IoT, revise los recursos de seguridad adicionales. Asegúrese de incluir los proveedores de dispositivos.

> [!TIP]
> Use [Azure Security Center para IoT](https://docs.microsoft.com/azure/asc-for-iot/) para ayudar a detectar vulnerabilidades y amenazas de seguridad de IoT.

## <a name="iot-security-best-practices"></a>Procedimientos recomendados para la seguridad de IoT

Algunos de los procedimientos principales para proteger los dispositivos IoT de forma segura son:

> [!div class="checklist"]
> * Proteja cada dispositivo que esté conectado al espacio de IoT con medios a prueba de manipulaciones.
> * Limite el rol de cada dispositivo, sensor y persona dentro del espacio de IoT. Si se pone en riesgo, el efecto será menor.
> * Considere la posibilidad de usar el filtrado de direcciones IP de dispositivo y la restricción de puertos.
> * Limite el ancho de banda de E/S y dispositivo para mejorar el rendimiento. La limitación de velocidad puede mejorar la seguridad al impedir ataques por denegación de servicio.
> * Mantenga actualizado el firmware del dispositivo.
> * Audite y revise periódicamente los procedimientos recomendados de seguridad de dispositivos, redes y puertas de enlace, ya que siguen mejorando y evolucionando.

Algunos de los procedimientos más importantes para proteger de forma segura un espacio de IoT son:

> [!div class="checklist"]
> * Cifre los datos persistentes, guardados o almacenados.
> * Exija el cambio o la actualización de claves y contraseñas de forma periódica.
> * Restrinja cuidadosamente el acceso y los permisos por rol. Consulte la sección [Procedimientos recomendados del control de acceso basado en rol](#role-based-access-control-best-practices).
> * Considere una topología de red dividida para que los dispositivos de cada red estén aislados de los demás.
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

## <a name="role-based-access-control-best-practices"></a>Procedimientos recomendados del control de acceso basado en roles

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los procedimientos recomendados de Azure IoT, lea [Procedimientos recomendados de seguridad de Internet de las cosas](../iot-fundamentals/iot-security-best-practices.md).

* Para información sobre el control de acceso basado en rol, lea [Control de acceso basado en rol](./security-role-based-access-control.md).

* Para información sobre la autenticación, lea [Autenticación con las API](./security-authenticating-apis.md).