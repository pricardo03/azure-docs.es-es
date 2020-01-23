---
title: Implementación en hosts dedicados
description: Uso de hosts dedicados para lograr un aislamiento real de nivel de host para las cargas de trabajo
ms.topic: article
ms.date: 01/10/2020
ms.author: danlep
ms.openlocfilehash: 619a39f4d08a4308cb0f566bc50860e9562bf9e4
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2020
ms.locfileid: "75903553"
---
# <a name="deploy-on-dedicated-hosts"></a>Implementación en hosts dedicados

"Dedicado" es una SKU de Azure Container Instances (ACI) que proporciona un entorno de proceso aislado y dedicado para ejecutar contenedores de forma segura. El uso de la SKU dedicada da como resultado que cada grupo de contenedores tenga un servidor físico dedicado en un centro de recursos de Azure, lo que garantiza un aislamiento completo de la carga de trabajo para ayudar a satisfacer los requisitos de cumplimiento y seguridad de su organización. 

La SKU dedicada es adecuada para cargas de trabajo de contenedor que requieren aislamiento de la carga de trabajo desde una perspectiva de servidor físico.

## <a name="using-the-dedicated-sku"></a>Uso de la SKU dedicada

> [!IMPORTANT]
> El uso de la SKU dedicada solo está disponible en la versión de API más reciente (2019-12-01) que se está implementando actualmente. Especifique esta versión de API en la plantilla de implementación. Además, el límite predeterminado de cualquier suscripción para usar la SKU dedicada es 0. Si desea usar esta SKU para las implementaciones de contenedores de producción, cree una [solicitud de soporte técnico de Azure][azure-support]

A partir de la versión 2019-12-01 de la API, hay una propiedad "sku" en la sección Propiedades del grupo de contenedores de una plantilla de implementación, que es necesaria para una implementación ACI. Actualmente, puede usar esta propiedad como parte de una plantilla de implementación de Azure Resource Manager para ACI. Puede obtener más información sobre la implementación de recursos de ACI con una plantilla en el [Tutorial: Implementación de un grupo con varios contenedores con una plantilla de Resource Manager](https://docs.microsoft.com/azure/container-instances/container-instances-multi-container-group). 

Esta propiedad sku admite cualquiera de los siguientes valores:
* Standard: la opción de implementación ACI estándar, que sigue garantizando la seguridad de nivel de hipervisor 
* Dedicado: se usa para el aislamiento de nivel de carga de trabajo con hosts físicos dedicados para el grupo de contenedores

## <a name="modify-your-json-deployment-template"></a>Modificar la plantilla de implementación JSON

En la plantilla de implementación, donde se especifica el recurso de grupo de contenedores, asegúrese de que la `"apiVersion": "2019-12-01",`. En la sección de propiedades del recurso de grupo de contenedores, establezca `"sku": "Dedicated",`.

Este es un fragmento de ejemplo para la sección de recursos de una plantilla de implementación de grupo de contenedores que usa la SKU dedicada:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "containerGroupName": {
        "type": "string",
        "defaultValue": "myContainerGroup",
        "metadata": {
          "description": "Container Group name."
        }
      }
    },
    "resources": [
        {
            "name": "[parameters('containerGroupName')]",
            "type": "Microsoft.ContainerInstance/containerGroups",
            "apiVersion": "2019-12-01",
            "location": "[resourceGroup().location]",
            "properties": {
                "sku": "Dedicated",
                "containers": [
                    {
                        "name": "container1",
                        "properties": {
                            "image": "nginx",
                            "command": [
                                "/bin/sh",
                                "-c",
                                "while true; do echo `date`; sleep 1000000; done"
                            ],
                            "ports": [
                                {
                                    "protocol": "TCP",
                                    "port": 80
                                }
                            ],
                            "environmentVariables": [],
                            "resources": {
                                "requests": {
                                    "memoryInGB": 1.0,
                                    "cpu": 1
                                }
                            }
                        }
                    }
                ],
                "restartPolicy": "Always",
                "ipAddress": {
                    "ports": [
                        {
                            "protocol": "TCP",
                            "port": 80
                        }
                    ],
                    "type": "Public"
                },
                "osType": "Linux",
            },
            "location": "eastus2euap",
            "tags": {}
        }
    ]
}
```

## <a name="deploy-your-container-group"></a>Implementación del grupo de contenedores

Si ha creado y editado el archivo de plantilla de implementación en el escritorio, puede cargarlo en el directorio de Cloud Shell si lo arrastra a dicho directorio. 

Para crear un grupo de recursos, use el comando [az group create][az-group-create].

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Implemente la plantilla con el comando [az group deployment create][az-group-deployment-create].

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file deployment-template.json
```

Al cabo de unos segundos, debe recibir una respuesta inicial de Azure. Una vez completada la implementación, todos los datos relacionados con Azure que conserva el servicio ACI se cifrarán con la clave proporcionada.

<!-- LINKS - Internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
