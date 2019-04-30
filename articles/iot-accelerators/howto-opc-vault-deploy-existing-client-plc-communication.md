---
title: Proteger la comunicación de cliente OPC y PLC OPC con OPC Vault - Azure | Microsoft Docs
description: Proteger la comunicación de cliente OPC y OPC PLC iniciando sesión sus certificados con la entidad emisora de certificados del almacén de OPC.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 30eedd982fa0536ce45506c159de6d04132e9a14
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61451096"
---
# <a name="secure-the-communication-of-opc-client-and-opc-plc"></a>Proteger la comunicación de cliente OPC y PLC OPC

Almacén de OPC es un microservicio que puede configurar, registrar y administrar el ciclo de vida de certificado de servidor OPC UA y las aplicaciones cliente en la nube. Este artículo muestra cómo proteger la comunicación de cliente OPC y OPC PLC iniciando sesión sus certificados con la entidad emisora de certificados del almacén de OPC.

En la siguiente configuración, el cliente OPC comprueba la conectividad con el PLC OPC. De forma predeterminada, la conectividad no es posible porque los dos componentes no se aprovisionan con los certificados de la derecha. Si todavía no se ha aprovisionado un componente de OPC UA con un certificado, generaría un certificado autofirmado en el inicio. Sin embargo, el certificado puede estar firmado por una entidad de certificación (CA) e instalado en el componente OPC UA. Cuando haya terminado de cliente OPC y OPC PLC, se habilita la conectividad. El flujo de trabajo siguiente describe el proceso. Encontrará información general sobre la seguridad de OPC UA en [este documento](https://opcfoundation.org/wp-content/uploads/2014/05/OPC-UA_Security_Model_for_Administrators_V1.00.pdf) notas del producto. Toda la información puede encontrarse en la especificación de OPC UA.

Banco de pruebas: El siguiente entorno está configurado para las pruebas.

Secuencias de comandos de OPC almacén:
- Proteger la comunicación de cliente OPC y OPC PLC iniciando sesión sus certificados con la entidad emisora de certificados del almacén de OPC.

> [!NOTE]
> Para obtener más información, vea GitHub [repositorio](https://github.com/Azure-Samples/iot-edge-industrial-configs#testbeds).

## <a name="generate-a-self-signed-certificate-on-startup"></a>Generar un certificado autofirmado en el inicio

**Preparación**

- Asegúrese de que las variables de entorno `$env:_PLC_OPT` y `$env:_CLIENT_OPT` están definidos, por ejemplo, `$env:_PLC_OPT=""` en su PowerShell.

- Establezca la variable de entorno `$env:_OPCVAULTID` en una cadena que permite buscar los datos de nuevo en el almacén de OPC. Se permiten solo caracteres alfanuméricos. En nuestro ejemplo, "123456" se usó como el valor de esta variable.

- Asegúrese de que existen volúmenes docker `opcclient` o `opcplc`. Póngase en contacto con `docker volume ls` y quitarlas con `docker volume rm <volumename>`. Es posible que deba quitar también los contenedores con `docker rm <containerid>` si todavía se usan los volúmenes por un contenedor.

**Guía de inicio rápido**

Ejecute el siguiente comando de PowerShell en la raíz del repositorio:

```
docker-compose -f connecttest.yml up
```

**Comprobación**

En el registro, compruebe que no hay ningún certificado instalado en el primer inicio. Aquí la salida del registro de OPC PLC (similar se muestra en el cliente OPC):...
```
opcplc-123456 | [20:51:32 INF] Trusted issuer store contains 0 certs
opcplc-123456 | [20:51:32 INF] Trusted issuer store has 0 CRLs.
opcplc-123456 | [20:51:32 INF] Trusted peer store contains 0 certs
opcplc-123456 | [20:51:32 INF] Trusted peer store has 0 CRLs.
opcplc-123456 | [20:51:32 INF] Rejected certificate store contains 0 certs
```
Si ve los certificados notificados, siga los pasos de preparación anteriores y elimine los volúmenes de docker.

Compruebe que ha fallado la conexión con el PLC OPC. Debería ver el siguiente resultado en la salida del registro de cliente OPC:

```
opcclient-123456 | [20:51:35 INF] Create secured session for endpoint URI 'opc.tcp://opcplc-123456:50000/' with timeout of 10000 ms.
opcclient-123456 | [20:51:36 ERR] Session creation to endpoint 'opc.tcp://opcplc-123456:50000/' failed 1 time(s). Please verify if server is up and OpcClient configuration is correct.
opcclient-123456 | Opc.Ua.ServiceResultException: Certificate is not trusted.
```
El motivo del error es que el certificado no es de confianza. Esto significa que `opc-client` intentó conectarse a `opc-plc` y recibido una respuesta, lo que indica que `opc-plc` no confía en `opc-client`, un resultado de `opc-plc` no se puede validar el certificado que `opc-client` ha proporcionado. Este es un certificado autofirmado con ninguna otra configuración de certificado en `opc-plc`, y se produjo un error en la conexión.

## <a name="sign-and-install-certificates-in-opc-ua-components"></a>Iniciar sesión e instalar certificados en los componentes OPC UA

**Preparación**
1. Examine la salida del registro del paso 1 y capturar "CreateSigningRequest información" para el PLC OPC y el cliente OPC. Aquí solo se muestra el resultado de OPC PLC:

    ```
    opcplc-123456 | [20:51:32 INF] ----------------------- CreateSigningRequest information ------------------
    opcplc-123456 | [20:51:32 INF] ApplicationUri: urn:OpcPlc:opcplc-123456
    opcplc-123456 | [20:51:32 INF] ApplicationName: OpcPlc
    opcplc-123456 | [20:51:32 INF] ApplicationType: Server
    opcplc-123456 | [20:51:32 INF] ProductUri: https://github.com/azure-samples/iot-edge-opc-plc
    opcplc-123456 | [20:51:32 INF] DiscoveryUrl[0]: opc.tcp://opcplc-123456:50000
    opcplc-123456 | [20:51:32 INF] ServerCapabilities: DA
    opcplc-123456 | [20:51:32 INF] CSR (base64 encoded):
    opcplc-123456 | MIICmzCCAYMCAQAwETEPMA0GA1UEAwwGT3BjUGxjMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAwTvlbinAPWPxR9Lw1ndGsrLGy8GiqVOxyGaDpPUcMchX0k0/ncg28h7xrB2H1PThdiZxUJuUwsNM74HrVgt
    ofmXhA4dLM1cTxZHyJVFjl2L3vK5M58NNf6UNdKcB0x3LyuoT6mAIMXmCioqymFCk1TMzIMzbAe7JVAdUaSRBP1vuqQ1rV/cfNAe35dKQW4aPYgl7pR5f1hqprcDu/oca67X8L4kTl4oN0/bCYTk+Ibcd9cG462oAN+bSwbHn8a2jNky8rGsofA
    o9DOT+0ALPhk6CApCYIP2yxoI/kT188eqUUxzAFF9nyU79AyCkpGPuY8DGMyf56pDofgtGpfY3wQIDAQABoEUwQwYJKoZIhvcNAQkOMTYwNDAyBgNVHREEKzAphhh1cm46T3BjUGxjOm9wY3BsYy0xMjM0NTaCDW9wY3BsYy0xMjM0NTYwDQYJK
    oZIhvcNAQELBQADggEBAAsZLoOLzS2VhDcQRu0QhRbG7CGAxX19l7fDCG2WjU7lTFnCvYVTWTYyaY61ljmrWc7IbCaQdMJM8GRnAnvAzUh/PBDxkOX7NqI2+8F1yQOHgs/AfKuppOd6DIP8EzFAHnc0H85jay6zFdmIDWoWwpy0ACqOVooOTKST
    7uty0mT87bj8Cdy1yf4mvBNQx+nsuTbKgxWCBxGYAyg9dIL2uKL0aeB/ROW5Gkelz5sCEzQ1fFDokUA4oC5QiATQBN3cY7EmvRbPgdToY7CpRN3iiO7J+7bC7BP9YKfuE34E8xOFpskHPHAPf3r002/L0S67HyuVSXLUj1+Jc0LeAAF9Bw0=
    opcplc-123456 | [20:51:32 INF] ---------------------------------------------------------------------------
    ```
    
1. Vaya a la [sitio Web de almacén de OPC](https://opcvault.azurewebsites.net/).

1. Seleccionar `Register New`

1. Escriba la información de OPC PLC de las salidas de registro `CreateSigningRequest information` área en los campos de entrada en el `Register New OPC UA Application` página, seleccione `Server` como ApplicationType.

1. Seleccione `Register`.

1. En la página siguiente, `Request New Certificate for OPC UA Application` seleccione `Request new Certificate with Signing Request`.

1. En la página siguiente, `Generate a new Certificate with a Signing Request` pegue en el `CSR (base64 encoded)` cadena a partir de la salida del registro en el `CreateRequest` campo de entrada. Asegúrese de que copiar la cadena completa.

1. Seleccione `Generate New Certificate`.

1. Ahora está moviendo hacia delante a `View Certificate Request Details`. En esta página, puede descargar toda la información necesaria para aprovisionar los almacenes de certificados de `opc-plc`.

1. En esta página:  
    - Seleccione `Certificate` en `Download as Base64` y copie la cadena de texto se presenta en el `EncodedBase64` campo y lo almacenará para su uso posterior. Nos referimos a ellos como `<applicationcertbase64-string>` más adelante. Seleccione `Back`.

    - Seleccione `Issuer` en `Download as Base64` y copie la cadena de texto se presenta en el `EncodedBase64` campo y lo almacenará para su uso posterior. Nos referimos a ellos como `<addissuercertbase64-string>` más adelante. Seleccione `Back`.

    - Seleccione `Crl` en `Download as Base64` y copie la cadena de texto se presenta en el `EncodedBase64` campo y lo almacenará para su uso posterior. Nos referimos a ellos como `<updatecrlbase64-string>` más adelante. Seleccione `Back`.

1. Establecer ahora en su PowerShell una variable denominada `$env:_PLC_OPT`:

    ```
    `$env:_PLC_OPT="--applicationcertbase64 <applicationcertbase64-string> --addtrustedcertbase64 <addissuercertbase64-string> --addissuercertbase64 <addissuercertbase64-string> --updatecrlbase64 <updatecrlbase64-string>"`  
    ```
    > [!NOTE] 
    > Reemplace las cadenas que se pasan como cadenas de Base64 de los valores de opción que recuperó los cambios desde el sitio Web.

Repita el proceso completo a partir de `Register New` (paso 3 anterior) para el cliente OPC. Hay solo las siguientes diferencias que debe tener en cuenta:

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

**Comprobación** Compruebe que los dos componentes ahora han suscrito a certificados de la aplicación. Compruebe la salida del registro para la salida siguiente:

```
opcplc-123456 | [20:54:38 INF] Starting to add certificate(s) to the trusted issuer store.
opcplc-123456 | [20:54:38 INF] Certificate 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' and thumbprint 'BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83' was added to the trusted issuer store.
opcplc-123456 | [20:54:38 INF] Starting to add certificate(s) to the trusted peer store.
opcplc-123456 | [20:54:38 INF] Certificate 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' and thumbprint 'BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83' was added to the trusted peer store.
opcplc-123456 | [20:54:38 INF] Starting to update the current CRL.
opcplc-123456 | [20:54:38 INF] Remove the current CRL from the trusted peer store.
opcplc-123456 | [20:54:38 INF] The new CRL issued by 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' was added to the trusted peer store.
opcplc-123456 | [20:54:38 INF] Remove the current CRL from the trusted issuer store.
opcplc-123456 | [20:54:38 INF] The new CRL issued by 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' was added to the trusted issuer store.
opcplc-123456 | [20:54:38 INF] Start updating the current application certificate.
opcplc-123456 | [20:54:38 INF] The current application certificate has SubjectName 'CN=OpcPlc' and thumbprint '8FD43F66479398BDA3AAF5B193199A6657632B49'.
opcplc-123456 | [20:54:39 INF] Remove the existing application certificate with thumbprint '8FD43F66479398BDA3AAF5B193199A6657632B49'.
opcplc-123456 | [20:54:39 INF] The new application certificate 'CN=OpcPlc' and thumbprint 'DA6B8B2FB533FBC188F7017BAA8A36FDB77E2586' was added to the application certificate store.
opcplc-123456 | [20:54:39 INF] Activating the new application certificate with thumbprint 'DA6B8B2FB533FBC188F7017BAA8A36FDB77E2586'.
opcplc-123456 | [20:54:39 INF] Application certificate with thumbprint 'DA6B8B2FB533FBC188F7017BAA8A36FDB77E2586' found in the application certificate store.
opcplc-123456 | [20:54:39 INF] Application certificate is for ApplicationUri 'urn:OpcPlc:opcplc-123456', ApplicationName 'OpcPlc' and Subject is 'OpcPlc'
 ```
El certificado de la aplicación está allí y firmado por una entidad de certificación.

En el registro, compruebe que ahora hay certificados instalados. A continuación es la salida del registro de OPC PLC y cliente OPC tiene una salida similar.
```
opcplc-123456 | [20:54:39 INF] Trusted issuer store contains 1 certs
opcplc-123456 | [20:54:39 INF] 01: Subject 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' (thumbprint: BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83)
opcplc-123456 | [20:54:39 INF] Trusted issuer store has 1 CRLs.
opcplc-123456 | [20:54:39 INF] 01: Issuer 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.', Next update time '10/19/2019 22:06:46'
opcplc-123456 | [20:54:39 INF] Trusted peer store contains 1 certs
opcplc-123456 | [20:54:39 INF] 01: Subject 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' (thumbprint: BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83)
opcplc-123456 | [20:54:39 INF] Trusted peer store has 1 CRLs.
opcplc-123456 | [20:54:39 INF] 01: Issuer 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.', Next update time '10/19/2019 22:06:46'
opcplc-123456 | [20:54:39 INF] Rejected certificate store contains 0 certs
```
El emisor del certificado de aplicación es la entidad de certificación `CN=Azure IoT OPC Vault CA, O=Microsoft Corp.` y confiar en el PLC OPC también todos los certificados firmados por esta CA.


Compruebe que se ha creado correctamente la conexión con el PLC OPC y el cliente OPC puede leer datos de OPC PLC. Debería ver el siguiente resultado en la salida del registro de cliente OPC:
```
opcclient-123456 | [20:54:42 INF] Create secured session for endpoint URI 'opc.tcp://opcplc-123456:50000/' with timeout of 10000 ms.
opcclient-123456 | [20:54:42 INF] Session successfully created with Id ns=3;i=1085867946.
opcclient-123456 | [20:54:42 INF] The session to endpoint 'opc.tcp://opcplc-123456:50000/' has 4 entries in its namespace array:
opcclient-123456 | [20:54:42 INF] Namespace index 0: http://opcfoundation.org/UA/
opcclient-123456 | [20:54:42 INF] Namespace index 1: urn:OpcPlc:opcplc-123456
opcclient-123456 | [20:54:42 INF] Namespace index 2: http://microsoft.com/Opc/OpcPlc/
opcclient-123456 | [20:54:42 INF] Namespace index 3: http://opcfoundation.org/UA/Diagnostics
opcclient-123456 | [20:54:42 INF] The server on endpoint 'opc.tcp://opcplc-123456:50000/' supports a minimal sampling interval of 0 ms.
opcclient-123456 | [20:54:42 INF] Execute 'OpcClient.OpcTestAction' action on node 'i=2258' on endpoint 'opc.tcp://opcplc-123456:50000/' with security.
opcclient-123456 | [20:54:42 INF] Action (ActionId: 000 ActionType: 'OpcTestAction', Endpoint: 'opc.tcp://opcplc-123456:50000/' Node 'i=2258') completed successfully
opcclient-123456 | [20:54:42 INF] Value (ActionId: 000 ActionType: 'OpcTestAction', Endpoint: 'opc.tcp://opcplc-123456:50000/' Node 'i=2258'): 10/20/2018 20:54:42
```
Si ve esta salida, a continuación, el PLC OPC es ahora confiar en OPC cliente y viceversa, ya que ambos tienen ahora los certificados firmados por una entidad de certificación y confianza de certificados que where firmado por esta entidad de certificación.

> [!NOTE] 
> Aunque sólo para OPC PLC, mostramos los primeros pasos de comprobación de dos, los deben comprobarse también para el cliente OPC.


## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido cómo implementar OPC almacén a un proyecto existente, aquí es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Implementar a OPC gemelo a un proyecto existente](howto-opc-twin-deploy-existing.md)