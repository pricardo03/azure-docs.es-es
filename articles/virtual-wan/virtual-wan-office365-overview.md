---
title: Plano de control de Office 365 en Azure Virtual WAN
description: Obtenga información acerca del plano de control de Office 365 en Virtual WAN.
author: cherylmc
ms.service: virtual-wan
services: virtual-wan
ms.topic: article
ms.date: 9/24/2018
ms.author: cherylmc
ms.openlocfilehash: cb91c1364a91c101ecf8362acd7aab01440143fc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60458605"
---
# <a name="office-365-control-plane-in-virtual-wan"></a>Plano de control de Office 365 en la Virtual WAN

Los clientes de Virtual WAN con determinados dispositivos SDWAN pueden configurar directivas de sesión de subgrupos de Office 365 por Internet para el tráfico de confianza en Azure Portal. Esto permite:
- que el tráfico de Office 365 ingrese a la red de Microsoft cerca del usuario, lo que proporciona una experiencia de usuario óptima.
- evitar que el tráfico se enrute en retroceso o se redireccione al origen, ahorrando así los costes de WAN.
- cumplir con los principios de conectividad de Office 365.

## <a name="faqs"></a>Preguntas más frecuentes
### <a name="what-is-the-customer-benefit"></a>¿Cuál es la ventaja para el cliente?
Con esta característica en Virtual WAN, los clientes ahora pueden especificar en qué categorías del tráfico de Office 365 confían para la sesión de subgrupos directa por internet. Este tráfico de confianza de Office 365 omitirá los servidores proxy y se enrutará directamente desde la ubicación del usuario hacia la ubicación de punto de preferencia de Microsoft más cercana. Esto evita que el tráfico se enrute en retroceso y se redireccione al origen, por lo que proporciona una experiencia de usuario óptima y ahorra en costes de WAN. 

### <a name="what-are-the-office-365-traffic-categories"></a>¿Cuáles son las categorías de tráfico de Office 365?
Los puntos de conexión de Office 365 representan direcciones de redes y subredes. Los puntos de conexión pueden ser direcciones URL, intervalos de direcciones IP o direcciones IP. Las direcciones URL pueden ser un FQDN como *account.office.net*, o una dirección URL de carácter comodín como **.office365.com*. Los puntos de conexión se distribuyen en tres categorías: **optimizar**, **permitir**, y **predeterminado**, según su importancia. Encontrará más detalles acerca de las categorías de punto de conexión [aquí](https://docs.microsoft.com/office365/enterprise/office-365-network-connectivity-principles#BKMK_Categories).

### <a name="which-office-365-traffic-category-is-recommended-by-microsoft-for-direct-internet-breakout"></a>¿Qué categoría de tráfico de Office 365 recomienda Microsoft para la sesión de subgrupos directa por internet?
La categoría **Optimizar** es para los puntos de conexión de red más importantes y es obligatoria para omitir la inspección e interrupción SSL y otros dispositivos de seguridad de red. Debe tener una salida de Internet directa cerca de los usuarios. Estos puntos de conexión representan escenarios de Office 365 que son más sensibles a la disponibilidad, latencia y rendimiento de la red. Esta categoría incluye un conjunto pequeño de (aproximadamente 10) direcciones URL claves y un conjunto definido de subredes IP dedicadas a las cargas de trabajo principales de Office 365, como Exchange Online, SharePoint Online, Skype Empresarial Online y Microsoft Teams. 

La categoría **Permitir** también se recomienda para la salida de Internet directa. Aunque el tráfico de red de la categoría Permitir puede tolerar cierta latencia de red. Los puntos de conexión de las categorías Optimizar y Permitir se hospedan en centros de datos de Microsoft y se administran como parte de Office 365. La categoría Predeterminado se puede dirigir a una ubicación de salida de Internet predeterminada y no requiere de una salida de Internet directa ni de omitir las interrupciones SSL y la inspección de dispositivos.

### <a name="how-do-i-set-my-o365-policies-via-virtual-wan"></a>¿Cómo configuro mis directivas de Office 365 a través de Virtual WAN?
Puede habilitar las directivas a través de la pestaña **Virtual WAN** -> **Configuración** -> **Configuración**. Aquí puede especificar sus categorías de tráfico de Office 365 preferidas para la sesión de subgrupos directa por Internet.

![Configuración del plano de control de Office 365 en la Virtual WAN](media/virtual-wan-office365-overview/configure-office365-control-plane.png)

### <a name="how-does-this-work"></a>¿Cómo funciona esto?

1.  El tráfico de Office 365 ingresa a la red de Microsoft cerca del usuario, lo que proporciona una experiencia óptima.
2.  SDWAN consume las directivas de ruta. A continuación, omite los servidores proxy de seguridad para las categorías de confianza y realiza sesiones de subgrupo directas locales para estas categorías.
3.  Se evita que el tráfico se enrute en retroceso o se redireccione al origen, lo que ahorra costes de WAN.

### <a name="which-partner-devices-support-this-via-virtual-wan"></a>¿Qué dispositivos asociados son compatibles con estas directivas a través de Virtual WAN?
Actualmente, Citrix admite estas directivas a través de Virtual WAN.

### <a name="what-happens-to-the-remaining-categories-of-untrusted-o365-traffic"></a>¿Qué ocurre con las categorías restantes de tráfico (que no es de confianza) de Office 365?
El tráfico de Office 365 restante sigue la ruta de acceso de tráfico de Internet predeterminada para clientes.

### <a name="what-if-i-have-already-specified-my-o365-policies-via-my-sdwan-provider"></a>¿Qué ocurre si ya he especificado las directivas de Office 365 a través de mi proveedor SDWAN?
Si especifica las directivas a través de SDWAN UX y Azure Virtual WAN, las directivas establecidas en Virtual WAN tendrán prioridad.

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre [Virtual WAN](virtual-wan-about.md).