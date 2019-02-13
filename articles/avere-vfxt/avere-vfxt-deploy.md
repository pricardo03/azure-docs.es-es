---
title: Implementación de Avere vFXT for Azure
description: Pasos para implementar el clúster de Avere vFXT en Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: 972ba937ad15fa9a6d2eb74e3e4c9e6e8f3923a4
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55745442"
---
# <a name="deploy-the-vfxt-cluster"></a>Implementación del clúster de vFXT

Este procedimiento explica cómo usar el asistente para la implementación disponible en Azure Marketplace. El asistente implementa automáticamente el clúster mediante una plantilla de Azure Resource Manager. Después de escribir los parámetros en el formulario y hacer clic en **Crear**, Azure completa automáticamente estos pasos: 

* Crea el controlador de clúster, que es una máquina virtual básica que contiene el software necesario para implementar y administrar el clúster.
* Configura el grupo de recursos y la infraestructura de red virtual, incluida la creación de elementos si es necesario.
* Crea las máquinas virtuales del nodo de clúster y las configura como el clúster de Avere.
* En caso de que se le solicite, cree un contenedor de blobs de Azure y configúrelo como un archivador principal del clúster.

Después de seguir las instrucciones de este documento, tendrá una red virtual, una subred, un controlador y un clúster de vFXT, tal como se muestra en el diagrama siguiente:

![diagrama que muestra la red virtual que contiene el almacenamiento de blobs opcional y una subred que contiene tres VM agrupadas etiquetadas como nodos de vFXT/clúster de vFXT y una VM etiquetada como controlador del clúster](media/avere-vfxt-deployment.png)

