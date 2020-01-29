---
title: Configuración de OpenSSL para Linux
titleSuffix: Azure Cognitive Services
description: Aprenda a configurar OpenSSL para Linux.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: jhakulin
ms.openlocfilehash: cadf31dede8ee81323076013d00b9431f597bda6
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76156495"
---
# <a name="configure-openssl-for-linux"></a>Configuración de OpenSSL para Linux

Cuando usa cualquier versión del SDK de Voz anterior a la 1.9.0, [OpenSSL](https://www.openssl.org) se configura dinámicamente en la versión del sistema host. En versiones posteriores del SDK de Voz, OpenSSL (versión [1.1.1 b](https://mta.openssl.org/pipermail/openssl-announce/2019-February/000147.html)) se vincula estáticamente a la biblioteca principal del SDK de Voz.

## <a name="troubleshoot-connectivity"></a>Solución de problemas de conectividad

Si hay errores de conexión al usar la versión 1.9.0 del SDK de Voz, asegúrese de que el directorio `ssl/certs` existe en el directorio `/usr/lib`, que se encuentra en el sistema de archivos de Linux. Si el directorio `ssl/certs` *no existe*, compruebe dónde está instalado OpenSSL en el sistema con el siguiente comando:

```bash
which openssl
```

A continuación, busque el directorio `certs` de OpenSSL y copie el contenido de ese directorio en el directorio `/usr/lib/ssl/certs`. Después, vuelva a intentarlo para ver si se han resuelto los problemas de conectividad.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Acerca del SDK de Voz](speech-sdk.md)