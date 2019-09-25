---
title: Traslado de un equilibrador de carga externo de Azure a otra región de Azure mediante Azure Portal
description: Use una plantilla de Azure Resource Manager para trasladar un equilibrador de carga externo de Azure de una región de Azure a otra mediante Azure Portal.
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/17/2019
ms.author: allensu
ms.openlocfilehash: eda0d6e8fe56b985c3b29fa80cee880444d63741
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105293"
---
# <a name="move-azure-external-load-balancer-to-another-region-using-the-azure-portal"></a>Traslado de un equilibrador de carga externo a otra región mediante Azure Portal

Hay varios escenarios en los que quizá quiera trasladar su equilibrador de carga externo actual de una región a otra. Por ejemplo, puede que quiera crear un equilibrador de carga externo con la misma configuración para realizar pruebas. También puede que quiera trasladar un equilibrador de carga externo a otra región como parte del planeamiento para la recuperación ante desastres.

Los equilibradores de carga externos de Azure no se pueden trasladar de una región a otra. Sin embargo, puede usar una plantilla de Azure Resource Manager para exportar la configuración y dirección IP pública actuales de un equilibrador de carga externo.  Después, puede preparar el recurso para otra región al exportar el equilibrador de carga y la dirección IP pública a una plantilla, modificar los parámetros para que coincidan con la región de destino y, a continuación, implementar la plantilla en la nueva región.  Para más información sobre Resource Manager y sus plantillas, consulte [Exportación de grupos de recursos a plantillas](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates).


## <a name="prerequisites"></a>Requisitos previos

- Asegúrese de que el equilibrador de carga externo de Azure se encuentra en la región de Azure desde la que va a realizar el traslado.

- Los equilibradores de carga externos de Azure no se pueden trasladar entre regiones.  Tendrá que asociar el nuevo equilibrador de carga a los recursos de la región de destino.

- Necesitará contar con el rol de colaborador de red u otro superior para exportar la configuración de un equilibrador de carga externo e implementar una plantilla para crear un equilibrador de carga externo en otra región.
   
- Identifique el diseño de red de origen y todos los recursos que está usando actualmente. Este diseño incluye, entre otros, equilibradores de carga, grupos de seguridad de red (NSG), direcciones IP públicas y redes virtuales.

- Compruebe que la suscripción a Azure permite crear equilibradores de carga externos en la región de destino que se usa. Para habilitar la cuota necesaria, póngase en contacto con el soporte técnico.

- Asegúrese de que la suscripción tiene suficientes recursos para admitir la adición de equilibradores de carga para este proceso.  Vea [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits)


## <a name="prepare-and-move"></a>Preparación y traslado
En los pasos siguientes se muestra cómo preparar el equilibrador de carga externo para el traslado mediante una plantilla de Resource Manager y cómo trasladar la configuración del equilibrador de carga externo a la región de destino mediante Azure Portal.  Como parte de este proceso, debe incluirse y crearse la configuración de la dirección IP pública del equilibrador de carga externo antes de trasladarlo.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-public-ip-template-and-deploy-from-the-portal"></a>Exportación de la plantilla de la dirección IP pública e implementación desde el portal

