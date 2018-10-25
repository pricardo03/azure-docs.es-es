---
title: Protección de los servicios web de Azure Machine Learning con SSL
description: Aprenda cómo proteger un servicio web implementado con el servicio Azure Machine Learning. Puede restringir el acceso a los servicios web y proteger los datos enviados por los clientes mediante capas de sockets seguros (SSL) y autenticación basada en claves.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 10/02/2018
ms.openlocfilehash: 885d867d0733ef923d327d8d6a36fc1588fd4961
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801019"
---
# <a name="secure-azure-machine-learning-web-services-with-ssl"></a>Protección de los servicios web de Azure Machine Learning con SSL

En este artículo, aprenderá a proteger un servicio web implementado con el servicio Azure Machine Learning. Puede restringir el acceso a los servicios web y proteger los datos enviados por los clientes mediante capas de sockets seguros (SSL) y autenticación basada en claves.

> [!Warning]
> Si no habilita SSL, cualquier usuario de Internet podrá hacer llamadas al servicio web.

El protocolo SSL cifra los datos que se envían entre el cliente y el servicio web. También lo usa el cliente para comprobar la identidad del servidor. La autenticación solo está habilitada para los servicios que han proporcionado un certificado SSL y una clave.  Si habilita SSL, se requiere una clave de autenticación cuando se accede al servicio web.

Tanto si implementa un servicio web habilitado con SSL como si habilita SSL para un servicio web implementado existente, los pasos son los mismos:

1. Obtenga un nombre de dominio.

2. Obtenga un certificado SSL.

3. Implemente o actualice el servicio web con la configuración de SSL habilitada.

4. Actualice el DNS para que apunte al servicio web.

Hay ligeras diferencias al proteger los servicios web mediante los [destinos de implementación](how-to-deploy-and-where.md). 

## <a name="get-a-domain-name"></a>Obtención de un nombre de dominio

Si todavía no tiene un nombre de dominio, puede comprar uno en un __registrador de nombres de dominio__. El proceso varía entre los distintos registradores, al igual que el costo. El registrador también proporciona herramientas para administrar el nombre de dominio. Estas herramientas se usan para asignar un nombre de dominio completo (como www.contoso.com) a la dirección IP que hospeda el servicio web.

## <a name="get-an-ssl-certificate"></a>Obtener un certificado SSL

Hay muchas maneras de obtener un certificado SSL. La más común es comprar uno en una __entidad de certificación__ (CA). Independientemente de dónde se obtiene el certificado, se necesitan estos archivos:

* Un __certificado__. El certificado debe incluir la cadena de certificados completa y debe estar codificado en PEM.
* Una __clave__. La clave debe estar codificada en PEM.

Cuando solicite un certificado, debe proporcionar el nombre de dominio completo (FQDN) de la dirección que pretende usar para el servicio web. Por ejemplo, www.contoso.com. La dirección que aparece en el certificado y la dirección que usan los clientes se comparan cuando se valida la identidad del servicio web. Si no coinciden, se generará un error. 

