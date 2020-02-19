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
ms.openlocfilehash: ff8772f7c3c3213c010b0bdbd0d0aa8897404bac
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77119986"
---
# <a name="configure-openssl-for-linux"></a>Configuración de OpenSSL para Linux

Cuando usa cualquier versión del SDK de Voz anterior a la 1.9.0, [OpenSSL](https://www.openssl.org) se configura dinámicamente en la versión del sistema host. En versiones posteriores del SDK de Voz, OpenSSL (versión [1.1.1 b](https://mta.openssl.org/pipermail/openssl-announce/2019-February/000147.html)) se vincula estáticamente a la biblioteca principal del SDK de Voz.

Para garantizar la conectividad, compruebe que se han instalado los certificados de OpenSSL en el sistema. Ejecute un comando:
```bash
openssl version -d
```

La salida en los sistemas basados en Ubuntu/Debian debe ser:
```
OPENSSLDIR: "/usr/lib/ssl"
```

Compruebe si existe el subdirectorio `certs` en OPENSSLDIR. En el ejemplo anterior, sería `/usr/lib/ssl/certs`.

* Si `/usr/lib/ssl/certs` existe y contiene muchos archivos de certificado individuales (con la extensión `.crt` o `.pem`), no es necesario realizar más acciones.

* Si OPENSSLDIR es algo distinto de `/usr/lib/ssl` y/o hay un único archivo de paquete de certificados en lugar de varios archivos individuales, debe establecer una variable de entorno SSL adecuada para indicar dónde se pueden encontrar los certificados.

## <a name="examples"></a>Ejemplos

- OPENSSLDIR is `/opt/ssl`. Existe un subdirectorio `certs` con muchos archivos `.crt` o `.pem`.
Establezca la variable de entorno `SSL_CERT_DIR` para que apunte a `/opt/ssl/certs` antes de ejecutar un programa que utilice el SDK de Voz. Por ejemplo:
```bash
SSL_CERT_DIR=/opt/ssl/certs ./helloworld
```

- OPENSSLDIR is `/etc/pki/tls`. Existe un archivo de paquete de certificados, por ejemplo `ca-bundle.pem` o `ca-bundle.crt`.
Establezca la variable de entorno `SSL_CERT_FILE` para que apunte a `/etc/pki/tls/ca-bundle.pem` antes de ejecutar un programa que utilice el SDK de Voz. Por ejemplo:
```bash
SSL_CERT_FILE=/etc/pki/tls/ca-bundle.pem ./helloworld
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Acerca del SDK de Voz](speech-sdk.md)
