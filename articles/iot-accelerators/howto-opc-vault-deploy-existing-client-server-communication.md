---
title: Proteger OPC UA de cliente y la aplicación de servidor de OPC UA con el almacén de OPC - Azure | Microsoft Docs
description: Cliente OPC UA y OPC UA server aplicación seguro con un nuevo par de claves y un certificado con el almacén de OPC.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 5ba2dba02585598b3797dd1b490976ebe34b489e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61450671"
---
# <a name="secure-opc-ua-client-and-opc-ua-server-application"></a>Proteger OPC UA de cliente y la aplicación de servidor de OPC UA 
OPC Vault es un microservicio que puede configurar, registrar y administrar el ciclo de vida de los certificados para aplicaciones cliente y servidor de OPC UA en la nube. Este artículo muestra cómo proteger un cliente OPC UA y un OPC UA de aplicación de servidor con un nuevo par de claves y un certificado con el almacén de OPC.

En la siguiente configuración, el cliente OPC está probando la conectividad con el PLC OPC. De forma predeterminada, la conectividad no es posible porque todavía no se ha aprovisionado ambos componentes con los certificados de la derecha. En este flujo de trabajo, no use los certificados autofirmados de componentes de OPC UA y firmarlos a través de OPC Vault. Vea la anterior [banco de pruebas](howto-opc-vault-deploy-existing-client-plc-communication.md). En su lugar, este banco de pruebas proporciona los componentes con un nuevo certificado, así como con una nueva clave privada que se generan en el almacén de OPC. Información general sobre la seguridad de OPC UA puede encontrarse en este [notas del producto](https://opcfoundation.org/wp-content/uploads/2014/05/OPC-UA_Security_Model_for_Administrators_V1.00.pdf). Toda la información puede encontrarse en la especificación de OPC UA.

Banco de pruebas: El siguiente entorno está configurado para las pruebas.

Secuencias de comandos de OPC almacén:
- Cliente OPC UA y OPC UA server aplicaciones seguras con un nuevo par de claves y un certificado con el almacén de OPC.

> [!NOTE]
> Para obtener más información, vea GitHub [repositorio](https://github.com/Azure-Samples/iot-edge-industrial-configs#testbeds).

## <a name="generate-a-new-certificate-and-private-key"></a>Generar un nuevo certificado y clave privada 
**Preparación**
- Asegúrese de que las variables de entorno `$env:_PLC_OPT` y `$env:_CLIENT_OPT` son indefinidos. Por ejemplo, `$env:_PLC_OPT=""` en su PowerShell
- Establezca la variable de entorno `$env:_OPCVAULTID` en una cadena que permite buscar los datos de nuevo en el almacén de OPC. Se recomienda si se establece en un número de 6 dígitos. En nuestro ejemplo, "123456" se utiliza como valor para la variable.
- Asegúrese de que no hay ningún volumen de docker `opcclient` o `opcplc`. Póngase en contacto con `docker volume ls` y quitarlas con `docker volume rm <volumename>`. Es posible que deba quitar también los contenedores con `docker rm <containerid>` si todavía se usan los volúmenes por un contenedor.

**Guía de inicio rápido**
1. Vaya a la [sitio Web de almacén de OPC](https://opcvault.azurewebsites.net/)

1. Seleccionar `Register New`

1. Escriba la información de OPC PLC, como se muestra en la salida del registro del banco de pruebas anterior `CreateSigningRequest information` área en los campos de entrada en el `Register New OPC UA Application` página, seleccione `Server` como ApplicationType.

1. Seleccionar `Register`

1. En la página siguiente, `Request New Certificate for OPC UA Application` seleccione `Request new KeyPair and Certificate`

1. En la página siguiente, `Generate a new Certificate with a Signing Request` pegue en el `CSR (base64 encoded)` cadena a partir de la salida del registro en el `CreateRequest` campo de entrada. Asegúrese de que copiar la cadena completa.

1. En la página siguiente, `Request New Certificate for OPC UA Application` seleccione `Request new Certificate with Signing Request`

1. En la página siguiente `Generate a new KeyPair and for an OPC UA Application` escriba `CN=OpcPlc` como SubjectName, `opcplc-<_OPCVAULTID>` (reemplace `<_OPCVAULTID>` con el suyo) como nombre de dominio, seleccione `PEM` como PrivateKeyFormat y escriba una contraseña (nos referimos más adelante a ella como `<certpassword-string>`)

1. Seleccionar `Generate New KeyPair`

1. Ahora está moviendo hacia delante a `View Certificate Request Details`. En esta página, puede descargar toda la información necesaria para aprovisionar los almacenes de certificados de `opc-plc`.

1. En esta página:  
    - Seleccione `Certificate` en `Download as Base64` y copie la cadena de texto se presenta en el `EncodedBase64` campo y lo almacenará para su uso posterior. Nos referimos a ellos como `<applicationcertbase64-string>` más adelante. Seleccione `Back`.
    - Seleccione `PrivateKey` en `Download as Base64` y copie la cadena de texto se presenta en el `EncodedBase64` campo y lo almacenará para su uso posterior. Nos referimos a ellos como `<privatekeybase64-string>` más adelante. Seleccione `Back`.
    - Seleccione `Issuer` en `Download as Base64` y copie la cadena de texto se presenta en el `EncodedBase64` campo y lo almacenará para su uso posterior. Nos referimos a ellos como `<addissuercertbase64-string>` más adelante. Seleccione `Back`.
    - Seleccione `Crl` en `Download as Base64` y copie la cadena de texto se presenta en el `EncodedBase64` campo y lo almacenará para su uso posterior. Nos referimos a ellos como `<updatecrlbase64-string>` más adelante. Seleccione `Back`.

1. Establecer ahora en su PowerShell una variable denominada `$env:_PLC_OPT`:

   `$env:_PLC_OPT="--applicationcertbase64 <applicationcertbase64-string> --privatekeybase64 <privatekeybase64-string> --certpassword <certpassword-string> --addtrustedcertbase64 <addissuercertbase64-string> --addissuercertbase64 <addissuercertbase64-string> --updatecrlbase64 <updatecrlbase64-string>"`  

    Reemplace las cadenas que se pasan como cadenas de Base64 de los valores de opción que recuperó los cambios desde el sitio Web.  

1. Repita el proceso completo a partir de `Register New` para el cliente OPC. Hay solo las siguientes diferencias que debe tener en cuenta:
    - Utilice la salida del registro el `opcclient`.
    - Seleccione `Client` como ApplicationType durante el registro.
    - Use `$env:_CLIENT_OPT` como nombre de la variable de PowerShell.

    > [!NOTE] 
    > Al trabajar con este escenario, quizás haya reconocido que la `<addissuercertbase64-string>` y `<updatecrlbase64-string>` valores son idénticos para `opcplc` y `opcclient`. Esto es cierto en nuestro caso de uso y puede ahorrarle tiempo mientras se lleva a cabo los pasos.

**Guía de inicio rápido**

Ejecute el siguiente comando de PowerShell en la raíz del repositorio:

```
docker-compose -f connecttest.yml up
```

**Comprobación**

Compruebe que los dos componentes no han tenido un certificado existente de la aplicación. Compruebe la salida del registro. A continuación es el resultado de OPC PLC y cliente OPC tiene una salida de registro similar.

```
opcplc-123456 | [13:40:08 INF] There is no existing application certificate.
```
Si hay un certificado de aplicación, quite los volúmenes de docker, como se explica en los pasos de preparación.

Compruebe en el registro que se instaló el certificado de CA del almacén de OPC en el almacén de certificados de emisor, así como en el almacén de certificados de confianza del mismo nivel. A continuación es la salida del registro de OPC PLC y cliente de OPC tiene una salida de registro similar. 

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
Ahora el PLC OPC confiar en todos los clientes de OPC UA con certificados firmados por el almacén de OPC.

Compruebe en el registro que se reconoce el formato de clave privada como PEM y que está instalado el nuevo certificado de aplicación. A continuación es la salida del registro de OPC PLC y cliente OPC tiene una salida de registro similar. 

```
opcplc-123456 | [13:40:09 INF] The private key for the new certificate was passed in using PEM format.
opcplc-123456 | [13:40:09 INF] Remove the existing application certificate.
opcplc-123456 | [13:40:09 INF] The new application certificate 'CN=OpcPlc' and thumbprint 'A3CB288FC1D2B7A5C08AACF531CF4A85E44A6C4C' was added to the application certificate store.
opcplc-123456 | [13:40:09 INF] Activating the new application certificate with thumbprint 'A3CB288FC1D2B7A5C08AACF531CF4A85E44A6C4C'.
```

El certificado de la aplicación y la clave privada están instalados en el almacén de certificados de la aplicación y utilizados por la aplicación de OPC UA.

Compruebe que se puede establecer la conexión entre el cliente OPC y OPC PLC correctamente y el cliente OPC puede leer correctamente los datos de OPC PLC. Debería ver el siguiente resultado en la salida del registro de cliente OPC:
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
Si ve esta salida, a continuación, el PLC OPC ahora confía en que el OPC cliente y viceversa, dado que ambas tienen ahora los certificados firmados por una entidad de certificación y ambos certificados de confianza que se han firmado por esta CA.

### <a name="a-testbed-for-opc-publisher"></a>Un banco de pruebas para el publicador de OPC ###

**Guía de inicio rápido**

Ejecute el siguiente comando de PowerShell en la raíz del repositorio:
```
docker-compose -f testbed.yml up
```

**Comprobación**
- Compruebe que los datos se envían al centro de IOT configura estableciendo `_HUB_CS` mediante [Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) o [iothub-explorer](https://github.com/Azure/iothub-explorer).
- Cliente de prueba OPC se va a usar las llamadas de método directo de IoTHub y llamadas a métodos OPC para configurar el publicador de OPC para publicar o cancelar la publicación nodos desde el servidor de prueba OPC.
- Ver la salida de mensajes de error.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido cómo implementar OPC almacén a un proyecto existente, aquí es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Implementar a OPC gemelo a un proyecto existente](howto-opc-twin-deploy-existing.md)