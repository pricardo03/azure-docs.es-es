---
title: 'Protección de las aplicaciones cliente y servidor de OPC UA mediante OPC Vault: Azure | Microsoft Docs'
description: Proteja las aplicaciones cliente y servidor de OPC UA con un nuevo par de claves y un certificado mediante OPC Vault.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 5ba2dba02585598b3797dd1b490976ebe34b489e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61450671"
---
# <a name="secure-opc-ua-client-and-opc-ua-server-application"></a>Protección de las aplicaciones cliente y servidor de OPC UA 
OPC Vault es un microservicio que puede configurar, registrar y administrar el ciclo de vida de los certificados para aplicaciones cliente y servidor de OPC UA en la nube. Este artículo le muestra cómo proteger aplicaciones cliente y servidor de OPC UA con un nuevo par de claves y un certificado mediante OPC Vault.

En la siguiente configuración, el cliente OPC prueba la conectividad al PLC de OPC. De forma predeterminada, la conectividad no es posible porque los dos componentes todavía no están aprovisionados con los certificados adecuados. En este flujo de trabajo, no se usan los certificados autofirmados de componentes de OPC UA, y se firman a través de OPC Vault. Consulte el [banco de pruebas](howto-opc-vault-deploy-existing-client-plc-communication.md) anterior. En su lugar, este banco de pruebas proporciona a los componentes un nuevo certificado, así como con una nueva clave privada, ambos generados por OPC Vault. Se puede encontrar algo de información preliminar sobre la seguridad de OPC UA en estas [notas del producto](https://opcfoundation.org/wp-content/uploads/2014/05/OPC-UA_Security_Model_for_Administrators_V1.00.pdf). La información completa se puede encontrar en la especificación de OPC UA.

Banco de pruebas: El entorno siguiente está configurado para pruebas.

Scripts de OPC Vault:
- Proteja las aplicaciones cliente y servidor de OPC UA con un nuevo par de claves y un certificado mediante OPC Vault.

> [!NOTE]
> Para más información, consulte el [repositorio](https://github.com/Azure-Samples/iot-edge-industrial-configs#testbeds) de GitHub.

## <a name="generate-a-new-certificate-and-private-key"></a>Generar un certificado y una clave privada nuevos 
**Preparación**
- Asegúrese de que las variables de entorno `$env:_PLC_OPT` y `$env:_CLIENT_OPT` estén sin definir. Por ejemplo, `$env:_PLC_OPT=""` en su instancia de PowerShell.
- Establezca la variable de entorno `$env:_OPCVAULTID` en una cadena que le permita buscar los datos de nuevo en OPC Vault. Se recomienda establecerla en un número de 6 dígitos. En nuestro ejemplo, se usó "123456" como valor para la variable.
- Asegúrese de que no existe ningún volumen de Docker `opcclient` ni `opcplc`. Compruébelo con `docker volume ls` y elimínelos con `docker volume rm <volumename>`. Puede que tenga que eliminar también los contenedores con `docker rm <containerid>` si el contenedor usa todavía los volúmenes.

**Guía de inicio rápido**
1. Vaya al [sitio web de OPC Vault](https://opcvault.azurewebsites.net/).

1. Seleccionar `Register New`

1. Escriba la información del PLC de OPC como se mostraba en el área `CreateSigningRequest information` de la salida del registro del banco de pruebas anterior en la página `Register New OPC UA Application` y seleccione `Server` como ApplicationType.

1. Seleccionar `Register`

1. En la página siguiente, `Request New Certificate for OPC UA Application`, seleccione `Request new KeyPair and Certificate`.

1. En la página siguiente, pegue `Generate a new Certificate with a Signing Request` en la cadena `CSR (base64 encoded)` de la salida del registro en el campo de entrada `CreateRequest`. Asegúrese de que copia la cadena completa.

1. En la página siguiente, `Request New Certificate for OPC UA Application`, seleccione `Request new Certificate with Signing Request`.

1. En la página siguiente, `Generate a new KeyPair and for an OPC UA Application`, escriba `CN=OpcPlc` como SubjectName, `opcplc-<_OPCVAULTID>` (reemplace `<_OPCVAULTID>` por el suyo) como DomainName, seleccione `PEM` como PrivateKeyFormat y escriba una contraseña (nos referimos a esta más adelante como `<certpassword-string>`).

1. Seleccionar `Generate New KeyPair`

1. Ahora vaya a `View Certificate Request Details`. En esta página, puede descargar toda la información necesaria para aprovisionar los almacenes de certificados de `opc-plc`.

1. En esta página:  
    - Seleccione `Certificate` en `Download as Base64` y copie la cadena de texto presente en el campo `EncodedBase64` y guárdela para su uso posterior. Haremos referencia a ella como `<applicationcertbase64-string>` más adelante. Seleccione `Back`.
    - Seleccione `PrivateKey` en `Download as Base64` y copie la cadena de texto presente en el campo `EncodedBase64` y guárdela para su uso posterior. Haremos referencia a ella como `<privatekeybase64-string>` más adelante. Seleccione `Back`.
    - Seleccione `Issuer` en `Download as Base64` y copie la cadena de texto presente en el campo `EncodedBase64` y guárdela para su uso posterior. Haremos referencia a ella como `<addissuercertbase64-string>` más adelante. Seleccione `Back`.
    - Seleccione `Crl` en `Download as Base64` y copie la cadena de texto presente en el campo `EncodedBase64` y guárdela para su uso posterior. Haremos referencia a ella como `<updatecrlbase64-string>` más adelante. Seleccione `Back`.

1. Ahora, establezca en su instancia de PowerShell una variable denominada `$env:_PLC_OPT`:

   `$env:_PLC_OPT="--applicationcertbase64 <applicationcertbase64-string> --privatekeybase64 <privatekeybase64-string> --certpassword <certpassword-string> --addtrustedcertbase64 <addissuercertbase64-string> --addissuercertbase64 <addissuercertbase64-string> --updatecrlbase64 <updatecrlbase64-string>"`  

    Reemplace las cadenas que se pasaron como cadenas Base64 de valores de opción que capturó en el sitio web.  

1. Repita el proceso completo empezando con `Register New` para el cliente de OPC. Solo debe tener en cuenta las siguientes diferencias:
    - Utilice la salida del registro de `opcclient`.
    - Seleccione `Client` como ApplicationType durante el registro.
    - Use `$env:_CLIENT_OPT` como nombre de la variable de PowerShell.

    > [!NOTE] 
    > Al trabajar con este escenario, puede que haya observado que los valores `<addissuercertbase64-string>` y `<updatecrlbase64-string>` son idénticos para `opcplc` y `opcclient`. Esto es cierto en nuestro caso de uso y puede ahorrarle algo de tiempo a la hora de realizar los pasos.

**Guía de inicio rápido**

Ejecute el siguiente comando de PowerShell en la raíz del repositorio:

```
docker-compose -f connecttest.yml up
```

**Comprobación**

Compruebe que los dos componentes no tienen un certificado de aplicación existente. Compruebe la salida del registro. A continuación, se muestra la salida del PLC de OPC. El cliente de OPC tiene una salida de registro similar.

```
opcplc-123456 | [13:40:08 INF] There is no existing application certificate.
```
Si hay un certificado de aplicación, quite los volúmenes de Docker, como se explica en los pasos de preparación.

Compruebe en el registro si se instaló el certificado de entidad de certificación de OPC Vault en el almacén de certificados del emisor, así como en el almacén de certificados de confianza del mismo nivel. A continuación, se muestra la salida del registro del PLC de OPC. El cliente de OPC tiene una salida de registro similar. 

```
opcplc-123456 | [13:40:09 INF] Trusted issuer store contains 1 certs
opcplc-123456 | [13:40:09 INF] 01: Subject 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' (thumbprint: BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83)
opcplc-123456 | [13:40:09 INF] Trusted issuer store has 1 CRLs.
opcplc-123456 | [13:40:09 INF] 01: Issuer 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.', Next update time '10/19/2019 22:06:46'
opcplc-123456 | [13:40:09 INF] Trusted peer store contains 1 certs
opcplc-123456 | [13:40:09 INF] 01: Subject 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' (thumbprint: BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83)
opcplc-123456 | [13:40:09 INF] Trusted peer store has 1 CRLs.
opcplc-123456 | [13:40:09 INF] 01: Issuer 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.', Next update time '10/19/2019 22:06:46'
opcplc-123456 | [13:40:09 INF] Rejected certificate store contains 0 certs
```
El PLC de OPC ahora confía en todos los clientes de OPC UA con certificados firmados por OPC Vault.

Compruebe en el registro que el formato de clave privada se reconoce como PEM y que el nuevo certificado de la aplicación está instalado. A continuación, se muestra la salida del registro del PLC de OPC. El cliente de OPC tiene una salida de registro similar. 

```
opcplc-123456 | [13:40:09 INF] The private key for the new certificate was passed in using PEM format.
opcplc-123456 | [13:40:09 INF] Remove the existing application certificate.
opcplc-123456 | [13:40:09 INF] The new application certificate 'CN=OpcPlc' and thumbprint 'A3CB288FC1D2B7A5C08AACF531CF4A85E44A6C4C' was added to the application certificate store.
opcplc-123456 | [13:40:09 INF] Activating the new application certificate with thumbprint 'A3CB288FC1D2B7A5C08AACF531CF4A85E44A6C4C'.
```

El certificado de la aplicación y la clave privada ahora están instalados en el almacén de certificados de la aplicación y se usan en la aplicación OPC UA.

Compruebe que se puede establecer la conexión entre el cliente de OPC y el PLC de OPC correctamente, y que el cliente de OPC puede leer correctamente los datos del PLC de OPC. Debería ver la salida siguiente en la salida del registro de OPC Client:
```
opcclient-123456 | [13:40:12 INF] Create secured session for endpoint URI 'opc.tcp://opcplc-123456:50000/' with timeout of 10000 ms.
opcclient-123456 | [13:40:12 INF] Session successfully created with Id ns=3;i=941910499.
opcclient-123456 | [13:40:12 INF] The session to endpoint 'opc.tcp://opcplc-123456:50000/' has 4 entries in its namespace array:
opcclient-123456 | [13:40:12 INF] Namespace index 0: http://opcfoundation.org/UA/
opcclient-123456 | [13:40:12 INF] Namespace index 1: urn:OpcPlc:opcplc-123456
opcclient-123456 | [13:40:12 INF] Namespace index 2: http://microsoft.com/Opc/OpcPlc/
opcclient-123456 | [13:40:12 INF] Namespace index 3: http://opcfoundation.org/UA/Diagnostics
opcclient-123456 | [13:40:12 INF] The server on endpoint 'opc.tcp://opcplc-123456:50000/' supports a minimal sampling interval of 0 ms.
opcclient-123456 | [13:40:12 INF] Execute 'OpcClient.OpcTestAction' action on node 'i=2258' on endpoint 'opc.tcp://opcplc-123456:50000/' with security.
opcclient-123456 | [13:40:12 INF] Action (ActionId: 000 ActionType: 'OpcTestAction', Endpoint: 'opc.tcp://opcplc-123456:50000/' Node 'i=2258') completed successfully
opcclient-123456 | [13:40:12 INF] Value (ActionId: 000 ActionType: 'OpcTestAction', Endpoint: 'opc.tcp://opcplc-123456:50000/' Node 'i=2258'): 10/21/2018 13:40:12
```
Si ve esta salida, significa que el PLC de OPC ya confía en el cliente de OPC y viceversa, ya que ambos ahora disponen de certificados firmados por una entidad de certificación en la que ambos confían.

### <a name="a-testbed-for-opc-publisher"></a>Banco de pruebas para el editor de OPC ###

**Guía de inicio rápido**

Ejecute el siguiente comando de PowerShell en la raíz del repositorio:
```
docker-compose -f testbed.yml up
```

**Comprobación**
- Compruebe que los datos se envían a la instancia de IoT Hub que configuró estableciendo `_HUB_CS` mediante el [Explorador de dispositivos](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) o [iothub-explorer](https://github.com/Azure/iothub-explorer).
- El cliente de prueba de OPC usará llamadas de método directo de IoT Hub y llamadas de método de OPC para configurar el editor de OPC para publicar o anular la publicación de nodos desde el servidor de prueba de OPC.
- Consulte la salida para ver los mensajes de error.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido cómo implementar OPC Vault en un proyecto existente, este es el siguiente paso que se le sugiere:

> [!div class="nextstepaction"]
> [Implementación de OPC Twin en un proyecto ya existente](howto-opc-twin-deploy-existing.md)