Una vez creado el clúster, debe [crear un punto de conexión de almacenamiento](#create-a-storage-endpoint-if-using-azure-blob) en la red virtual si usa el almacenamiento de blobs. 

Antes de usar la plantilla de creación, asegúrese de haber abordado estos requisitos previos:  

1. [Suscripción nueva](avere-vfxt-prereqs.md#create-a-new-subscription)
1. [Permisos de propietario de la suscripción](avere-vfxt-prereqs.md#configure-subscription-owner-permissions)
1. [Cuota del clúster de vFXT](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)
1. [Roles de acceso personalizados](avere-vfxt-prereqs.md#create-access-roles): debe crear un rol de control de acceso basado en rol para asignarlo a los nodos de clúster. También tiene la opción de crear un rol de acceso personalizado para el controlador de clúster, pero la mayoría de los usuarios adoptarán el rol de propietario predeterminado, que proporciona los privilegios de controlador correspondientes a un propietario del grupo de recursos. Lea [Roles integrados en los recursos de Azure](../role-based-access-control/built-in-roles.md#owner) para obtener información detallada.

Para más información sobre los pasos de planeación e implementación del clúster, lea [Planeamiento de un sistema de Avere vFXT](avere-vfxt-deploy-plan.md) e [Descripción general de la implementación](avere-vfxt-deploy-overview.md).

## <a name="create-the-avere-vfxt-for-azure"></a>Creación del clúster de Avere vFXT for Azure

Acceda a la plantilla de creación en Azure Portal; para ello, busque Avere y seleccione "Avere vFXT ARM Deployment". 

![Ventana del explorador que muestra Azure Portal con la ruta "Nuevo > Marketplace > Todo". En la página Todo, el campo de búsqueda tiene el término "avere" y el segundo resultado, "Avere vFXT ARM Deployment", está subrayado en rojo para resaltarlo.](media/avere-vfxt-template-choose.png)

Después de leer los detalles de la página de Avere vFXT ARM Deployment, haga clic en **Crear** para comenzar. 

![Azure Marketplace con la visualización de la primera página de la plantilla de implementación](media/avere-vfxt-deploy-first.png)

La plantilla se divide en cuatro pasos: dos páginas de recopilación de información más los pasos de validación y confirmación. 

* La primera página se centra en la configuración de la máquina virtual del controlador de clúster. 
* La segunda página recopila los parámetros necesarios para crear el clúster y los recursos asociados, como subredes y almacenamiento. 
* La tercera página resume y valida la configuración. 
* La cuarta página explica los términos y condiciones del software y permite iniciar el proceso de creación del clúster. 

## <a name="page-one-parameters---cluster-controller-information"></a>Parámetros de la primera página: información sobre el controlador de clúster

La primera página de la plantilla de implementación recopila información sobre el controlador de clúster. 

![Primera página de la plantilla de implementación](media/avere-vfxt-deploy-1.png)

Rellene la información siguiente:

* **Nombre del controlador de clúster**: establezca el nombre de la máquina virtual del controlador de clúster.

* **Nombre de usuario del controlador**: rellene el nombre de usuario raíz para la máquina virtual del controlador de clúster. 

* **Tipo de autenticación**: elija la contraseña o la autenticación con clave pública SSH para conectarse al controlador. Se recomienda el método de clave pública SSH; lea [Creación y uso de claves SSH](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) si necesita ayuda.

* **Contraseña** o **clave pública SSH**: según el tipo de autenticación seleccionado, debe proporcionar una clave pública RSA o una contraseña en los campos siguientes. Esta credencial se usa con el nombre de usuario proporcionado anteriormente.

* **Id. de rol de creación del clúster de Avere**: use este campo para especificar el rol de control de acceso para el controlador de clúster. El valor predeterminado es el rol integrado [propietario](../role-based-access-control/built-in-roles.md#owner). Los privilegios de propietario para el controlador de clúster están restringidos al grupo de recursos del clúster. 

  Debe usar el identificador único global que se corresponde con el rol. Para el valor predeterminado (propietario), el GUID es 8e3af657-a8ff-443c-a75c-2fe8c4bcb635. Para encontrar el GUID de un rol personalizado, use este comando: 

  ```azurecli
  az role definition list --query '[*].{roleName:roleName, name:name}' -o table --name 'YOUR ROLE NAME'
  ```

* **Suscripción**: seleccione la suscripción para Avere vFXT. 

* **Grupo de recursos**: seleccione un grupo de recursos vacío existente para el clúster de Avere vFXT o haga clic en "Crear" y escriba un nombre para el nuevo grupo de recursos. 

* **Ubicación**: seleccione la ubicación de Azure para el clúster y los recursos.

Haga clic en **Aceptar** cuando haya finalizado. 

> [!NOTE]
> Si desea que el controlador de clúster tenga una dirección IP pública, cree una red virtual para el clúster en lugar de seleccionar una red existente. Esta configuración se encuentra en la segunda página.

## <a name="page-two-parameters---vfxt-cluster-information"></a>Parámetros de la segunda página: información del clúster de vFXT

La segunda página de la plantilla de implementación permite establecer el tamaño del clúster, el tipo de nodo, el tamaño de la caché y los parámetros de almacenamiento, entre otras opciones. 

![Segunda página de la plantilla de implementación](media/avere-vfxt-deploy-2.png)

* **Número de nodos del clúster de Avere vFXT**: elija el número de nodos que desea usar en el clúster. El valor mínimo es tres nodos y el máximo es doce. 

* **Contraseña de administración del clúster**: crea la contraseña para la administración del clúster. Esta contraseña se utilizará con el nombre de usuario ```admin``` para iniciar sesión en el panel de control del clúster a fin de supervisar el clúster y configurarlo.

* **Rol de operaciones del clúster de Avere**: especifique el nombre del rol de control de acceso para los nodos de clúster. Se trata de un rol personalizado que se creó como un paso de requisito previo. 

  En el ejemplo descrito en [Cree el rol de acceso del nodo de clúster.](avere-vfxt-prereqs.md#create-the-cluster-node-access-role), se guarda el archivo como ```avere-operator.json``` y el nombre del rol correspondiente es ```avere-operator```.

* **Nombre del clúster de Avere vFXT**: asigne al clúster un nombre exclusivo. 

* **Tamaño**: especifique el tipo de máquina virtual que desea usar al crear los nodos de clúster. 

* **Tamaño de la caché por nodo**: la memoria caché del clúster se reparte entre los nodos del clúster, por lo que el tamaño total de la caché del clúster de Avere vFXT será el tamaño de la caché por nodo multiplicado por el número de nodos. 

  La configuración recomendada es usar 1 TB por nodo si se usan nodos de clúster Standard_D16s_v3 y usar 4 TB por nodo si se usan nodos Standard_E32s_v3.

* **Red virtual**: seleccione una red virtual existente para hospedar el clúster o defina una nueva red virtual para crearla. 

  > [!NOTE]
  > Si crea una red virtual, el controlador de clúster tendrá una dirección IP pública, para poder acceder a la nueva red privada. Si elige una red virtual existente, se configura el controlador del clúster sin una dirección IP pública. 
  > 
  > Una dirección IP visible públicamente en el controlador de clúster facilita el acceso al clúster de vFXT, pero plantea un pequeño riesgo de seguridad. 
  >  * Una dirección IP pública en el controlador del clúster le permite usarla como host de salto para conectarse al clúster de Avere vFXT desde fuera de la subred privada.
  >  * Si no configura una dirección IP pública en el controlador, debe usar otro host de salto, una conexión VPN o ExpressRoute para acceder al clúster. Por ejemplo, cree el controlador dentro de una red virtual que ya tenga configurada una conexión VPN.
  >  * Si crea un controlador con una dirección IP pública, debe proteger la VM de controlador con un grupo de seguridad de red. De forma predeterminada, la implementación de Avere vFXT for Azure crea un grupo de seguridad de red y restringe el acceso entrante solo al puerto 22 para los controladores con direcciones IP públicas. Puede proteger aún más el sistema al bloquear el acceso al intervalo de direcciones IP de origen, es decir, solo permitir las conexiones desde las máquinas que pretende usar para el acceso al clúster.

* **Subred**: elija una subred de la red virtual existente o cree una. 

* **Uso de Blob Storage**: elija **true** para crear un contenedor de blobs de Azure y configurarlo como almacenamiento back-end para el nuevo clúster de Avere vFXT. Esta opción también crea una cuenta de almacenamiento dentro del mismo grupo de recursos que el clúster. 

  Establezca este campo en **false** si no desea crear un contenedor. En este caso, debe asociar y configurar el almacenamiento después de crear el clúster. Puede encontrar las instrucciones en [Configurar el almacenamiento](avere-vfxt-add-storage.md). 

* **Cuenta de almacenamiento**: si crea un contenedor de blobs de Azure, escriba el nombre de la nueva cuenta de almacenamiento. 

## <a name="validation-and-purchase"></a>Compra y validación

En la tercera página se ofrece un resumen de la configuración y se validan los parámetros. Si la validación es correcta, haga clic en el botón **Aceptar** para continuar. 

![Tercera página de la plantilla de implementación: validación](media/avere-vfxt-deploy-3.png)

En la cuarta página, haga clic en el botón **Crear** para aceptar los términos y crear el clúster de Avere vFXT for Azure. 

![Cuarta página de la plantilla de implementación: términos y condiciones y botón Crear](media/avere-vfxt-deploy-4.png)

La implementación del clúster tarda entre 15 y 20 minutos.

## <a name="gather-template-output"></a>Recopilación de la salida de la plantilla

Cuando la plantilla de Avere vFXT termina de crear el clúster, genera como resultado alguna información importante sobre el nuevo clúster. 

> [!TIP]
> Asegúrese de copiar la dirección IP de administración del resultado de la plantilla. Necesita esta dirección para administrar el clúster.

Para encontrar esta información, siga este procedimiento:

1. Vaya al grupo de recursos del controlador del clúster.

1. En el lado izquierdo, haga clic en **Implementaciones** y, luego, en **microsoft-avere.vfxt-template**.

   ![Página del portal del grupo de recursos con Implementaciones seleccionada a la izquierda y microsoft-avere.vfxt-template que aparece en una tabla en el Nombre de la implementación](media/avere-vfxt-outputs-deployments.png)

1. En el lado izquierdo, haga clic en **Salidas**. Copie los valores en cada uno de los campos. 

   ![página de resultados en la que se muestran los valores SSHSTRING, RESOURCE_GROUP, LOCATION, NETWORK_RESOURCE_GROUP, NETWORK, SUBNET, SUBNET_ID, VSERVER_IPs y MGMT_IP en los campos situados a la derecha de las etiquetas](media/avere-vfxt-outputs-values.png)


## <a name="create-a-storage-endpoint-if-using-azure-blob"></a>Creación de un punto de conexión de almacenamiento (si usa Azure Blob)

Si utiliza almacenamiento de Azure Blob para el almacenamiento de datos de back-end, debe crear un punto de conexión de servicio de almacenamiento en la red virtual. Este [punto de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md) mantiene el tráfico de Azure Blob de manera local, en lugar de enrutarlo fuera de la red virtual.

1. En el portal, haga clic en **Redes virtuales** a la izquierda.
1. Seleccione la red virtual para el controlador. 
1. Haga clic en **Extremos de servicio** a la izquierda.
1. Haga clic en **Agregar** en la parte superior.
1. Deje el servicio como ``Microsoft.Storage`` y elija la subred del controlador.
1. Haga clic en **Agregar**en la parte inferior.

  ![Captura de pantalla de Azure Portal con anotaciones de los pasos para crear el punto de conexión de servicio](media/avere-vfxt-service-endpoint.png)

## <a name="next-step"></a>Paso siguiente

Ahora que el clúster se está ejecutando y conoce la dirección IP de administración, puede [conectarse a la herramienta de configuración de clústeres](avere-vfxt-cluster-gui.md) para habilitar el soporte técnico, agregar almacenamiento si es necesario y personalizar otra configuración del clúster.
