---
title: Proteger los servicios web mediante el uso de SSL
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo proteger un servicio web que se implementa a través del servicio de Azure Machine Learning mediante la habilitación de HTTPS. HTTPS protege los datos de los clientes mediante el uso de seguridad de capa de transporte (TLS), un sustituto para las capas de sockets seguros (SSL). Los clientes también usan HTTPS para comprobar la identidad del servicio web.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: 779a34800057284ce77b6d76e030ddca5fadc25f
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393822"
---
# <a name="use-ssl-to-secure-a-web-service-through-azure-machine-learning"></a>Usar SSL para proteger un servicio web a través de Azure Machine Learning

Este artículo muestra cómo proteger un servicio web que se implementa a través del servicio de Azure Machine Learning.

Usa [HTTPS](https://en.wikipedia.org/wiki/HTTPS) para restringir el acceso a servicios web y proteger los datos que los clientes envíen. HTTPS le ayuda a proteger las comunicaciones entre un cliente y un servicio web mediante el cifrado de las comunicaciones entre los dos. El cifrado usa [capa de transporte (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security). TLS se conoce a veces todavía como *Secure Sockets Layer* (SSL), que fue el predecesor de TLS.

> [!TIP]
> El SDK de Azure Machine Learning usa el término "SSL" para las propiedades que están relacionados con las comunicaciones seguras. Esto no significa que no usa el servicio web *TLS*. SSL es simplemente un término más comúnmente reconocido.

TLS y SSL dependen de ambas *certificados digitales*, que ayuda con la comprobación de identidad y cifrado. Para obtener más información sobre el trabajo de certificados digitales, vea el tema de Wikipedia [infraestructura de clave pública](https://en.wikipedia.org/wiki/Public_key_infrastructure).

> [!WARNING]
> Si no usa HTTPS para el servicio web, los datos que se envían hacia y desde el servicio podrían ser visibles para otros usuarios en internet.
>
> HTTPS también permite al cliente comprobar la autenticidad del servidor que se está conectando. Esta característica protege a los clientes contra [man-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) ataques.

Este es el proceso general para proteger un servicio web:

1. Obtenga un nombre de dominio.

2. Obtenga un certificado digital.

3. Implementar o actualizar el servicio web con SSL habilitado.

4. Actualice el DNS para que apunte al servicio web.

> [!IMPORTANT]
> Si va a implementar en Azure Kubernetes Service (AKS), puede adquirir su propio certificado o usar un certificado proporcionado por Microsoft. Si usa un certificado de Microsoft, no es necesario obtener un nombre de dominio o un certificado SSL. Para obtener más información, consulte el [habilitar SSL e implementar](#enable) sección de este artículo.

Hay ligeras diferencias cuando proteja los servicios web a través de [destinos de implementación](how-to-deploy-and-where.md).

## <a name="get-a-domain-name"></a>Obtención de un nombre de dominio

Si aún no tiene un nombre de dominio, comprar uno en un *registrador de nombres de dominio*. El proceso y los precios difieren entre los registradores. El registrador proporciona herramientas para administrar el nombre de dominio. Use estas herramientas para asignar un nombre de dominio completo (FQDN) (por ejemplo, www\.contoso.com) a la dirección IP que hospeda el servicio web.

## <a name="get-an-ssl-certificate"></a>Obtener un certificado SSL

Hay muchas maneras de obtener un certificado SSL (certificado digital). La más común es comprar uno en un *certificación* (CA). Independientemente de dónde obtener el certificado, necesita los siguientes archivos:

* Un **certificado**. El certificado debe contener la cadena de certificados completa, y debe ser "PEM codificado."
* Una **clave**. La clave también debe ser PEM codificado.

Cuando se solicita un certificado, debe proporcionar el FQDN de la dirección que se va a usar para el servicio web (por ejemplo, www\.contoso.com). La dirección que se marca en el certificado y la dirección que usan los clientes se comparan para comprobar la identidad del servicio web. Si esas direcciones no coinciden, el cliente obtiene un mensaje de error.

> [!TIP]
> Si la entidad de certificación no puede proporcionar el certificado y clave como archivos codificados en PEM, puede usar una utilidad como [OpenSSL](https://www.openssl.org/) para cambiar el formato.

> [!WARNING]
> Use *autofirmados* certificados para el desarrollo. No se utilizan en entornos de producción. Los certificados autofirmados pueden provocar problemas en las aplicaciones cliente. Para obtener más información, consulte la documentación para las bibliotecas de red que usa la aplicación cliente.

## <a id="enable"></a> Habilitar SSL e implementar

Para implementar el servicio con SSL habilitado (o volver a implementar), establezca el *ssl_enabled* parámetro en "True" siempre que sea aplicable. Establecer el *ssl_certificate* parámetro en el valor de la *certificado* archivo. Establecer el *ssl_key* al valor de la *clave* archivo.

### <a name="deploy-on-aks-and-field-programmable-gate-array-fpga"></a>Implementar en AKS y la matriz de puerta programable de campo (FPGA)

  > [!NOTE]
  > La información de esta sección también se aplica al implementar un servicio web seguro para la interfaz visual. Si no está familiarizado con el uso del SDK de Python, consulte [¿qué es el SDK de Azure Machine Learning para Python?](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

Cuando se implementa en AKS, puede asociar uno existente o crear un nuevo clúster AKS.
  
-  Si crea un nuevo clúster, usa  **[AksCompute.provisionining_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#provisioning-configuration-agent-count-none--vm-size-none--ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--location-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--service-cidr-none--dns-service-ip-none--docker-bridge-cidr-none-)** .
- Si se conecta a un clúster existente, usa  **[AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none-)** . Ambos devuelven un objeto de configuración que tiene un **enable_ssl** método.

El **enable_ssl** método puede usar un certificado proporcionado por Microsoft o un certificado que adquiera.

  * Cuando se usa un certificado de Microsoft, debe usar el *leaf_domain_label* parámetro. Este parámetro genera el nombre DNS para el servicio. Por ejemplo, un valor de "myservice" crea un nombre de dominio de "myservice\<seis caracteres aleatorios >.\< azureregion >. cloudapp.azure.com ", donde \<azureregion > es la región que contiene el servicio. Si lo desea, puede usar el *overwrite_existing_domain* parámetro para sobrescribir la existente *leaf_domain_label*.

    Para implementar el servicio con SSL habilitado (o volver a implementar), establezca el *ssl_enabled* parámetro en "True" siempre que sea aplicable. Establecer el *ssl_certificate* parámetro en el valor de la *certificado* archivo. Establecer el *ssl_key* al valor de la *clave* archivo.

    > [!IMPORTANT]
    > Cuando se usa un certificado de Microsoft, no es necesario adquirir su propio certificado o nombre de dominio.

    El ejemplo siguiente muestra cómo crear una configuración que permite un certificado SSL de Microsoft:

    ```python
    from azureml.core.compute import AksCompute
    # Config used to create a new AKS cluster and enable SSL
    provisioning_config = AksCompute.provisioning_configuration()
    provisioning_config.enable_ssl(leaf_domain_label = "myservice")
    # Config used to attach an existing AKS cluster to your workspace and enable SSL
    attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                          cluster_name = cluster_name)
    attach_config.enable_ssl(leaf_domain_label = "myservice")
    ```

  * Cuando usas *un certificado que compró*, usa el *ssl_cert_pem_file*, *ssl_key_pem_file*, y *ssl_cname* parámetros. En el ejemplo siguiente se muestra cómo usar *.pem* archivos para crear una configuración que usa un certificado SSL que adquirió:

    ```python
    from azureml.core.compute import AksCompute
    # Config used to create a new AKS cluster and enable SSL
    provisioning_config = AksCompute.provisioning_configuration()
    provisioning_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    # Config used to attach an existing AKS cluster to your workspace and enable SSL
    attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
    attach_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

Para obtener más información acerca de *enable_ssl*, consulte [AksProvisioningConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksprovisioningconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-) y [AksAttachConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksattachconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-).

### <a name="deploy-on-azure-container-instances"></a>Implementar en Azure Container Instances

Cuando se implementa en Azure Container Instances, proporcione valores para los parámetros relacionados con SSL, como el siguiente fragmento de código de muestra:

```python
from azureml.core.webservice import AciWebservice

aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
```

Para obtener más información, consulte [AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none-).

## <a name="update-your-dns"></a>Actualización del DNS

Después, debe actualizar el DNS para que apunte al servicio web.

+ **Para las instancias de contenedor:**

  Usar las herramientas desde el registrador de nombres de dominio para actualizar el registro DNS para el nombre de dominio. El registro debe apuntar a la dirección IP del servicio.

  Puede haber un retraso de minutos u horas antes de que los clientes pueden resolver el nombre de dominio, según el registrador y el "período de vida" (TTL) que está configurado para el nombre de dominio.

+ **Para AKS:**

  > [!WARNING]
  > Si ha usado *leaf_domain_label* para crear el servicio mediante un certificado de Microsoft, no actualice manualmente el valor DNS para el clúster. El valor debe establecerse automáticamente.

  Actualice el DNS en el **configuración** pestaña de la dirección IP pública del clúster AKS. (Vea la siguiente imagen). La dirección IP pública es un tipo de recurso que se crea en el grupo de recursos que contiene los nodos de agente AKS y otros recursos de red.

  ![Azure Machine Learning Service: protección de servicios web con SSL](./media/how-to-secure-web-service/aks-public-ip-address.png)

## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre cómo:
+ [Consumir un modelo de Machine Learning implementado como un servicio web](how-to-consume-web-service.md)
+ [Ejecutar de forma segura experimentos e inferencia dentro de una red virtual de Azure](how-to-enable-virtual-network.md)