> [!TIP]
> Si la entidad de certificación no puede proporcionar el certificado y la clave como archivos codificados en PEM, puede usar una utilidad como [OpenSSL](https://www.openssl.org/) para cambiar el formato.

> [!IMPORTANT]
> Los certificados autofirmados solo se deben usar para desarrollo. No se deben usar en el entorno de producción. Si usa un certificado autofirmado, consulte la sección [Consumo de servicios web con certificados autofirmados](#self-signed) para instrucciones específicas.


## <a name="enable-ssl-and-deploy"></a>Habilitación de SSL e implementación

Para implementar (o volver a implementar) el servicio con el protocolo SSL habilitado, establezca el parámetro `ssl_enabled` en `True`, cuando proceda. Establezca el parámetro `ssl_certificate` en el valor del archivo __certificate__ y `ssl_key` en el valor del archivo __key__. 

+ **Implementación en Azure Kubernetes Service (AKS)**
  
  Al aprovisionar el clúster de AKS, proporcione valores para los parámetros relacionados con SSL, tal como se muestra en el fragmento de código:

    ```python
    from azureml.core.compute import AksCompute
    
    provisioning_config = AksCompute.provisioning_configuration(ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Implementación en Azure Container Instances (ACI)**
 
  Al implementar en ACI, proporcione valores para los parámetros relacionados con SSL, tal como se muestra en el fragmento de código:

    ```python
    from azureml.core.webservice import AciWebservice
    
    aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Implementación en matrices de puertas programables por campo (FPGA)**

  La respuesta de la operación `create_service` incluye la dirección IP del servicio. La dirección IP se usa al asignar el nombre DNS a la dirección IP del servicio. La respuesta también incluye una __clave principal__ y una __clave secundaria__ que se usan para consumir el servicio. Proporcione valores para los parámetros relacionados con SSL, tal como se muestra en el fragmento de código:

    ```python
    from amlrealtimeai import DeploymentClient
    
    subscription_id = "<Your Azure Subscription ID>"
    resource_group = "<Your Azure Resource Group Name>"
    model_management_account = "<Your AzureML Model Management Account Name>"
    location = "eastus2"
    
    model_name = "resnet50-model"
    service_name = "quickstart-service"
    
    deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account, location)
    
    with open('cert.pem','r') as cert_file:
        with open('key.pem','r') as key_file:
            cert = cert_file.read()
            key = key_file.read()
            service = deployment_client.create_service(service_name, model_id, ssl_enabled=True, ssl_certificate=cert, ssl_key=key)
    ```

## <a name="update-your-dns"></a>Actualización del DNS

Después, debe actualizar el DNS para que apunte al servicio web.

+ **Para ACI y FPGA**:  

  Use las herramientas que proporciona el registrador de nombres de dominio para actualizar el registro de DNS de su nombre de dominio. El registro debe apuntar a la dirección IP del servicio.  

  En función del registrador y del período de vida (TTL) configurado para el nombre de dominio, pueden pasar de varios minutos a varias horas antes de que los clientes puedan resolver el nombre de dominio.

+ **Para AKS**: 

  Actualice el DNS en la pestaña "Configuración" de la "Dirección IP pública" del clúster de AKS, tal como se muestra en la imagen. Puede encontrar la dirección IP pública como uno de los tipos de recurso creados bajo el grupo de recursos que contiene los nodos del agente de AKS y otros recursos de red.

  ![Azure Machine Learning Service: protección de servicios web con SSL](./media/how-to-secure-web-service/aks-public-ip-address.png)

## <a name="consume-authenticated-services"></a>Consumo de servicios autenticados

### <a name="how-to-consume"></a>Procedimiento para el consumo 
+ **Para ACI y AKS**: 

  Para los servicios web ACI y AKS, aprenda a consumir servicios web en estos artículos:
  + [Implementación en ACI](how-to-deploy-to-aci.md)

  + [Implementación en AKS](how-to-deploy-to-aks.md)

+ **Para FPGA**:  

  En los ejemplos siguientes se muestra cómo consumir un servicio FPGA autenticado con Python y C#.
  Reemplace `authkey` por la clave principal o secundaria que se devolvió al implementar el servicio.

  Ejemplo de Python:
    ```python
    from amlrealtimeai import PredictionClient
    client = PredictionClient(service.ipAddress, service.port, use_ssl=True, access_token="authKey")
    image_file = R'C:\path_to_file\image.jpg'
    results = client.score_image(image_file)
    ```

  Ejemplo de C#:
    ```csharp
    var client = new ScoringClient(host, 50051, useSSL, "authKey");
    float[,] result;
    using (var content = File.OpenRead(image))
        {
            IScoringRequest request = new ImageRequest(content);
            result = client.Score<float[,]>(request);
        }
    ```

### <a name="set-the-authorization-header"></a>Establecimiento del encabezado de autorización
Otros clientes gRPC pueden autenticar las solicitudes al establecer un encabezado de autorización. El enfoque general es crear un objeto `ChannelCredentials` que combina `SslCredentials` con `CallCredentials`. Esto se agrega al encabezado de autorización de la solicitud. Para más información sobre cómo implementar la compatibilidad con los encabezados específicos, consulte [https://grpc.io/docs/guides/auth.html](https://grpc.io/docs/guides/auth.html).

En los ejemplos siguientes se muestra cómo establecer el encabezado en C# y Go:

+ Use C# para establecer el encabezado:
    ```csharp
    creds = ChannelCredentials.Create(baseCreds, CallCredentials.FromInterceptor(
                          async (context, metadata) =>
                          {
                              metadata.Add(new Metadata.Entry("authorization", "authKey"));
                              await Task.CompletedTask;
                          }));
    
    ```

+ Use Go para establecer el encabezado:
    ```go
    conn, err := grpc.Dial(serverAddr, 
        grpc.WithTransportCredentials(credentials.NewClientTLSFromCert(nil, "")),
        grpc.WithPerRPCCredentials(&authCreds{
        Key: "authKey"}))
    
    type authCreds struct {
        Key string
    }
    
    func (c *authCreds) GetRequestMetadata(context.Context, uri ...string) (map[string]string, error) {
        return map[string]string{
            "authorization": c.Key,
        }, nil
    }
    
    func (c *authCreds) RequireTransportSecurity() bool {
        return true
    }
    ```

<a id="self-signed"></a>

## <a name="consume-services-with-self-signed-certificates"></a>Consumo de servicios con certificados autofirmados

Hay dos maneras de permitir que el cliente se autentique en un servicio protegido con un certificado autofirmado:

* En el sistema cliente, establezca la variable de entorno `GRPC_DEFAULT_SSL_ROOTS_FILE_PATH` en el sistema cliente para que apunte al archivo de certificado.

* Cuando construya un objeto `SslCredentials`, pase el contenido del archivo de certificado al constructor.

Usar cualquiera de los métodos hace que gRPC use el certificado como el certificado raíz.

> [!IMPORTANT]
> gRPC no acepta certificados que no son de confianza. Si se usa un certificado que no es de confianza, se generará un error con el código de estado `Unavailable`. Los detalles del error incluyen `Connection Failed`.
