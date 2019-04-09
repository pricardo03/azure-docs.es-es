---
title: Implementación de Avere vFXT for Azure
description: Pasos para implementar el clúster de Avere vFXT en Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: v-erkell
ms.openlocfilehash: 7ded66c29f12b8f68746726ca6c126bffbc51f0d
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2019
ms.locfileid: "59056612"
---
# <a name="deploy-the-vfxt-cluster"></a>Implementación del clúster de vFXT

Este procedimiento explica cómo usar el asistente para la implementación disponible en Azure Marketplace. El asistente implementa automáticamente el clúster mediante una plantilla de Azure Resource Manager. Después de escribir los parámetros en el formulario y hacer clic en **Crear**, Azure completa automáticamente estos pasos:

* Crea el controlador del clúster, que es una máquina virtual básica que contiene el software necesario para implementar y administrar el clúster.
* Configura el grupo de recursos y la infraestructura de red virtual, incluida la creación de nuevos elementos.
* Crea el clúster de máquinas virtuales de nodos y configura el clúster Avere.
* Si se solicita, crea un nuevo contenedor de blobs de Azure y lo configura como un sistema de almacenamiento de clúster principal.

Después de seguir las instrucciones de este documento, tendrá una red virtual, una subred, un controlador y un clúster vFXT tal como se muestra en el diagrama siguiente. Este diagrama muestra al filtro de core de Azure Blob opcional, que incluye un nuevo contenedor de almacenamiento de blobs (en una nueva cuenta de almacenamiento, que no se muestra) y un punto de conexión de servicio para el almacenamiento de Microsoft dentro de la subred. 

![diagrama que muestra tres rectángulos concéntricos con Avere componentes del clúster. El rectángulo exterior se denomina "Grupo de recursos" y contiene un hexágono con la etiqueta "Blob storage (opcional)". El rectángulo en el siguiente es con la etiqueta ' red Virtual: 10.0.0.0/16' y no contiene ningún componente único. El rectángulo más interno tiene la etiqueta 'Subnet:10.0.0.0/24' y contiene una máquina virtual con la etiqueta 'Controlador de clúster', una pila de tres máquinas virtuales con la etiqueta 'vFXT nodos (clúster vFXT)' y un hexágono con la etiqueta 'Punto de conexión de servicio'. Hay una flecha que conecta el punto de conexión de servicio (que se encuentra dentro de la subred) y el almacenamiento de blobs (que está fuera de la subred y red virtual, en el grupo de recursos). La flecha que se pasa a través de la subred y los límites de red virtual.](media/avere-vfxt-deployment.png)  

Antes de usar la plantilla de creación, asegúrese de haber abordado estos requisitos previos:  

