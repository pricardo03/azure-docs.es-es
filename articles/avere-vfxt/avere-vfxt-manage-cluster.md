---
title: 'Administración del clúster de Avere de vFXT: Azure'
description: 'Cómo administrar el clúster de Avere: agregar o quitar nodos, y reiniciar, detener o destruir el clúster de vFXT'
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: bc91b052d3d69924af9afeb012c0ebb5be01dfbf
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55745561"
---
# <a name="manage-the-avere-vfxt-cluster"></a>Administración del clúster de Avere de vFXT

Después de crear el clúster, es posible que deba agregar nodos de clúster, o bien detener o reiniciar el clúster. Una vez finalizado el proyecto, debe saber cómo detener y quitar el clúster de forma permanente. 

Dependiendo de la tarea de administración del clúster, es posible que deba usar el panel de control de Avere, el script de creación de clúster de la línea de comandos de vfxt.py o Azure Portal para hacerlo. 

Esta tabla ofrece una descripción general de las herramientas que se pueden usar para cada tarea. 

| . | Panel de control de Avere | vfxt.py  | Azure Portal |
| --- | --- | --- | --- |
| Agregar nodos de clúster | no | Sí | no |
| Quitar nodos de clúster | Sí | no | no |
| Detener un nodo de clúster | Sí (también puede reiniciar los servicios o el sistema) | no | El apagado de una máquina virtual del nodo desde el portal se interpreta como un error de nodo |
| Iniciar un nodo detenido | no | no | Sí |
| Destruir un único nodo de clúster | no | no | Sí |
| Reiniciar el clúster |  |  |  |
| Apagar o detener el clúster de forma segura | Sí | Sí | no |
| Destruir el clúster  | no | Sí | Sí, pero no se garantiza la integridad de los datos |

A continuación, se incluyen instrucciones detalladas de cada herramienta.

## <a name="about-stopped-instances-in-azure"></a>Acerca de las instancias detenidas en Azure

Al apagar o detener cualquier máquina virtual de Azure, deja de incurrir en gastos de proceso, pero debe seguir pagando por su almacenamiento. Si apaga un nodo de vFXT o todo el clúster de vFXT y no quiere intentar reiniciarlo, debe usar Azure Portal para eliminar las máquinas virtuales relacionadas. 

En Azure Portal, un nodo *detenido* (que se puede reiniciar) muestra el estado **detenido**; un nodo *eliminado* muestra el estado **detenido (desasignado)**  y ya no incurre en cargos de proceso o almacenamiento.

Antes de eliminar la máquina virtual, asegúrese de que todos los datos cambiados se han escrito de la memoria caché al almacenamiento de back-end mediante las opciones del panel de control de Avere o vfxt.py para detener o apagar el clúster.

## <a name="manage-the-cluster-with-avere-control-panel"></a>Administración del clúster con el panel de control de Avere 

El panel de control de Avere se puede usar para estas tareas: 

* Detener o reiniciar nodos individuales
* Quitar un nodo del clúster
* Detener o reiniciar todo el clúster

El panel de control de Avere prioriza la integridad de los datos. Por tanto, intenta escribir los datos modificados en el almacenamiento de back-end antes de una operación posiblemente destructiva. Esto la convierte en una opción más segura que el portal de Avere. 

Acceda al panel de control de Avere desde un explorador web. Siga las instrucciones de [Acceso al clúster de vFXT](avere-vfxt-cluster-gui.md) si necesita ayuda.

### <a name="manage-nodes-with-avere-control-panel"></a>Administración de nodos con el panel de control de Avere

La página de configuración **FXT Nodes** (Nodos de FXT) tiene controles para administrar nodos individuales.

Para apagar, reiniciar o quitar un nodo, busque el nodo en la lista de la página **FXT Nodes** (Nodos de FXT) y haga clic en el botón correspondiente en su columna **Acciones**.

> [!NOTE] 
> Las direcciones IP se pueden mover entre nodos del clúster si el número de nodos activos cambia.

