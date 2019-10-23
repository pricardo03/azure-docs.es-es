---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/14/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 93f6bc8533218af7f0e6dcd1c5f7be6fe8c00e29
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "69520880"
---
[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-to-generate-certificates-for-point-to-site-connectivity"></a>¿Puedo usar mi propio CA raíz de PKI interna para generar certificados de conectividad de punto a sitio?

Sí. Anteriormente, solo podían usarse certificados raíz autofirmados. Todavía puede cargar 20 certificados raíz.

### <a name="can-i-use-certificates-from-azure-key-vault"></a>¿Puedo usar certificados de Azure Key Vault?

No.

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
