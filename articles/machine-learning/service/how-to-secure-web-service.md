---
title: Protección de servicios web con SSL
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo proteger un servicio web implementado con el servicio de Azure Machine Learning mediante la habilitación de HTTPS. HTTPS protege los datos enviados por los clientes que usan la seguridad de capa de transporte (TLS), un sustituto para las capas de sockets seguros (SSL). También se usa por los clientes para comprobar la identidad del servicio web.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 02/05/2019
ms.custom: seodec18
ms.openlocfilehash: 1a6aa75f3d25cd88cd1edb9b2cdcfabc3b4ec8f9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60818541"
---
# <a name="use-ssl-to-secure-web-services-with-azure-machine-learning-service"></a>Uso de SSL para proteger servicios web con Azure Machine Learning Service

En este artículo, aprenderá a proteger un servicio web implementado con el servicio Azure Machine Learning. Puede restringir el acceso a servicios web y proteger los datos enviados por los clientes que usan [protocolo seguro (HTTPS)](https://en.wikipedia.org/wiki/HTTPS).

HTTPS se utiliza para proteger las comunicaciones entre un cliente y el servicio web mediante el cifrado de las comunicaciones entre los dos. El cifrado se controla mediante [capa de transporte (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). Esto es todavía conoce a veces como Secure Sockets Layer (SSL), que fue el predecesor TLS.

> [!TIP]
> El SDK de Azure Machine Learning usa el término 'SSL' para las propiedades relacionadas con la habilitación de las comunicaciones seguras. Esto no significa que no se utiliza TLS mediante el servicio web, simplemente que SSL es el término más reconocible para muchos lectores.

TLS y SSL dependen de ambas __certificados digitales__, que se usan para realizar la comprobación de identidad y cifrado. Para obtener más información sobre el trabajo de certificados digitales, vea la entrada de Wikipedia en [infraestructura de clave pública (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure).

> [!Warning]
> Si no habilita y usar HTTPS para el servicio web, los datos enviados hacia y desde el servicio pueden ser visibles a otros usuarios en internet.
>
> HTTPS también permite al cliente comprobar la autenticidad del servidor que se está conectando. Esto protege a los clientes contra [man-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) ataques.

El proceso de protección de un servicio web nuevo o uno existente es como sigue:

1. Obtenga un nombre de dominio.

2. Obtenga un certificado digital.

3. Implemente o actualice el servicio web con la configuración de SSL habilitada.

4. Actualice el DNS para que apunte al servicio web.

Hay ligeras diferencias al proteger los servicios web mediante los [destinos de implementación](how-to-deploy-and-where.md).

## <a name="get-a-domain-name"></a>Obtención de un nombre de dominio

Si todavía no tiene un nombre de dominio, puede comprar uno en un __registrador de nombres de dominio__. El proceso varía entre los distintos registradores, al igual que el costo. El registrador también proporciona herramientas para administrar el nombre de dominio. Estas herramientas se usan para asignar un nombre de dominio completo (por ejemplo, www\.contoso.com) a la dirección IP que hospeda el servicio web.

## <a name="get-an-ssl-certificate"></a>Obtener un certificado SSL

Hay muchas maneras de obtener un certificado SSL (certificado digital). La más común es comprar uno en una __entidad de certificación__ (CA). Independientemente de dónde se obtiene el certificado, se necesitan estos archivos:

* Un __certificado__. El certificado debe incluir la cadena de certificados completa y debe estar codificado en PEM.
* Una __clave__. La clave debe estar codificada en PEM.

Cuando solicite un certificado, debe proporcionar el nombre de dominio completo (FQDN) de la dirección que pretende usar para el servicio web. Por ejemplo, www\.contoso.com. La dirección que aparece en el certificado y la dirección que usan los clientes se comparan cuando se valida la identidad del servicio web. Si no coinciden, se generará un error.

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

  Al implementar en FPGA, proporcione valores para los parámetros relacionados con SSL, tal como se muestra en el fragmento de código:

    ```python
    from azureml.contrib.brainwave import BrainwaveWebservice

    deployment_config = BrainwaveWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem")
    ```

## <a name="update-your-dns"></a>Actualización del DNS

Después, debe actualizar el DNS para que apunte al servicio web.

+ **Para ACI**:

  Use las herramientas que proporciona el registrador de nombres de dominio para actualizar el registro de DNS de su nombre de dominio. El registro debe apuntar a la dirección IP del servicio.

  En función del registrador y del período de vida (TTL) configurado para el nombre de dominio, pueden pasar de varios minutos a varias horas antes de que los clientes puedan resolver el nombre de dominio.

+ **Para AKS**:

  Actualice el DNS en la pestaña "Configuración" de la "Dirección IP pública" del clúster de AKS, tal como se muestra en la imagen. Puede encontrar la dirección IP pública como uno de los tipos de recurso creados bajo el grupo de recursos que contiene los nodos del agente de AKS y otros recursos de red.

  ![Azure Machine Learning Service: protección de servicios web con SSL](./media/how-to-secure-web-service/aks-public-ip-address.png)

## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre cómo:
+ [Consumir un modelo de Machine Learning implementado como un servicio web](how-to-consume-web-service.md)
+ [Ejecución segura de experimentos e inferencias dentro de una instancia de Azure Virtual Network](how-to-enable-virtual-network.md)
