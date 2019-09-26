---
title: Traslado de una red virtual de Azure a otra región de Azure mediante Azure Portal
description: Use una plantilla de Azure Resource Manager para trasladar una red virtual de Azure de una región de Azure a otra mediante Azure Portal.
author: asudbring
ms.service: virtual-network
ms.topic: article
ms.date: 08/26/2019
ms.author: allensu
ms.openlocfilehash: a09ce7b77dfcaa51e7c82f67a5d20000f3e22b61
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71220001"
---
# <a name="move-azure-virtual-network-to-another-region-using-the-azure-portal"></a>Traslado de una red virtual de Azure a otra región mediante Azure Portal

Hay varios escenarios en los que puede desear mover las redes virtuales (VNET) de Azure existentes de una región a otra. Por ejemplo, puede que desee crear una red virtual con la misma configuración para la prueba y disponibilidad de la red virtual existente. También es posible que quiera mover una red virtual de producción a otra región como parte del planeamiento de la recuperación ante desastres.

Puede usar una plantilla de Azure Resource Manager para completar el traslado de la red virtual a otra región. Para ello, exporte la red virtual a una plantilla, modifique los parámetros para que coincidan con la región de destino y, a continuación, implemente la plantilla en la región nueva.  Para más información sobre Resource Manager y las plantillas, consulte [Inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal)


## <a name="prerequisites"></a>Requisitos previos

- Asegúrese de que la red virtual de Azure se encuentra en la región de Azure desde la que va a moverla.

- Para exportar una red virtual e implementar una plantilla para crear una red virtual en otra región, necesitará el rol Colaborador de red u otro superior.

- Los emparejamientos de redes virtuales no se volverán a crear y se producirá un error si todavía están presentes en la plantilla.  Tendrá que quitar todos los pares de redes virtuales antes de exportar la plantilla y, a continuación, volver a establecer los pares después de la migración de la red virtual.

- Identifique el diseño de red de origen y todos los recursos que está usando actualmente. Este diseño incluye, pero no se limita a los equilibradores de carga, grupos de seguridad de red (NSG), e IP públicas.

- Compruebe que su suscripción de Azure permite crear redes virtuales en la región de destino. Para habilitar la cuota necesaria, póngase en contacto con el soporte técnico.

- Asegúrese de que la suscripción tiene suficientes recursos para admitir la adición de redes virtuales para este proceso.  Vea [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits)


## <a name="prepare-and-move"></a>Preparación y traslado
En los pasos siguientes se muestra cómo preparar la red virtual para el traslado mediante una plantilla de Resource Manager y cómo trasladar la red virtual a la región de destino con el portal.

