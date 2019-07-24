---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e2e91dc91cf0fbe6827808785a4c3cc25b06542b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186135"
---
[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>¿Puedo usar mi propio CA raíz de PKI interna para la conectividad de punto a sitio?

Sí. Anteriormente, solo podían usarse certificados raíz autofirmados. Todavía puede cargar 20 certificados raíz.

### <a name="what-tools-can-i-use-to-create-certificates"></a>¿Qué herramientas puedo usar para crear certificados?

Puede usar la solución Enterprise PKI (la PKI interna), Azure PowerShell, MakeCert y OpenSSL.

### <a name="certsettings"></a>¿Hay instrucciones para los parámetros y la configuración de certificados?

* **Solución PKI interna/PKI de empresa:** Vea los pasos para [Generar certificados](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert).

* **Azure PowerShell:** Vea el artículo [Azure PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md) para conocer los pasos.

* **MakeCert:** Vea el artículo [MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md) para conocer los pasos.

* **OpenSSL:** 

    * Al exportar certificados, asegúrese de convertir el certificado raíz en Base64.

    * Para el certificado de cliente:

      * Al crear la clave privada, especifique la longitud como 4096.
      * Al crear el certificado para el parámetro *-extensions*, especifique *usr_cert*.