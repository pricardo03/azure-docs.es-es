---
title: Configuración de la conectividad SSL en Azure Database for PostgreSQL
description: Instrucciones e información para configurar Azure Database for PostgreSQL y las aplicaciones asociadas, a fin de usar correctamente las conexiones SSL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: 5a0fc99052b18dc1fa837147aa914a473d27d832
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60871414"
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a>Configuración de la conectividad SSL en Azure Database for PostgreSQL
Azure Database for PostgreSQL prefiere conectar las aplicaciones cliente al servicio de PostgreSQL mediante la Capa de sockets seguros (SSL). Aplicar conexiones SSL entre el servidor de base de datos y las aplicaciones cliente ayuda a proteger contra los ataques de tipo "man in the middle" mediante el cifrado del flujo de datos entre el servidor y la aplicación.

De forma predeterminada, el servicio de base de datos de PostgreSQL está configurado para requerir la conexión SSL. Si lo desea, puede deshabilitar el requisito de SSL para conectarse al servicio de base de datos en caso de que la aplicación cliente no admita la conectividad SSL. 

## <a name="enforcing-ssl-connections"></a>Aplicación de conexiones SSL
Para todos los servidores de Azure Database for PostgreSQL aprovisionados en Azure Portal o con la CLI de Azure, el establecimiento de conexiones SSL está habilitado de forma predeterminada. 

Del mismo modo, las cadenas de conexión que están predefinidas en la configuración de "Cadenas de conexión" del servidor en Azure Portal incluyen los parámetros necesarios para que los lenguajes comunes se conecten al servidor de base de datos mediante SSL. El parámetro SSL varía según el conector, por ejemplo, "ssl = true" o "sslmode = require" o "sslmode = required" y otras variaciones.

## <a name="configure-enforcement-of-ssl"></a>Configuración del establecimiento de SSL
Si lo desea, puede deshabilitar el establecimiento de la conectividad SSL. Microsoft Azure recomienda habilitar siempre la opción de configuración **Enforce SSL connection** (Establecer conexión SSL) para mayor seguridad.

### <a name="using-the-azure-portal"></a>Uso de Azure Portal
Visite el servidor de Azure Database for PostgreSQL y haga clic en **Seguridad de conexión**. Use el botón de alternancia para habilitar o deshabilitar la opción **Aplicar conexión SSL**. A continuación, haga clic en **Guardar**. 

![Seguridad de conexión - Deshabilitar el establecimiento de SSL](./media/concepts-ssl-connection-security/1-disable-ssl.png)

Puede confirmar la configuración; para ello, consulte la página de **información general** para ver el indicador de **estado de establecimiento de SSL**.

### <a name="using-azure-cli"></a>Uso de la CLI de Azure
Puede habilitar o deshabilitar el parámetro **ssl-enforcement** con los valores `Enabled` o `Disabled` respectivamente en la CLI de Azure.

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Comprobación de que la aplicación o el entorno admiten conexiones SSL
Muchos marcos de aplicaciones comunes que usan PostgreSQL en los servicios de bases de datos (como Drupal y Django), no habilitan SSL de forma predeterminada durante la instalación. La conectividad SSL se debe habilitar después de la instalación, o bien mediante comandos de la CLI específicos de la aplicación. Si el servidor PostgreSQL establece conexiones SSL y la aplicación asociada no está configurada correctamente, es posible que esta última no pueda conectarse al servidor de bases de datos. Consulte la documentación de la aplicación para obtener información sobre cómo habilitar las conexiones SSL.


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>Aplicaciones que requieren la verificación de certificados para la conectividad SSL
En algunos casos, las aplicaciones requieren un archivo de certificado local generado a partir de un archivo de certificado (.cer) de una entidad de certificación (CA) de confianza para conectarse de forma segura. Consulte los pasos siguientes para obtener el archivo .cer, descodificar el certificado y enlazarlo a la aplicación.

### <a name="download-the-certificate-file-from-the-certificate-authority-ca"></a>Descarga del archivo de certificado de la entidad de certificación (CA) 
El certificado necesario para comunicarse a través de SSL con el servidor de Azure Database for PostgreSQL se encuentra [aquí](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt). Descargue el archivo de certificado en una unidad local.

### <a name="install-a-cert-decoder-on-your-machine"></a>Instale un descodificador de certificado en el equipo 
Puede usar [OpenSSL](https://github.com/openssl/openssl) para descodificar el archivo de certificado necesario para que la aplicación para conectarse de forma segura a su servidor de base de datos. Para obtener información sobre cómo instalar OpenSSL, consulte el [instrucciones de instalación de OpenSSL](https://github.com/openssl/openssl/blob/master/INSTALL). 


### <a name="decode-your-certificate-file"></a>Descodificación del archivo de certificado
El archivo descargado de la entidad de certificación raíz está en formato cifrado. Utilice OpenSSL para descodificar el archivo de certificado. Para ello, ejecute este comando de OpenSSL:

```
openssl x509 -inform DER -in BaltimoreCyberTrustRoot.crt -text -out root.crt
```

### <a name="connecting-to-azure-database-for-postgresql-with-ssl-certificate-authentication"></a>Conexión a Azure Database for PostgreSQL con la autenticación de certificados SSL
Ahora que ha descodificado correctamente el certificado, ya puede conectarse al servidor de bases de datos de forma segura mediante SSL. Para permitir la verificación del certificado del servidor, el certificado debe estar en el archivo ~/.postgresql/root.crt del directorio principal del usuario. (En Microsoft Windows el archivo se denomina %APPDATA%\postgresql\root.crt). 

#### <a name="connect-using-psql"></a>Conectarse mediante psql
En el ejemplo siguiente se muestra cómo conectarse correctamente a su servidor de PostgreSQL mediante la utilidad de línea de comandos psql. Use el archivo `root.crt` creado y las opciones `sslmode=verify-ca` o `sslmode=verify-full`.

Mediante la interfaz de la línea de comandos de PostgreSQL, ejecute el siguiente comando:
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=mylogin@mydemoserver"
```
Si la operación se realiza correctamente, debe recibir la salida siguiente:
```bash
Password for user mylogin@mydemoserver:
psql (9.6.2)
WARNING: Console code page (437) differs from Windows code page (1252)
     8-bit characters might not work correctly. See psql reference
     page "Notes for Windows users" for details.
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
Type "help" for help.

postgres=>
```

## <a name="next-steps"></a>Pasos siguientes
Revise las distintas opciones de conectividad de aplicaciones en [Bibliotecas de conexiones de Azure Database for PostgreSQL](concepts-connection-libraries.md).
