---
title: Protección de la comunicación entre OPC Client y OPC PLC con OPC Vault en Azure | Microsoft Docs
description: Proteja la comunicación entre OPC Client y OPC PLC mediante la firma de sus certificados con la entidad de certificación de OPC Vault.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 30eedd982fa0536ce45506c159de6d04132e9a14
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61451096"
---
# <a name="secure-the-communication-of-opc-client-and-opc-plc"></a>Protección de la comunicación entre OPC Client y OPC PLC

OPC Vault es un microservicio que puede configurar, registrar y administrar el ciclo de vida de los certificados para aplicaciones cliente y servidor de OPC UA en la nube. En este artículo se muestra cómo proteger la comunicación entre OPC Client y OPC PLC mediante la firma de sus certificados con la entidad de certificación de OPC Vault.

En la siguiente configuración, OPC Client prueba la conectividad a OPC PLC. De forma predeterminada, la conectividad no es posible porque los dos componentes no están aprovisionados con los certificados adecuados. Si todavía no se ha aprovisionado un componente de OPC UA con un certificado, se generaría un certificado autofirmado en el inicio. No obstante, una entidad de certificación (CA) puede firmar el certificado y este se puede instalar en el componente de OPC UA. Una vez hecho esto para OPC Client y OPC PLC, la conectividad estará habilitada. El siguiente flujo de trabajo describe el proceso. Se puede encontrar algo de información preliminar sobre la seguridad de OPC UA en [este documento](https://opcfoundation.org/wp-content/uploads/2014/05/OPC-UA_Security_Model_for_Administrators_V1.00.pdf) con notas del producto. La información completa se puede encontrar en la especificación de OPC UA.

Banco de pruebas: El entorno siguiente está configurado para pruebas.

Scripts de OPC Vault:
- Proteja la comunicación entre OPC Client y OPC PLC mediante la firma de sus certificados con la entidad de certificación de OPC Vault.

> [!NOTE]
> Para más información, consulte el [repositorio](https://github.com/Azure-Samples/iot-edge-industrial-configs#testbeds) de GitHub.

## <a name="generate-a-self-signed-certificate-on-startup"></a>Generación de un certificado autofirmado en el inicio

**Preparación**

- Asegúrese de que las variables de entorno `$env:_PLC_OPT` y `$env:_CLIENT_OPT` estén sin definir, por ejemplo, `$env:_PLC_OPT=""` en su PowerShell.

- Establezca la variable de entorno `$env:_OPCVAULTID` en una cadena que le permita buscar los datos de nuevo en OPC Vault. Solo puede contener caracteres alfanuméricos. En nuestro ejemplo, se usó "123456" como valor para esta variable.

- Asegúrese de que no existen volúmenes Docker `opcclient` o `opcplc`. Compruébelo con `docker volume ls` y elimínelos con `docker volume rm <volumename>`. Puede que tenga que eliminar también los contenedores con `docker rm <containerid>` si el contenedor usa todavía los volúmenes.

**Guía de inicio rápido**

Ejecute el siguiente comando de PowerShell en la raíz del repositorio:

```
docker-compose -f connecttest.yml up
```

**Comprobación**

En el registro, compruebe que no hay ningún certificado instalado en el primer inicio. Esta es la salida del registro de OPC PLC (la de OPC Client es similar):
```
opcplc-123456 | [20:51:32 INF] Trusted issuer store contains 0 certs
opcplc-123456 | [20:51:32 INF] Trusted issuer store has 0 CRLs.
opcplc-123456 | [20:51:32 INF] Trusted peer store contains 0 certs
opcplc-123456 | [20:51:32 INF] Trusted peer store has 0 CRLs.
opcplc-123456 | [20:51:32 INF] Rejected certificate store contains 0 certs
```
Si ve los certificados notificados, siga los pasos de preparación anteriores y elimine los volúmenes de Docker.

Compruebe que ha habido un error con la conexión con OPC PLC. Debería ver la salida siguiente en la salida del registro de OPC Client:

```
opcclient-123456 | [20:51:35 INF] Create secured session for endpoint URI 'opc.tcp://opcplc-123456:50000/' with timeout of 10000 ms.
opcclient-123456 | [20:51:36 ERR] Session creation to endpoint 'opc.tcp://opcplc-123456:50000/' failed 1 time(s). Please verify if server is up and OpcClient configuration is correct.
opcclient-123456 | Opc.Ua.ServiceResultException: Certificate is not trusted.
```
El motivo del error es que el certificado no es de confianza. Esto significa que `opc-client` intentó conectarse a `opc-plc` y recibió una respuesta que indica que `opc-plc` no confía en `opc-client`. Esto se debe a que `opc-plc` no puede validar el certificado que `opc-client` ha proporcionado. Este es un certificado autofirmado sin ninguna otra configuración de certificado en `opc-plc` por lo que se produjo un error en la conexión.

## <a name="sign-and-install-certificates-in-opc-ua-components"></a>Inicio de sesión e instalación de certificados en los componentes de OPC UA

**Preparación**
1. Examine la salida del registro del paso 1 y capture "CreateSigningRequest information" para OPC PLC y OPC Client. Aquí se muestra solo la salida para OPC PLC:

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
    
1. Vaya al [sitio web de OPC Vault](https://opcvault.azurewebsites.net/).

1. Seleccionar `Register New`

1. Escriba la información de OPC PLC del área `CreateSigningRequest information` de las salidas del registro en los campos de entrada de la página `Register New OPC UA Application` y seleccione `Server` como ApplicationType.

1. Seleccione `Register`.

1. En la página siguiente, `Request New Certificate for OPC UA Application` seleccione `Request new Certificate with Signing Request`.

1. En la página siguiente, pegue `Generate a new Certificate with a Signing Request` en la cadena `CSR (base64 encoded)` de la salida del registro en el campo de entrada `CreateRequest`. Asegúrese de que copia la cadena completa.

1. Seleccione `Generate New Certificate`.

1. Ahora vaya a `View Certificate Request Details`. En esta página, puede descargar toda la información necesaria para aprovisionar los almacenes de certificados de `opc-plc`.

1. En esta página:  
    - Seleccione `Certificate` en `Download as Base64` y copie la cadena de texto presente en el campo `EncodedBase64` y guárdela para su uso posterior. Haremos referencia a ella como `<applicationcertbase64-string>` más adelante. Seleccione `Back`.

    - Seleccione `Issuer` en `Download as Base64` y copie la cadena de texto presente en el campo `EncodedBase64` y guárdela para su uso posterior. Haremos referencia a ella como `<addissuercertbase64-string>` más adelante. Seleccione `Back`.

    - Seleccione `Crl` en `Download as Base64` y copie la cadena de texto presente en el campo `EncodedBase64` y guárdela para su uso posterior. Haremos referencia a ella como `<updatecrlbase64-string>` más adelante. Seleccione `Back`.

1. Ahora, establezca en su instancia de PowerShell una variable denominada `$env:_PLC_OPT`:

    ```
    `$env:_PLC_OPT="--applicationcertbase64 <applicationcertbase64-string> --addtrustedcertbase64 <addissuercertbase64-string> --addissuercertbase64 <addissuercertbase64-string> --updatecrlbase64 <updatecrlbase64-string>"`  
    ```
    > [!NOTE] 
    > Reemplace las cadenas que se pasaron como cadenas Base64 de valores de opción que capturó en el sitio web.

Repita el proceso completo empezando con `Register New` (paso 3 anterior) para OPC Client. Solo debe tener en cuenta las siguientes diferencias:

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

**Comprobación** Compruebe que los dos componentes tienen ya certificados de aplicación firmados. Compruebe la salida del registro para la salida siguiente:

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
El certificado de la aplicación está allí y está firmado por una entidad de certificación.

En el registro, compruebe que ahora ya hay certificados instalados. A continuación, se muestra la salida del registro de OPC PLC. La de OPC Client es similar.
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
El emisor del certificado de aplicación es la entidad de certificación `CN=Azure IoT OPC Vault CA, O=Microsoft Corp.` y OPC PLC confía también en todos los certificados firmados por esta entidad de certificación.


Compruebe que se ha creado correctamente la conexión con OPC PLC y que OPC Client puede leer datos de OPC PLC. Debería ver la salida siguiente en la salida del registro de OPC Client:
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
Si ve esta salida, significa que OPC PLC confía ya en OPC Client y viceversa, ya que ambos disponen ahora de certificados firmados por una entidad de certificación en la que ambos confían.

> [!NOTE] 
> Aunque solo hemos mostrado los dos primeros pasos de comprobación para OPC PLC, estos también se deben comprobar para OPC Client.


## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido cómo implementar OPC Vault en un proyecto existente, este es el siguiente paso que se le sugiere:

> [!div class="nextstepaction"]
> [Implementación de OPC Twin en un proyecto ya existente](howto-opc-twin-deploy-existing.md)