1. [Suscripción nueva](avere-vfxt-prereqs.md#create-a-new-subscription)
1. [Permisos de propietario de la suscripción](avere-vfxt-prereqs.md#configure-subscription-owner-permissions)
1. [Cuota del clúster de vFXT](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)
1. [Punto de conexión de servicio de almacenamiento (si es necesario)](avere-vfxt-prereqs.md#create-a-storage-service-endpoint-in-your-virtual-network-if-needed) : requerido para se implementa mediante una red virtual existente y la creación de almacenamiento de blobs

Para más información sobre los pasos de planeación e implementación del clúster, lea [Planeamiento de un sistema de Avere vFXT](avere-vfxt-deploy-plan.md) e [Descripción general de la implementación](avere-vfxt-deploy-overview.md).

## <a name="create-the-avere-vfxt-for-azure"></a>Creación del clúster de Avere vFXT for Azure

Obtener acceso a la plantilla de creación en Azure portal buscando Avere y seleccionando "Avere vFXT para la plantilla de ARM de Azure". 

![Ventana del explorador que muestra Azure Portal con la ruta "Nuevo > Marketplace > Todo". En la página, el campo de búsqueda tiene todo el término "avere" y el segundo resultado, "Avere vFXT para la plantilla de ARM de Azure" se muestra en rojo para resaltarlo.](media/avere-vfxt-template-choose.png)

Después de leer los detalles en el vFXT Avere para la página de la plantilla de ARM de Azure, haga clic en **crear** para comenzar. 

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

* **Nombre del clúster de Avere vFXT**: asigne al clúster un nombre exclusivo. 

* **Tamaño** -esta sección muestra el tipo de máquina virtual que se usará para los nodos del clúster. Aunque hay solo una opción recomendada, el **cambiar tamaño** vínculo abre una tabla con detalles sobre este tipo de instancia y un vínculo a una calculadora de precios.  

* **Tamaño de la caché por nodo**: la memoria caché del clúster se reparte entre los nodos del clúster, por lo que el tamaño total de la caché del clúster de Avere vFXT será el tamaño de la caché por nodo multiplicado por el número de nodos. 

  La configuración recomendada es usar 4 TB por nodo para nodos Standard_E32s_v3.

* **Red virtual** : definir una nueva red virtual para alojar el clúster, o seleccione una red virtual existente que cumpla los requisitos previos descritos en [diseñar el sistema de vFXT Avere](avere-vfxt-deploy-plan.md#resource-group-and-network-infrastructure). 

  > [!NOTE]
  > Si crea una red virtual, el controlador de clúster tendrá una dirección IP pública, para poder acceder a la nueva red privada. Si elige una red virtual existente, se configura el controlador del clúster sin una dirección IP pública. 
  > 
  > Una dirección IP visible públicamente en el controlador de clúster facilita el acceso al clúster de vFXT, pero plantea un pequeño riesgo de seguridad. 
  >  * Una dirección IP pública en el controlador del clúster le permite usarla como host de salto para conectarse al clúster de Avere vFXT desde fuera de la subred privada.
  >  * Si no configura una dirección IP pública en el controlador, debe usar otro host de salto, una conexión VPN o ExpressRoute para acceder al clúster. Por ejemplo, cree el controlador dentro de una red virtual que ya tenga configurada una conexión VPN.
  >  * Si crea un controlador con una dirección IP pública, debe proteger la VM de controlador con un grupo de seguridad de red. De forma predeterminada, la implementación de Avere vFXT for Azure crea un grupo de seguridad de red y restringe el acceso entrante solo al puerto 22 para los controladores con direcciones IP públicas. Puede proteger aún más el sistema al bloquear el acceso al intervalo de direcciones IP de origen, es decir, solo permitir las conexiones desde las máquinas que pretende usar para el acceso al clúster.

  La plantilla de implementación también configura la nueva red virtual con un punto de conexión de servicio de almacenamiento para el almacenamiento de blobs de Azure y con control de acceso de red bloqueado a solo las direcciones IP de la subred del clúster. 

* **Subred**: elija una subred de la red virtual existente o cree una. 

* **Crear y usar el almacenamiento de blobs** -elija **true** para crear un nuevo contenedor de blobs de Azure y configurarla como almacenamiento back-end para el nuevo clúster de vFXT Avere. Esta opción también crea una nueva cuenta de almacenamiento dentro del mismo grupo de recursos como el clúster y un punto de conexión de servicio de almacenamiento de Microsoft dentro de la subred del clúster. 
  
  Si se proporciona una red virtual existente, debe tener un punto de conexión de servicio de almacenamiento antes de crear el clúster. (Para obtener más información, lea [diseñar el sistema de vFXT Avere](avere-vfxt-deploy-plan.md).)

  Establezca este campo en **false** si no desea crear un contenedor. En este caso, debe asociar y configurar el almacenamiento después de crear el clúster. Puede encontrar las instrucciones en [Configurar el almacenamiento](avere-vfxt-add-storage.md). 

* **(Nuevo) Cuenta de almacenamiento** : si crear un nuevo contenedor de blobs de Azure, escriba un nombre para la nueva cuenta de almacenamiento. 

## <a name="validation-and-purchase"></a>Compra y validación

La página tres resume la configuración y valida los parámetros. Si la validación es correcta, haga clic en el botón **Aceptar** para continuar. 

![Tercera página de la plantilla de implementación: validación](media/avere-vfxt-deploy-3.png)

En la cuarta página, escriba cualquier información de contacto necesario y haga clic en el **crear** botón para aceptar los términos y crear el vFXT Avere para el clúster de Azure. 

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

## <a name="next-step"></a>Paso siguiente

Ahora que el clúster se está ejecutando y conoce la dirección IP de administración, puede [conectarse a la herramienta de configuración de clústeres](avere-vfxt-cluster-gui.md) para habilitar el soporte técnico, agregar almacenamiento si es necesario y personalizar otra configuración del clúster.