1. Inicie sesión en [Azure Portal](http://portal.azure.com) > **Grupos de recursos**.
2. Seleccione el grupo de recursos que contiene la dirección IP pública de origen y haga clic en él.
3. Seleccione > **Configuración** > **Exportar plantilla**.
4. Elija **Implementar** en la hoja **Exportar plantilla**.
5. Haga clic en **PLANTILLA** > **Editar parámetros** para abrir el archivo **parameters.json** en el editor en línea.
8. Para modificar el parámetro del nombre de la dirección IP pública, cambie la propiedad en **parameters** > **value** del nombre de la dirección IP pública de origen por el nombre de la dirección IP pública de destino. Asegúrese de que el nombre se encuentra entre comillas:

    ```json
            {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "publicIPAddresses_myVM1pubIP_name": {
            "value": "<target-publicip-name>"
              }
             }
            }

    ```

    Haga clic en **Guardar** en el editor.

9.  Haga clic en **PLANTILLA** > **Editar plantilla** para abrir el archivo **template.json** en el editor en línea. 

10. Para editar la región de destino a la que se va a trasladar la dirección IP pública, cambie la propiedad **location** en **resources**:

    ```json
            "resources": [
            {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2019-06-01",
            "name": "[parameters('publicIPAddresses_myPubIP_name')]",
            "location": "<target-region>",
            "sku": {
                "name": "Basic",
                "tier": "Regional"
            },
            "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "7549a8f1-80c2-481a-a073-018f5b0b69be",
                "ipAddress": "52.177.6.204",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
               }
               }
             ]             
    ```
  
11. Para obtener los códigos de ubicación de la región, consulte [Ubicaciones de Azure](https://azure.microsoft.com/global-infrastructure/locations/).  El código de una región es el nombre en inglés de la región sin espacios. Por ejemplo, Centro de EE. UU. es **Central US** = **centralus**.
    
12. Si quiere, también puede cambiar otros parámetros de la plantilla. Estos son opcionales según sus necesidades:

    * **SKU**: puede cambiar la SKU de la dirección IP pública en la configuración del nivel estándar al básico o viceversa si modifica la propiedad **sku** > **name** en el archivo **template.json**:

        ```json
          "resources": [
         {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2019-06-01",
            "name": "[parameters('publicIPAddresses_myPubIP_name')]",
            "location": "<target-region>",
            "sku": {
                "name": "Basic",
                "tier": "Regional"
            },
        ```

        Para más información sobre las diferencias entre las direcciones IP públicas de la SKU básica y estándar, consulte [Creación, modificación o eliminación de una dirección IP pública](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address):

    * **Método de asignación de IP pública** y **Tiempo de espera de inactividad**: puede cambiar estas dos opciones en la plantilla si cambia la propiedad **publicIPAllocationMethod** de **Dynamic** a **Static** o bien de **Static** a **Dynamic**. El tiempo de espera de inactividad se puede modificar si cambia la propiedad **idleTimeoutInMinutes** a la cantidad deseada.  El valor predeterminado es **4**:

        ```json
          "resources": [
         {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2019-06-01",
            "name": "[parameters('publicIPAddresses_myPubIP_name')]",
            "location": "<target-region>",
            "sku": {
                "name": "Basic",
                "tier": "Regional"
            },
            "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "7549a8f1-80c2-481a-a073-018f5b0b69be",
                "ipAddress": "52.177.6.204",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
        
        ```

        Para más información sobre los métodos de asignación y los valores de tiempo de espera de inactividad, consulte [Creación, modificación o eliminación de una dirección IP pública](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).

 
13. Haga clic en **Guardar** en el editor en línea.

14. Haga clic en **BÁSICO** > **Suscripción** para elegir la suscripción en la que se implementará la dirección IP pública de destino.

15. Haga clic en **BÁSICO** > **Grupo de recursos** para elegir el grupo de recursos en el que se implementará la dirección IP pública de destino.  Puede hacer clic en **Crear nuevo** para crear un nuevo grupo de recursos para la dirección IP pública de destino.  Asegúrese de que el nombre no es el mismo que el del grupo de recursos de origen de la dirección IP pública de origen existente. 

16. Compruebe que **BÁSICO** > **Ubicación** está establecido en la ubicación de destino en la que quiere implementar la dirección IP pública.

17. En **CONFIGURACIÓN**, compruebe que el nombre coincide con el nombre que especificó en el editor de parámetros anterior.

18. Marque la casilla en **TÉRMINOS Y CONDICIONES**.

19. Haga clic en el botón **Comprar** para implementar la dirección IP pública de destino.
20. Si tiene otra dirección IP pública que se usa para NAT de salida del equilibrador de carga que se está trasladando, repita los pasos anteriores para exportar e implementar la segunda dirección IP pública de salida en la región de destino.

### <a name="export-the-external-load-balancer-template-and-deploy-from-the-azure-portal"></a>Exportación de la plantilla del equilibrador de carga externo e implementación desde Azure Portal

1. Inicie sesión en [Azure Portal](http://portal.azure.com) > **Grupos de recursos**.
2. Seleccione el grupo de recursos que contiene el equilibrador de carga externo de origen y haga clic en él.
3. Seleccione > **Configuración** > **Exportar plantilla**.
4. Elija **Implementar** en la hoja **Exportar plantilla**.
5. Haga clic en **PLANTILLA** > **Editar parámetros** para abrir el archivo **parameters.json** en el editor en línea.

5. Para editar el parámetro del nombre del equilibrador de carga externo, cambie la propiedad **value** del nombre del equilibrador de carga externo de origen por el nombre del equilibrador de carga externo de destino. Asegúrese de que el nombre se encuentra entre comillas:

    ```json
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
          "loadBalancers_myLoadbalancer_ext_name": {
          "value": "<target-external-lb-name>"
    },
          "publicIPAddresses_myPubIP_in_externalid": {
          "value": "<target-publicIP-resource-ID>"
    },

    ```

6.  Para modificar el valor de la dirección IP pública de destino que se ha trasladado antes, primero debe obtener el identificador de recurso y, a continuación, copiarlo y pegarlo en el archivo **parameters.json**. Para obtener el identificador realice lo siguiente:
    
    1. Inicie sesión en [Azure Portal](http://portal.azure.com) > **Grupos de recursos** en otra pestaña o ventana del explorador.
    2. Seleccione el grupo de recursos de destino que contiene la dirección IP pública que se trasladó en los pasos anteriores y haga clic en él.
    3. Seleccione **Configuración** > **Propiedades**.
    4. En la hoja de la derecha, resalte **Id. de recurso** y cópielo en el Portapapeles.  También puede hacer clic en el botón **Copiar en el Portapapeles** a la derecha de la ruta de acceso del **Id. de recurso**.
    5. Pegue el identificador de recurso en la propiedad **value** del editor **Editar parámetros** que abrió en otra ventana o pestaña del explorador:

        ```json
           ```json
           "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
           "contentVersion": "1.0.0.0",
           "parameters": {
              "loadBalancers_myLoadbalancer_ext_name": {
              "value": "<target-external-lb-name>"
        },
              "publicIPAddresses_myPubIP_in_externalid": {
              "value": "<target-publicIP-resource-ID>"
        },

        ```
    6. Haga clic en **Guardar** en el editor en línea.
   

7.  Si ha configurado las reglas de salida y NAT de salida para el equilibrador de carga, en este archivo habrá una tercera entrada para el identificador externo de la IP pública saliente.  Repita los pasos anteriores en la **región de destino** para obtener el identificador de la dirección IP pública de salida y pegue esa entrada en el archivo **parameters.json**:

    ```json
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "loadBalancers_myLoadbalancer_ext_name": {
                "value": "<target-external-lb-name>",
                
            },
                "publicIPAddresses_myPubIP_in_externalid": {
                "value": "<target-publicIP-resource-ID>",
                
            },
                "publicIPAddresses_myPubIP_out_externalid": {
                "defaultValue": "<target-publicIP-outbound-resource-ID>",
                
            }
        },
    ```

8.  Haga clic en **PLANTILLA** > **Editar plantilla** para abrir el archivo **template.json** en el editor en línea.
9.  Para editar la región de destino a la que se va a trasladar la configuración del equilibrador de carga externo, cambie la propiedad **location** en **resources** del archivo **template.json**:

    ```json
        "resources": [
            {
                "type": "Microsoft.Network/loadBalancers",
                "apiVersion": "2019-06-01",
                "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
                "location": "<target-external-lb-region>",
                "sku": {
                    "name": "Standard",
                    "tier": "Regional"
                },
    ```

10. Para obtener los códigos de ubicación de la región, consulte [Ubicaciones de Azure](https://azure.microsoft.com/global-infrastructure/locations/).  El código de una región es el nombre en inglés de la región sin espacios. Por ejemplo, Centro de EE. UU. es **Central US** = **centralus**.

11. Si quiere, también puede cambiar otros parámetros de la plantilla. Estos son opcionales según sus necesidades:
    
    * **SKU**: puede cambiar la SKU del equilibrador de carga externo en la configuración del nivel estándar al básico o viceversa si modifica la propiedad **sku** > **name** en el archivo **template.json**:

        ```json
        "resources": [
        {
            "type": "Microsoft.Network/loadBalancers",
            "apiVersion": "2019-06-01",
            "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
            "location": "<target-external-lb-region>",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
        ```
      Para más información sobre las diferencias entre los equilibradores de carga de la SKU básica y estándar, consulte [Introducción a Azure Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

    * **Reglas de equilibrio de carga**: puede agregar o quitar reglas de equilibrio de carga en la configuración agregando o quitando entradas en la sección **loadBalancingRules** del archivo **template.json**:

        ```json
        "loadBalancingRules": [
                    {
                        "name": "myInboundRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false,
                            "loadDistribution": "Default",
                            "disableOutboundSnat": true,
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/backendAddressPools/myBEPoolInbound')]"
                            },
                            "probe": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/probes/myHTTPProbe')]"
                            }
                        }
                    }
                ]
        ```
       Para más información sobre las reglas de equilibrio de carga, consulte [¿Qué es Azure Load Balancer?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)

    * **Sondeos**: puede agregar o quitar sondeos para el equilibrador de carga en la configuración agregando o quitando entradas en la sección **probes** del archivo **template.json**:

        ```json
        "probes": [
                    {
                        "name": "myHTTPProbe",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "protocol": "Http",
                            "port": 80,
                            "requestPath": "/",
                            "intervalInSeconds": 15,
                            "numberOfProbes": 2
                        }
                    }
                ],
        ```
       Para más información sobre los sondeos de estado de Azure Load Balancer, consulte [Sondeos de estado de Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview).

    * **Reglas NAT de entrada**: puede agregar o quitar reglas NAT de entrada para el equilibrador de carga agregando o quitando entradas en la sección **inboundNatRules** del archivo **template.json**:

        ```json
        "inboundNatRules": [
                    {
                        "name": "myInboundNATRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 4422,
                            "backendPort": 3389,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false
                        }
                    }
                ]
        ```
        Para completar la adición o eliminación de una regla NAT de entrada, esta debe agregarse o quitarse como una propiedad **type** al final del archivo **template.json**:

        ```json
        {
            "type": "Microsoft.Network/loadBalancers/inboundNatRules",
            "apiVersion": "2019-06-01",
            "name": "[concat(parameters('loadBalancers_myLoadBalancer_name'), '/myInboundNATRule')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name'))]"
            ],
            "properties": {
                "provisioningState": "Succeeded",
                "frontendIPConfiguration": {
                    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                },
                "frontendPort": 4422,
                "backendPort": 3389,
                "enableFloatingIP": false,
                "idleTimeoutInMinutes": 4,
                "protocol": "Tcp",
                "enableTcpReset": false
            }
        }
        ```
        Para más información sobre las reglas NAT de entrada, consulte [¿Qué es Azure Load Balancer?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)

    * **Reglas de salida**: puede agregar o quitar reglas de salida en la configuración mediante la edición de la propiedad **outboundRules** en el archivo **template.json**:

        ```json
        "outboundRules": [
                    {
                        "name": "myOutboundRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "allocatedOutboundPorts": 10000,
                            "protocol": "All",
                            "enableTcpReset": false,
                            "idleTimeoutInMinutes": 15,
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/backendAddressPools/myBEPoolOutbound')]"
                            },
                            "frontendIPConfigurations": [
                                {
                                    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPoutbound')]"
                                }
                            ]
                        }
                    }
                ]
        ```

         Para más información sobre las reglas de salida, consulte [Reglas de salida de Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview).

12. Haga clic en **Guardar** en el editor en línea.
    
13. Haga clic en **BÁSICO** > **Suscripción** para elegir la suscripción en la que se implementará el equilibrador de carga externo de destino.

15. Haga clic en **BÁSICO** > **Grupo de recursos** para elegir el grupo de recursos en el que se implementará el equilibrador de carga de destino.  Puede hacer clic en **Crear nuevo** para crear un nuevo grupo de recursos para el equilibrador de carga externo de destino o puede elegir el grupo de recursos existente que se creó anteriormente para la dirección IP pública.  Asegúrese de que el nombre no es el mismo que el del grupo de recursos de origen del equilibrador de carga externo de origen existente. 

16. Compruebe que **BÁSICO** > **Ubicación** está establecido en la ubicación de destino en la que quiere implementar el equilibrador de carga externo.

17. En **CONFIGURACIÓN**, compruebe que el nombre coincide con el nombre que especificó en el editor de parámetros anterior.  Compruebe que los identificadores de recursos estén especificados para todas las direcciones IP públicas en la configuración.

18. Marque la casilla en **TÉRMINOS Y CONDICIONES**.

19. Haga clic en el botón **Comprar** para implementar la dirección IP pública de destino.

## <a name="discard"></a>Discard (Descartar) 

Si quiere descartar la dirección IP pública de destino y el equilibrador de carga externo, elimine el grupo de recursos que contiene la dirección IP pública de destino y el equilibrador de carga externo.  Para ello, seleccione el grupo de recursos desde el panel en el portal y seleccione **Eliminar** en la parte superior de la página de información general.

## <a name="clean-up"></a>Limpieza

Para confirmar los cambios y completar el traslado de la dirección IP pública y el equilibrador de carga externo, elimine la dirección IP pública de origen y el equilibrador de carga externo o el grupo de recursos. Para ello, seleccione la dirección IP pública y el equilibrador de carga externo o grupo de recursos desde el panel en el portal y seleccione **Eliminar** en la parte superior de cada página.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha migrado un equilibrador de carga externo de Azure de una región a otra y ha limpiado los recursos de origen.  Para más información sobre cómo trasladar recursos entre regiones y la recuperación ante desastres en Azure, consulte:


- [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Traslado de máquinas virtuales de Azure a otra región](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
