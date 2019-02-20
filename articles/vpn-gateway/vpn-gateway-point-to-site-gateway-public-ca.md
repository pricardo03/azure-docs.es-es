---
title: Transición de los certificados de entidad de certificación de autofirmados a públicos para puertas de enlace P2S | Azure VPN Gateway | Microsoft Docs
description: Este artículo lo ayuda a realizar una transición exitosa a los nuevos certificados de entidad de certificación públicos para puertas de enlace P2S.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: cherylmc
ms.openlocfilehash: ac1ae4125418a9c0b3e9587cd03a44e752ac8f82
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56236964"
---
# <a name="transition-from-self-signed-to-public-ca-certificates-for-p2s-gateways"></a>Transición de los certificados de entidad de certificación de autofirmados a públicos para puertas de enlace P2S

Azure VPN Gateway ya no emite certificados autofirmados para puertas de enlace de conexiones P2S. Los certificados emitidos ahora están firmados por una entidad de certificación (CA) pública. Sin embargo, las puertas de enlace anteriores todavía utilizan certificados autofirmados. Estos certificados autofirmados están a punto de expirar y deben realizar la transición a los certificados de entidad de certificación pública.

Anteriormente, un certificado autofirmado para la puerta de enlace debía actualizarse cada 18 meses. Los archivos de configuración de cliente VPN tenían que generarse y volver a implementarse en todos los clientes P2S. Al migrar a los certificados de entidad de certificación pública, se elimina esta limitación. Además de la transición para certificados, este cambio también proporciona mejoras en la plataforma, una métrica mejor y estabilidad mejorada.

Solo las puertas de enlace anteriores se ven afectados por este cambio. Si el certificado de puerta de enlace se puede migrar, recibirá un aviso en Azure Portal. Puede ver si se ve afectada la puerta de enlace siguiendo los pasos de este artículo.

>[!IMPORTANT]
>La transición está programada para el 12 de marzo de 2019 a partir de las 18:00 UTC. Puede crear una incidencia de soporte técnico si prefiere un período diferente. Cree y finalice la solicitud con al menos 24 horas de antelación.  Puede solicitar una de las ventanas siguientes:
>
>* 06:00 UTC el 25 de febrero
>* 18:00 UTC el 25 de febrero
>* 06:00 UTC el 1 de marzo
>* 18:00 UTC el 1 de marzo
>
>**Todas las puertas de enlace restantes realizarán la transición el 12 de marzo de 2019 a partir de las 18:00 UTC**.
>

## <a name="1-verify-your-certificate"></a>1. Comprobación del certificado

1. Compruebe si se ven afectadas por esta actualización. Descargar la configuración actual del cliente VPN mediante los pasos descritos en [este artículo](point-to-site-vpn-client-configuration-azure-cert.md).

2. Abra o extraiga el archivo ZIP y vaya a la carpeta "Generic". En la carpeta Generic, verá dos archivos, uno de los cuales es *VPNSettings.xml*.
3. Abra *VPNSettings.xml* en cualquier editor o visor de XML. En el archivo XML, busque los siguientes campos:

  * `<ServerCertRootCn>DigiCert Global Root CA</ServerCertRootCn>`
  * `<ServerCertIssuerCn>DigiCert Global Root CA</ServerCertIssuerCn>`
4. Si *ServerCertRotCn* y *ServerCertIssuerCn* son "DigiCert Global Root CA", no se ven afectados por esta actualización y no es necesario continuar con los pasos descritos en este artículo. Sin embargo, si aparece algo más, el certificado de puerta de enlace forma parte de la actualización y realizará la transición.

## <a name="2-check-certificate-transition-schedule"></a>2. Comprobación de la programación de transición del certificado

Si el certificado forma parte de la actualización, el certificado de puerta de enlace se migrará. Consulte esta nota **importante** de tiempos de transición. Tras la actualización, los clientes P2S no podrán conectarse con su perfil antiguo. Debe generar nuevos perfiles de cliente VPN e instalarlos en los clientes.

## <a name="3-generate-vpn-client-configuration-profile"></a>3. Generación del perfil de configuración de cliente VPN

Una vez que se ha migrado el certificado, descargue el nuevo perfil VPN (archivos de configuración de cliente VPN) desde Azure Portal. Para ver los pasos, consulte [Creación e instalación de los archivos de configuración de cliente de VPN](point-to-site-vpn-client-configuration-azure-cert.md). No es necesario generar nuevos certificados de cliente.

## <a name="4-deploy-vpn-client-profile"></a>4. Implementación del perfil de cliente VPN

Implemente el nuevo perfil en todos los clientes VPN de punto a sitio. Los clientes VPN perderán conectividad hasta que el nuevo perfil VPN para conexiones de punto a sitio se descargue e implemente en los dispositivos cliente. No es necesario volver a emitir los certificados de cliente que ya están instalados en los equipos cliente VPN.

## <a name="5-connect-the-vpn-client"></a>5. Conexión del cliente VPN

Conéctese a Azure desde el cliente VPN como lo haría normalmente.
