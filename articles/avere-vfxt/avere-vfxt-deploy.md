---
title: Implementación de Avere vFXT for Azure
description: Pasos para implementar el clúster de Avere vFXT en Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: d7c207f89b9cb50f940f071fbbf6ee81b4d44976
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "52164330"
---
# <a name="deploy-the-vfxt-cluster"></a>Implementación del clúster de vFXT

La manera más sencilla de crear un clúster de vFXT en Azure es usar un controlador del clúster. El controlador del clúster es una VM que incluye los scripts, las plantillas y la infraestructura de software necesarios para crear y administrar el clúster de vFXT.

La implementación de un nuevo clúster de vFXT incluye estos pasos:

1. [Cree el controlador del clúster](#create-the-cluster-controller-vm).
1. Si usa Azure Blob Storage, [cree un punto de conexión de almacenamiento](#create-a-storage-endpoint-if-using-azure-blob) en la red virtual.
1. [Conéctese al controlador del clúster](#access-the-controller). El resto de estos pasos se realiza desde la VM de controlador del clúster. 
1. [Cree el rol de acceso](#create-the-cluster-node-access-role) para los nodos del clúster. Se proporciona un prototipo.
1. [Personalice el script de creación del clúster](#edit-the-deployment-script) para el tipo de clúster de vFXT que quiere crear.
1. [Ejecute el script de creación del clúster](#run-the-script).

Para más información sobre los pasos de planeación e implementación del clúster, lea [Planeamiento de un sistema de Avere vFXT](avere-vfxt-deploy-plan.md) e [Descripción general de la implementación](avere-vfxt-deploy-overview.md). 

Después de seguir las instrucciones de este documento, tendrá una red virtual, una subred, un controlador y un clúster de vFXT, tal como se muestra en el diagrama siguiente:

![diagrama que muestra la red virtual que contiene el almacenamiento de blobs opcional y una subred que contiene tres VM agrupadas etiquetadas como nodos de vFXT/clúster de vFXT y una VM etiquetada como controlador del clúster](media/avere-vfxt-deployment.png)

Antes de comenzar, asegúrese de haber abordado estos requisitos previos:  

1. [Suscripción nueva](avere-vfxt-prereqs.md#create-a-new-subscription)
1. [Permisos de propietario de la suscripción](avere-vfxt-prereqs.md#configure-subscription-owner-permissions)
1. [Cuota del clúster de vFXT](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)

De manera opcional, puede crear el rol del nodo de clúster [antes](avere-vfxt-pre-role.md) de crear el controlador del clúster, pero es más fácil hacerlo después.

## <a name="create-the-cluster-controller-vm"></a>Creación de la VM de controlador del clúster

El primer paso es crear la máquina virtual que va a crear y configurar los nodos del clúster de vFXT. 

El controlador del clúster es una VM Linux con los scripts y el software de creación de clústeres de Avere vFXT preinstalados. No necesita una cantidad importante de potencia de procesamiento ni espacio de almacenamiento, por lo que puede elegir opciones de bajo costo. Esta VM se usará esporádicamente durante la vigencia del clúster vFXT.

Hay dos métodos para crear la VM de controlador del clúster. Un [plantilla de Azure Resource Manager](#create-controller---arm-template) se incluye [a continuación](#create-controller---arm-template) para simplificar el proceso, pero también puede crear el controlador a partir de la [imagen de Azure Marketplace](#create-controller---azure-marketplace-image). 

Puede crear un grupo de recursos como parte de la creación del controlador.

> [!TIP]
>
> Decida si quiere usar una dirección IP pública en el controlador del clúster o no. Una dirección IP pública proporciona fácil acceso al clúster de vFXT, pero genera un pequeño riesgo de seguridad.
>
>  * Una dirección IP pública en el controlador del clúster le permite usarla como host de salto para conectarse al clúster de Avere vFXT desde fuera de la subred privada.
>  * Si no configura una dirección IP pública en el controlador, debe usar otro host de salto, una conexión VPN o ExpressRoute para acceder al clúster. Por ejemplo, cree el controlador dentro de una red virtual que tenga configurada una conexión VPN.
>  * Si crea un controlador con una dirección IP pública, debe proteger la VM de controlador con un grupo de seguridad de red. Permita el acceso únicamente a través del puerto 22 para proporcionar acceso a internet.

### <a name="create-controller---resource-manager-template"></a>Creación del controlador con una plantilla de Resource Manager

Para crear el nodo del controlador del clúster desde el portal, haga clic en el botón "Implementar en Azure". Esta plantilla de implementación crea la VM que creará y administrará el clúster de Avere vFXT.

[![botón para crear el controlador](media/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmaster%2Fsrc%2Fvfxt%2Fazuredeploy.json)

Proporcione la siguiente información.

En la sección **BÁSICO**:  

* **Suscripción** para el clúster
* **Grupo de recursos** para el clúster 
* **Ubicación** 

En la sección **CONFIGURACIÓN**:

* Si va a crear una nueva red virtual o no

  * Si crea una nueva red virtual, el controlador del clúster se asignará una dirección IP pública para que pueda acceder a él. También se crea un grupo de seguridad de red para esta red virtual que restringe el tráfico entrante únicamente al puerto 22.
  * Si quiere usar ExpressRoute o una VPN para conectarse al controlador del clúster, establezca este valor en `false` y especifique una red virtual existente en los campos restantes. El controlador del clúster usará esa red virtual para la comunicación de red. 

* Grupo de recursos de red virtual, nombre y nombre de subred: escriba los nombres de los recursos existentes (si usa una red virtual existente) o escriba nuevos nombres si crea una red virtual
* **Nombre del controlador**: establezca un nombre para la VM de controlador
* Nombre de usuario de administrador de controlador: el valor predeterminado es `azureuser`
* Claves SSH: pegue la clave pública para asociar con el nombre de usuario de administrador. Lea [Creación y uso de un par de claves SSH](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) si necesita ayuda.

En **TÉRMINOS Y CONDICIONES**: 

* Lea los Términos de servicio y haga clic en la casilla para aceptarlos. 

  > [!NOTE] 
  > Si no es propietario de la suscripción, pida a un propietario que acepte los términos por usted según los pasos de requisitos previos de [Accept software terms in advance](avere-vfxt-prereqs.md#accept-software-terms-in-advance) (Aceptación de los términos de software por adelantado). 


Haga clic en **Comprar** cuando termine. Después de cinco o seis minutos, el nodo del controlador estará en funcionamiento.

Visite la página de resultados para recopilar la información del controlador que necesita para crear el clúster. Consulte [Información necesaria para crear el clúster](#information-needed-to-create-the-cluster) para más información.

### <a name="create-controller---azure-marketplace-image"></a>Creación del controlador: imagen de Azure Marketplace

Busque la plantilla del controlador desde la búsqueda de Azure Marketplace con el nombre ``Avere``. Seleccione la plantilla **Avere vFXT for Azure Controller**.

Si aún no lo ha hecho, acepte los términos y habilite el acceso mediante programación para la imagen de Marketplace haciendo clic en el vínculo "Want to deploy programmatically" (¿Quiere implementar mediante programación?) debajo del botón **Crear**.

![Captura de pantalla de un vínculo para el acceso mediante programación, que se encuentra debajo del botón Crear](media/avere-vfxt-deploy-programmatically.png)

Haga clic en el botón **Habilitar** y guarde la configuración.

![Captura de pantalla que muestra el clic del mouse para habilitar el acceso mediante programación](media/avere-vfxt-enable-program.png)

Vuelva a la página principal de la plantilla **Avere vFXT for Azure Controller** y haga clic en **Crear**. 

En el primer panel, rellene o confirme las opciones básicas:

* **Suscripción**
* **Grupo de recursos** (escriba un nuevo nombre si quiere crear un grupo).
* **Nombre de máquina virtual**: Nombre del controlador
* **Región**
* **Opciones de disponibilidad**: Redundancia no es necesaria
* **Imagen**: Imagen del nodo del controlador de Avere vFXT
* **Tamaño**: Deje el valor predeterminado o elija otro tipo de bajo costo
* **Cuenta de administrador**: Establezca cómo iniciar sesión en el controlador del clúster: 
  * Seleccione Nombre de usuario/contraseña o Clave pública SSH (recomendado).
  
    > [!TIP] 
    > Una clave SSH es más segura. Lea [Creación y uso de un par de claves SSH](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) si necesita ayuda. 
  * Especifique el nombre de usuario. 
  * Pegue la clave SSH, o escriba la contraseña y confírmela.
* **Reglas de puerto de entrada**: Si usa una dirección IP pública, abra el puerto 22 (SSH)

Haga clic en **Siguiente** para establecer las opciones de disco:

* **Tipo de disco del sistema operativo**: Valor predeterminado de HDD es suficiente
* **Usar discos no administrados**: No es necesario
* **Discos de datos**: No use

Haga clic en **siguiente** para las opciones de conexión en red:

* **Red virtual**: Seleccione la red virtual para el controlador, o escriba un nombre para crear una red virtual. Si no quiere usar una dirección IP pública en el controlador, considere la posibilidad de ubicarlo en una red virtual que tenga ExpressRoute u otro método de acceso ya configurado.
* **Subred**: Seleccione una subred dentro de la red virtual (opcional). Si crea una red virtual, puede crear una subred al mismo tiempo.
* **Dirección IP pública**: Si quiere usar una dirección IP pública, puede especificarla aquí. 
* **Grupo de seguridad de red**: Deje la configuración predeterminada (**Básico**). 
* **Puertos de entrada públicos**: Si usa una dirección IP pública, use este control para permitir el acceso desde el tráfico SSH. 
* **Redes aceleradas** no está disponible para esta VM.

Haga clic en **Siguiente** para establecer las opciones de administración:

* **Diagnósticos de arranque**: Cambie a **Desactivado**.
* **Diagnósticos del SO invitado**: Deje deshabilitado.
* **Cuenta de almacenamiento de diagnóstico**: De manera opcional, seleccione o especifique una cuenta nueva para almacenar la información de diagnóstico.
* **Identidad de servicio administrada**: Cambie esta opción a **Activado**, lo que crea una entidad de servicio de Azure AD para el controlador del clúster.
* **Apagado automático**: Deje desactivado. 

En este momento, puede hacer clic en **Revisar y crear** si no quiere usar etiquetas de la instancia. En caso contrario, haga clic en **Siguiente** dos veces para omitir la página **Configuración de invitado** e ir a la página de etiquetas. Cuando termine allí, haga clic en **Revisar y crear**. 

Una vez validadas las selecciones, haga clic en el botón **Crear**.  

La creación tarda cinco o seis minutos.

## <a name="create-a-storage-endpoint-if-using-azure-blob"></a>Creación de un punto de conexión de almacenamiento (si usa Azure Blob)

Si utiliza almacenamiento de Azure Blob para el almacenamiento de datos de back-end, debe crear un punto de conexión de servicio de almacenamiento en la red virtual. Este [punto de conexión de servicio](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) mantiene el tráfico de Azure Blob de manera local, en lugar de enrutarlo a través de internet.

1. En el portal, haga clic en **Redes virtuales** a la izquierda.
1. Seleccione la red virtual para el controlador. 
1. Haga clic en **Extremos de servicio** a la izquierda.
1. Haga clic en **Agregar** en la parte superior.
1. Deje el servicio como ``Microsoft.Storage`` y elija la subred del controlador.
1. Haga clic en **Agregar**en la parte inferior.

  ![Captura de pantalla de Azure Portal con anotaciones de los pasos para crear el punto de conexión de servicio](media/avere-vfxt-service-endpoint.png)

## <a name="information-needed-to-create-the-cluster"></a>Información necesaria para crear el clúster

Después de crear el controlador del clúster, asegúrese de que dispone de la información que necesita para los pasos siguientes. 

Información necesaria para conectar el controlador: 

* Nombre de usuario y clave SSH (o contraseña) del controlador
* Dirección IP del controlador u otro método para conectarse a la VM de controlador

Información necesaria para el clúster: 

* Definición de un nombre de grupo de recursos
* Ubicación de Azure 
* Nombre de la red virtual
* Nombre de subred
* Nombre de rol del nodo de clúster: este nombre se establece al crear el rol, como se describe [a continuación](#create-the-cluster-node-access-role)
* Nombre de la cuenta de almacenamiento si crea un contenedor de blobs

Si ha creado el nodo del controlador con la plantilla de Resource Manager, puede obtener la información de la [salida de la plantilla](#find-template-output). 

Si ha usado la imagen de Azure Marketplace para crear el controlador, ya habrá proporcionado directamente la mayoría de estos elementos. 

En la página de información de la VM de controlador puede encontrar la información que falta. Por ejemplo, haga clic en **Todos los recursos** y busque el nombre del controlador. A continuación, haga clic en el nombre del controlador para ver los detalles.

### <a name="find-template-output"></a>Búsqueda de los resultados de la plantilla

Para buscar esta información desde los resultados de la plantilla de Resource Manager, siga este procedimiento:

1. Vaya al grupo de recursos del controlador del clúster.

1. En el lado izquierdo, haga clic en **Implementaciones** y, luego, en **Microsoft.Template**.

   ![Página del portal del grupo de recursos con Implementaciones seleccionada a la izquierda y Microsoft.Template que aparece en una tabla en el Nombre de la implementación](media/avere-vfxt-deployment-template.png)

1. En el lado izquierdo, haga clic en **Salidas**. Copie los valores en cada uno de los campos. 

   ![Página de resultados con SSHSTRING, RESOURCE_GROUP, LOCATION, NETWORK, SUBNET, y SUBNET_ID en los campos a la derecha de las etiquetas](media/avere-vfxt-template-outputs.png)

## <a name="access-the-controller"></a>Acceso al controlador

Para seguir con el resto de los pasos de la implementación, deberá conectarse al controlador del clúster.

1. El método para conectarse al controlador del clúster depende de la configuración.

   * Si el controlador tiene una dirección IP pública, use SSH a la dirección IP del controlador como el nombre de usuario administrador que configuró (por ejemplo, ``ssh azureuser@40.117.136.91``).
   * Si el controlador no tiene una dirección IP pública, use una VPN o una conexión [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) a la red virtual.

1. Después de iniciar sesión en el controlador, ejecute `az login` para autenticarse. Copie el código de autenticación proporcionado en el shell y, luego, use un explorador web para cargar [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin) y autenticarse en el sistema de Microsoft. Vuelva al shell para la confirmación.

   ![Resultado de la línea de comandos del comando "AZ login" que muestra el vínculo de explorador y el código de autenticación](media/avere-vfxt-azlogin.png)

1. Para agregar su suscripción, ejecute este comando con el identificador de suscripción: ```az account set --subscription YOUR_SUBSCRIPTION_ID```

## <a name="create-the-cluster-node-access-role"></a>Creación del rol de acceso del nodo de clúster

> [!NOTE] 
> * Si no es propietario de la suscripción y aún no se ha creado el rol, pida a un propietario de la suscripción que siga estos pasos o siga el procedimiento de [Creación del rol de acceso en tiempo de ejecución del clúster Avere vFXT sin un controlador](avere-vfxt-pre-role.md).
> 
> * Los usuarios internos de Microsoft deben usar el rol existente denominado "Avere Cluster Runtime Operator" (Operador de tiempo de ejecución de clúster Avere) en lugar de intentar crear uno. 

El [control de acceso basado en roles](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC) da a los nodos de clúster de vFXT la autorización para realizar las tareas necesarias.  

Como parte de la operación normal del clúster vFXT, los nodos de vFXT individuales deben hacer cosas como leer las propiedades de los recursos de Azure, administrar el almacenamiento y controlar la configuración de la interfaz de red de otros nodos. 

1. En el controlador, abra el archivo ``/avere-cluster.json`` en un editor.

   ![consola que muestra un comando de lista y, luego, "vi /avere-cluster.json"](media/avere-vfxt-open-role.png)

1. Edite el archivo para incluir el identificador de suscripción y elimine la línea encima de él. Guarde el archivo como ``avere-cluster.json``.

   ![Editor de texto de la consola que muestra el identificador de suscripción y "eliminar esta línea" seleccionado para su eliminación](media/avere-vfxt-edit-role.png)

1. Use este comando para crear el rol:  

   ```bash
   az role definition create --role-definition /avere-cluster.json
   ```

Pasará el nombre del rol para el script de creación del clúster en el paso siguiente. 

## <a name="create-nodes-and-configure-the-cluster"></a>Creación de nodos y configuración del clúster

Para crear el clúster de Avere vFXT, edite uno de los scripts de ejemplo incluidos en el controlador y ejecútelo allí. Los scripts de ejemplo se encuentran en el directorio raíz (`/`) en el controlador del clúster.

* Si quiere crear un contenedor de blobs para usarlo como sistema de almacenamiento back-end de Avere vFXT, use el script ``create-cloudbacked-cluster``.

* Si va a agregar almacenamiento más adelante, use el script ``create-minimal-cluster``.

> [!TIP]
> Prototipos de scripts para agregar nodos y destruir el clúster de vFXT también se incluyen en el directorio `/` de la VM de controlador del clúster.

### <a name="edit-the-deployment-script"></a>Edición del script de implementación

Abra el script de ejemplo en un editor. Es posible que quiera guardar el script personalizado con un nombre diferente para evitar sobrescribir el ejemplo original.

Escriba valores para estas variables de script.

* Definición de un nombre de grupo de recursos

  * Si usa componentes de red o de almacenamiento que se encuentran en distintos grupos de recursos, quite la marca de comentario de las variables y proporcione también esos nombres. 

```python
# Resource groups
# At a minimum specify the resource group.  If the network resources live in a
# different group, specify the network resource group.  Likewise for the storage
# account resource group.
RESOURCE_GROUP=
#NETWORK_RESOURCE_GROUP=
#STORAGE_RESOURCE_GROUP=
```

* Nombre de la ubicación
* Nombre de la red virtual
* Nombre de subred
* Nombre de rol en tiempo de ejecución de Azure AD: Si ha seguido el ejemplo de [Creación del rol de acceso del nodo de clúster](#create-the-cluster-node-access-role), use ``avere-cluster``. 
* Nombre de la cuenta de almacenamiento (si crea un nuevo contenedor de blobs)
* Nombre del clúster: No puede tener dos clústeres de vFXT con el mismo nombre en el mismo grupo de recursos. El procedimiento recomendado es asignar un nombre único a cada clúster.
* Contraseña administrativa: Elija una contraseña segura para supervisar y administrar el clúster. Esta contraseña se asigna al usuario ``admin``. 
* Tipo de instancia del nodo: Consulte [vFXT node sizes](avere-vfxt-deploy-plan.md#vfxt-node-sizes) (Tamaños de nodo vFXT) para más información.
* Tamaño de caché del nodo: Consulte [vFXT node sizes](avere-vfxt-deploy-plan.md#vfxt-node-sizes) (Tamaños de nodo vFXT) para más información.

Guarde el archivo y salga.

### <a name="run-the-script"></a>Ejecute el script

Para ejecutar el script, escriba el nombre de archivo que ha creado. (Ejemplo: `./create-cloudbacked-cluster-west1`)  

> [!TIP]
> Considere la posibilidad de ejecutar este comando dentro de un [terminal multiplexor](http://linuxcommand.org/lc3_adv_termmux.php), como `screen` o `tmux`, por si pierde la conexión.  

La salida también se registra en `~/vfxt.log`.

Cuando finalice el script, copie la dirección IP de administración, que es necesaria para la administración del clúster.

![Resultado de la línea de comandos del script que muestra la dirección IP de administración cerca del final](media/avere-vfxt-mgmt-ip.png)

## <a name="next-step"></a>Paso siguiente

Ahora que el clúster se está ejecutando y conoce la dirección IP de administración, puede [conectarse a la herramienta de configuración de clústeres](avere-vfxt-cluster-gui.md) para habilitar el soporte técnico y agregar almacenamiento si es necesario.
