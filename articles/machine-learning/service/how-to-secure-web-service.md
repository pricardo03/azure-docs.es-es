---
title: Protección de servicios web con SSL
titleSuffix: Azure Machine Learning service
description: Aprenda cómo proteger un servicio web implementado con el servicio Azure Machine Learning. Puede restringir el acceso a los servicios web y proteger los datos enviados por los clientes mediante capas de sockets seguros (SSL) y autenticación basada en claves.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 10/02/2018
ms.custom: seodec18
ms.openlocfilehash: 14350a04326ba22dcc5c8608b6ac6b9180666832
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53101215"
---
# <a name="use-ssl-to-secure-web-services-with-azure-machine-learning-service"></a>Uso de SSL para proteger servicios web con Azure Machine Learning Service

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

> [!WARNING]
> Los certificados autofirmados solo se deben usar para desarrollo. No se deben usar en el entorno de producción. Los certificados autofirmados pueden provocar problemas en las aplicaciones cliente. Para más información, consulte la documentación de las bibliotecas de red utilizadas en la aplicación cliente.

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

## <a name="next-steps"></a>Pasos siguientes

Obtenga información en el artículo [Consume a ML Model deployed as a web service ](how-to-consume-web-service.md) (Consumir un modelo de ML implementado como servicio web).