---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 12/11/2018
ms.date: 12/24/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: c0ce4e882f270f5e0c789a608aaada5c6c9cba92
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60320207"
---
Si tiene problemas para conectarse, compruebe los siguientes elementos:

- Si ha exportado un certificado de cliente con el **Asistente para exportar certificados**, asegúrese de haberlo exportado como un archivo .pfx y de haber seleccionado **Incluir todos los certificados en la ruta de certificación (si es posible)**. Si lo exporta con este valor, también se exporta la información del certificado raíz. Una vez instalado el certificado en el equipo cliente, también se instala el certificado raíz del archivo .pfx. Para verificar que el certificado raíz está instalado, abra **Administrar certificados de usuario** y seleccione **Entidades de certificación raíz de confianza \Certificados**. Verifique que el certificado raíz está presente, ya que es necesario para que la autenticación funcione.

- Si usó un certificado emitido por una solución de CA empresarial y no puede autenticarse, verifique el orden de autenticación en el certificado de cliente. Compruebe el orden de la lista de autenticación; para ello, haga doble clic en el certificado de cliente, seleccione la pestaña **Detalles** y, después, seleccione **Uso mejorado de clave**. Asegúrese de que *Autenticación de cliente* sea el primer elemento de la lista. Si no es así, emita un certificado de cliente basado en la plantilla Usuario que tenga *Autenticación de cliente* como primer elemento de la lista.

- Para información adicional de solución de problemas de P2S, consulte [Solución de problemas de conexiones P2S](../articles/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