### <a name="export-the-template-and-deploy-from-the-portal"></a>Exportación de la plantilla e implementación desde el portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com) > **Grupos de recursos**.
2. Seleccione el grupo de recursos que contiene la red virtual de origen y haga clic en él.
3. Seleccione > **Configuración** > **Exportar plantilla**.
4. En la hoja **Exportar plantilla**, elija **Implementar**.
5. Haga clic en **PLANTILLA** > **Editar parámetros** para abrir el archivo **parameters.json** en el editor en línea.
6. Para editar el parámetro correspondiente al nombre de la red virtual, cambie la propiedad **value** de **parameters**:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "virtualNetworks_myVNET1_name": {
                "value": "<target-virtual-network-name>"
            }
        }
    }
    ```
7. Cambie el valor de nombre de la red virtual de origen en el editor por un nombre de su elección para la red virtual de destino. Asegúrese de escribir el nombre entre comillas.

8.  Haga clic en **Guardar** en el editor.

9.  Haga clic en **Plantilla** > **Editar plantilla** para abrir el archivo **template.json** en el editor en línea.

10. Para editar la región de destino a la que se va a trasladar la red virtual, cambie la propiedad **location** en **resources** en el editor en línea:

    ```json
    "resources": [
                {
                    "type": "Microsoft.Network/virtualNetworks",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('virtualNetworks_myVNET1_name')]",
                    "location": "<target-region>",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "resourceGuid": "6e2652be-35ac-4e68-8c70-621b9ec87dcb",
                        "addressSpace": {
                            "addressPrefixes": [
                                "10.0.0.0/16"
                            ]
                        },

    ```

11. Para obtener los códigos de ubicación de la región, consulte [Ubicaciones de Azure](https://azure.microsoft.com/global-infrastructure/locations/).  El código de una región es el nombre de la región sin espacios, **Centro de EE. UU.**  = **centralus**.

12. También puede cambiar otros parámetros de la plantilla si así lo desea; son opcionales según sus requisitos:

    * **Espacio de direcciones**: el espacio de direcciones de la red virtual se puede modificar antes de guardar al editar la sección **resources** > **addressSpace** y cambiar la propiedad **addressPrefixes** en el archivo **template.json**:

        ```json
                "resources": [
                    {
                    "type": "Microsoft.Network/virtualNetworks",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('virtualNetworks_myVNET1_name')]",
                    "location": "<target-region",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "resourceGuid": "6e2652be-35ac-4e68-8c70-621b9ec87dcb",
                    "addressSpace": {
                        "addressPrefixes": [
                        "10.0.0.0/16"
                        ]
                    },

        ```

    * **Subred**: es posible modificar o realizar adiciones al nombre de subred y al espacio de direcciones de subred si edita la sección **subnets** del archivo **template.json**. El nombre de la subred se puede cambiar si modifica la propiedad **name**. El espacio de direcciones de subred se puede cambiar si modifica la propiedad **addressPrefix** en el archivo **template.json**:

        ```json
                "subnets": [
                    {
                    "name": "subnet-1",
                    "etag": "W/\"d9f6e6d6-2c15-4f7c-b01f-bed40f748dea\"",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "addressPrefix": "10.0.0.0/24",
                    "delegations": [],
                    "privateEndpointNetworkPolicies": "Enabled",
                    "privateLinkServiceNetworkPolicies": "Enabled"
                    }
                    },
                    {
                    "name": "GatewaySubnet",
                    "etag": "W/\"d9f6e6d6-2c15-4f7c-b01f-bed40f748dea\"",
                    "properties": {
                    "provisioningState": "Succeeded",
                    "addressPrefix": "10.0.1.0/29",
                    "serviceEndpoints": [],
                    "delegations": [],
                    "privateEndpointNetworkPolicies": "Enabled",
                    "privateLinkServiceNetworkPolicies": "Enabled"
                    }
                    }

                ]
        ```

         Para cambiar el prefijo de dirección, debe editar el archivo **template.json** en dos lugares; primero en la sección mencionada anteriormente y después en la sección **type** a continuación.  Cambie la propiedad **addressPrefix** para que coincida con la anterior:

        ```json
         "type": "Microsoft.Network/virtualNetworks/subnets",
           "apiVersion": "2019-06-01",
           "name": "[concat(parameters('virtualNetworks_myVNET1_name'), '/GatewaySubnet')]",
              "dependsOn": [
                 "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_myVNET1_name'))]"
                   ],
              "properties": {
                 "provisioningState": "Succeeded",
                 "addressPrefix": "10.0.1.0/29",
                 "serviceEndpoints": [],
                 "delegations": [],
                 "privateEndpointNetworkPolicies": "Enabled",
                 "privateLinkServiceNetworkPolicies": "Enabled"
                  }
                 },
                  {
                  "type": "Microsoft.Network/virtualNetworks/subnets",
                  "apiVersion": "2019-06-01",
                  "name": "[concat(parameters('virtualNetworks_myVNET1_name'), '/subnet-1')]",
                     "dependsOn": [
                        "[resourceId('Microsoft.Network/virtualNetworks', parameters('virtualNetworks_myVNET1_name'))]"
                          ],
                     "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "10.0.0.0/24",
                        "delegations": [],
                        "privateEndpointNetworkPolicies": "Enabled",
                        "privateLinkServiceNetworkPolicies": "Enabled"
                         }
                  }
         ]
        ```

13. Haga clic en **Guardar** en el editor en línea.

14. Haga clic en **BÁSICO** > **Suscripción** para elegir la suscripción en la que se implementará la red virtual de destino.

15. Haga clic en **BÁSICO** > **Grupo de recursos** para elegir el grupo de recursos en el que se implementará la red virtual de destino.  Puede hacer clic en **Crear nuevo** para crear un nuevo grupo de recursos para la red virtual de destino.  Asegúrese de que el nombre no es el mismo que el del grupo de recursos de origen de la red virtual existente.

16. Compruebe que **BÁSICO** > **Ubicación** está establecido en la ubicación de destino en la que quiere implementar la red virtual.

17. En **CONFIGURACIÓN**, compruebe que el nombre coincide con el nombre que especificó en el editor de parámetros anterior.

18. Marque la casilla en **TÉRMINOS Y CONDICIONES**.

19. Haga clic en el botón **Comprar** para implementar la red virtual de destino.

## <a name="discard"></a>Discard (Descartar)

Si quiere descartar la red virtual de destino, elimine el grupo de recursos que contiene la red virtual de destino.  Para ello, en el portal, seleccione el grupo de recursos en el panel y, luego, **Eliminar** en la parte superior de la página de información general.

## <a name="clean-up"></a>Limpieza

Para confirmar los cambios y completar el traslado de la red virtual, elimine la red virtual o el grupo de recursos de origen. Para ello, en el portal, seleccione la red virtual o el grupo de recursos en el panel y seleccione **Eliminar** en la parte superior de cada página.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha movido una red virtual de Azure de una región a otra y ha limpiado los recursos de origen.  Para más información sobre cómo trasladar recursos entre regiones y la recuperación ante desastres en Azure, consulte:


- [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Traslado de máquinas virtuales de Azure a otra región](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