Lea [Cluster > FXT Nodes](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_fxt_nodes.html#gui-fxt-nodes>) (Clúster > Nodos de FXT) en la guía de configuración del clúster de Avere para obtener más información.

### <a name="stop-or-reboot-the-cluster-with-avere-control-panel"></a>Detención o reinicio del clúster con el panel de control de Avere

La página de configuración **System Maintenance** (Mantenimiento del sistema) presenta comandos para reiniciar los servicios del clúster, reiniciar el clúster o apagar el clúster de forma segura. Lea [Administration > System Maintenance](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_system_maintenance.html#gui-system-maintenance>) (Administración > Mantenimiento del sistema) en la guía de configuración del clúster de Avere para obtener más información.

Cuando un clúster se está apagando, publica mensajes de estado en la pestaña **Dashboard** (Panel) en primer lugar. Transcurridos unos instantes, la sesión del panel de control de Avere dejará de responder, lo que significa que el clúster se ha apagado.

## <a name="manage-the-cluster-with-vfxtpy"></a>Administración del clúster con vfxt.py

vfxt.py es una herramienta de línea de comandos para la administración y creación de clústeres. 

vfxt.py está preinstalado en la máquina virtual del controlador del clúster. Si quiere instalarlo en otro sistema, consulte la documentación en <https://github.com/Azure/AvereSDK>.

El script vfxt.py puede usarse para estas tareas de administración del clúster:

* Agregar nodos nuevos a un clúster.
* Detener o iniciar un clúster.  
* Destruir un clúster.

Al igual que el panel de control de Avere, las operaciones de vfxt.py intentan garantizar que los datos cambiados se almacenan permanentemente en el almacenamiento de back-end antes de apagar o destruir el clúster o el nodo. Esto la convierte en una opción más segura que el portal de Avere.

Una guía de uso completa de vfxt.py está disponible en GitHub: [administración de clúster en la nube con vfxt.py](https://github.com/azure/averesdk/blob/master/docs/README.md)

### <a name="add-cluster-nodes-with-vfxtpy"></a>Adición de nodos de clúster con vfxt.py

Una script de ejemplo para agregar nodos de clúster se incluye en el controlador del clúster. Busque ``./add-nodes`` en el controlador y ábralo en un editor para personalizarlo con la información de su clúster. 

El clúster debe estar ejecutándose para usar este comando. 

Use los valores siguientes: 

* Nombre del grupo de recursos del clúster, y también de los recursos de red y almacenamiento si no son los mismos que los del clúster
* Ubicación del clúster
* Red y subred del clúster 
* Rol de acceso del nodo de clúster 
* Dirección IP de administración del clúster y contraseña administrativa 
* Número de nodos que se va a agregar (1, 2 o 3)
* Valores de tamaño de la caché y tipo de instancia del nodo 

Si no usa el prototipo, debe construir un comando como el siguiente, que incluya toda la información descrita anteriormente. 

```bash
   vfxt.py --cloud-type azure --from-environment \
   --resource-group GROUP_NAME \ 
   [--network-resource-group GROUP_NAME --storage-resource-group GROUP_NAME]  \
   --location LOCATION --azure-network NETWORK_NAME --azure-subnet SUBNET_NAME \
   --add-nodes --nodes NODE_COUNT \
   --management-address CLUSTER_IP --admin-password ADMIN_PASSWORD \
   --instance-type TYPE --node-cache-size SIZE \
   --azure-role ROLE_NAME \
   --log ~/vfxt.log
```

Para obtener más información, lea [Add nodes to a cluster](https://github.com/Azure/AvereSDK/blob/master/docs/using_vfxt_py.md#add-nodes-to-a-cluster) (Adición de nodos a un clúster) en la guía de uso de vfxt.py.

### <a name="stop-a-cluster-with-vfxtpy"></a>Detención de un clúster con vfxt.py

```bash
vfxt.py --cloud-type azure --from-environment --stop --resource-group GROUPNAME --admin-password PASSWORD --management-address ADMIN_IP --location LOCATION --azure-network NETWORK --azure-subnet SUBNET
```

### <a name="start-a-stopped-cluster-with-vfxtpy"></a>Inicio de un clúster detenido con vfxt.py

```bash
vfxt.py --cloud-type azure --from-environment --start --resource-group GROUPNAME --admin-password PASSWORD --management-address ADMIN_IP --location LOCATION --azure-network NETWORK --azure-subnet SUBNET --instances INSTANCE1_ID INSTANCE2_ID INSTANCE3_ID ...
```    

Dado que el clúster está detenido, debe pasar identificadores de instancia para especificar los nodos del clúster. Lea [Specifying which cluster to modify](https://github.com/Azure/AvereSDK/blob/master/docs/using_vfxt_py.md#specifying-which-cluster-to-modify) (Especificación del clúster que se va a modificar) en la guía de uso de vfxt.py para obtener más información.

### <a name="destroy-a-cluster-with-vfxtpy"></a>Destrucción de un clúster con vfxt.py

```bash
vfxt.py --cloud-type azure --from-environment --destroy --resource-group GROUPNAME --admin-password PASSWORD --management-address ADMIN_IP --location LOCATION --azure-network NETWORK --azure-subnet SUBNET --management-address ADMIN_IP
```

La opción ``--quick-destroy`` puede usarse si no quiere escribir datos modificados de la caché del clúster.

Lea la [guía de uso de vfxt.py](<https://github.com/Azure/AvereSDK/blob/master/docs/README.md>) para obtener más información.  

## <a name="manage-cluster-vms-from-the-azure-portal"></a>Administración de máquinas virtuales del clúster desde Azure Portal 

Azure Portal puede usarse para destruir máquinas virtuales del clúster individualmente, pero no se garantiza la integridad de los datos si el clúster no se apaga primero correctamente. 

Azure Portal puede usarse para estas tareas de administración del clúster: 

* Iniciar un nodo de vFXT detenido.
* Detener un nodo de vFXT individual (el clúster lo interpreta como un error del nodo).
* Destruir un clúster de vFXT *si* no es necesario garantizar que los datos modificados en la caché del clúster se escriben en el archivador principal.
* Quitar permanentemente los nodos de vFXT y otros recursos del clúster una vez apagado con seguridad.

### <a name="restart-vfxt-instances-from-the-azure-portal"></a>Reinicio de las instancias de vFXT desde Azure Portal

Si necesita reiniciar un nodo detenido, debe usar Azure Portal. Seleccione **Máquinas virtuales** en el menú izquierdo y, a continuación, haga clic en el nombre de la máquina virtual en la lista para abrir la página de información general correspondiente.

Haga clic en el botón **Iniciar** de la parte superior de la página de información general para reactivar la máquina virtual.

![Pantalla de Azure Portal con la opción para iniciar una máquina virtual detenida](media/avere-vfxt-start-stopped-incurring-annot.png)

### <a name="delete-cluster-nodes"></a>Eliminación de nodos del clúster

Si quiere eliminar un nodo del clúster de vFXT, pero mantener el resto del clúster, primero debe [quitar el nodo del clúster](#manage-nodes-with-avere-control-panel) con el panel de control de Avere.

> [!CAUTION]
> Si elimina un nodo sin quitarlo primero del clúster de vFXT, los datos podrían perderse.

Para destruir permanentemente una o varias instancias usadas como un nodo de vFXT, use Azure Portal.
Seleccione **Máquinas virtuales** en el menú izquierdo y, a continuación, haga clic en el nombre de la máquina virtual en la lista para abrir la página de información general correspondiente.

Haga clic en el botón **Eliminar** de la parte superior de la página de información general para destruir la máquina virtual de forma permanente.

Puede usar este método para quitar permanentemente los nodos del clúster una vez apagado con seguridad. 

### <a name="destroy-the-cluster-from-the-azure-portal"></a>Destrucción del clúster desde Azure Portal

> [!NOTE] 
> Si quiere que los cambios del cliente restantes en la memoria caché se escriban en el almacenamiento de back-end, use la opción `--destroy` de vfxt.py o el panel de control de Avere para apagar el clúster correctamente antes de quitar las instancias de nodo en Azure Portal.

Para destruir instancias de nodo permanentemente, las puede eliminar en Azure Portal. Puede eliminarlas de una en una como se ha descrito anteriormente, o bien puede usar la página **Máquinas virtuales** para encontrar todas las máquinas virtuales del clúster, seleccionarlas con las casillas de verificación y hacer clic en el botón **Eliminar** para quitarlas todas a la vez.

![Lista de máquinas virtuales en el portal, filtrada por el término "clúster", con tres de las cuatro activadas y resaltadas](media/avere-vfxt-multi-vm-delete.png)

### <a name="delete-additional-cluster-resources-from-the-azure-portal"></a>Eliminación de recursos de clúster adicionales desde Azure Portal

Si ha creado recursos adicionales específicamente para el clúster de vFXT, puede quitarlos como parte de la destrucción del clúster. No debe destruir los elementos que contienen datos que necesita ni elementos compartidos con otros proyectos.

Además de eliminar los nodos de clúster, considere la posibilidad de quitar estos componentes: 

* Máquina virtual del controlador del clúster
* Discos de datos asociados a los nodos de clúster
* Interfaces de red y direcciones IP públicas asociadas con componentes de clúster
* Redes virtuales
* Cuentas de almacenamiento (**solo** si no contienen ningún dato importante)
* Conjunto de disponibilidad 

![Lista de "todos los recursos" de Azure Portal con los recursos creados para un clúster de prueba](media/avere-vfxt-all-resources-list.png)

### <a name="delete-a-clusters-resource-group-from-the-azure-portal"></a>Eliminación del grupo de recursos de un clúster de Azure Portal

Si ha creado un grupo de recursos específicamente para alojar el clúster, puede destruir todos los recursos relacionados del clúster mediante la destrucción del grupo de recursos. 

> [!Caution] 
> Destruya el grupo de recursos solamente si está seguro de que no hay nada de valor en el grupo. Por ejemplo, asegúrese de que se han movido los datos necesarios de los contenedores de almacenamiento del grupo de recursos.  

Para eliminar un grupo de recursos, haga clic en **Grupos de recursos** en el menú izquierdo del portal y filtre la lista de grupos de recursos para encontrar el que creó para el clúster de vFXT. Seleccione el grupo de recursos y haga clic en los tres puntos situados a la derecha del panel. Seleccione **Eliminar grupo de recursos**. El portal le pedirá que confirme la eliminación, que es irreversible.  

![Grupo de recursos con la acción "Eliminar grupo de recursos"](media/avere-vfxt-delete-resource-group.png)
