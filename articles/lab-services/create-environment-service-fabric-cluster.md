---
title: Crear un entorno con un clúster de Service Fabric en Azure DevTest Labs | Microsoft Docs
description: Aprenda a crear un entorno con un clúster de Service Fabric independiente e iniciar y detener el clúster mediante programaciones.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: EMaher
manager: femila
editor: spelluru
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2019
ms.author: enewman
ms.openlocfilehash: 9848f197800c391285c4065685b910685f0ac64b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60312184"
---
# <a name="create-an-environment-with-self-contained-service-fabric-cluster-in-azure-devtest-labs"></a>Crear un entorno con clúster de Service Fabric independiente en Azure DevTest Labs
En este artículo se proporciona información sobre cómo crear un entorno con un clúster de Service Fabric independiente en Azure DevTest Labs. 

## <a name="overview"></a>Información general
DevTest Labs puede crear entornos de prueba independiente como se define en plantillas de Azure Resource Manager. Estos entornos contienen los recursos de IaaS, como máquinas virtuales y los recursos de PaaS, como Service Fabric. DevTest Labs le permite administrar máquinas virtuales en un entorno, ya que proporciona comandos para controlar las máquinas virtuales. Estos comandos ofrecen la capacidad de iniciar o detener una máquina virtual en una programación. De forma similar, DevTest Labs también permite administrar clústeres de Service Fabric en un entorno. Puede iniciar o detener un clúster de Service Fabric en un entorno, ya sea manualmente o mediante una programación.

## <a name="create-a-service-fabric-cluster"></a>Creación de un clúster de Service Fabric
Clústeres de Service Fabric se crean mediante entornos en DevTest Labs. Cada entorno se define mediante una plantilla de Azure Resource Manager en un repositorio Git. El [repositorio Git público](https://github.com/Azure/azure-devtestlab/tree/master/Environments/) de DevTest Labs contiene la plantilla de Resource Manager para crear un clúster de Service Fabric en el [ServiceFabric clúster](https://github.com/Azure/azure-devtestlab/tree/master/Environments/ServiceFabric-LabCluster) carpeta. 

1. En primer lugar, cree un laboratorio en Azure DevTest Labs mediante el uso de instrucciones en el siguiente artículo: [Creación de un laboratorio](devtest-lab-create-lab.md). Tenga en cuenta que el **entornos públicos** opción es **en** de forma predeterminada. 
2. Confirme que el proveedor de Service Fabric está registrado para la suscripción siguiendo estos pasos:
    1. Seleccione **suscripciones** en el menú de navegación izquierdo y seleccione su **suscripción**
    2. En el **suscripción** página, seleccione **proveedores de recursos** en el **configuración** sección en el menú izquierdo. 
    3. Si **Microsoft.ServiecFabric** no está registrado, seleccione **registrar**. 
3. En la página **DevTest Lab** de su laboratorio, seleccione **+ Agregar** en la barra de herramientas. 
    
    ![Botón Agregar en la barra de herramientas](./media/create-environment-service-fabric-cluster/add-button.png)
3. En el **elegir una base** página, seleccione **clúster de Service Fabric laboratorio** en la lista. 

    ![Seleccione el clúster de Service Fabric laboratorio en la lista](./media/create-environment-service-fabric-cluster/select-service-fabric-cluster.png)
4. En el **configurar** página, realice los pasos siguientes: 
    1. Especifique un **nombre** para el clúster **entorno**. Este es el nombre del grupo de recursos de Azure en el que el clúster de Service Fabric se va a crear. 
    2. Seleccione el **del sistema operativo (SO)** para las máquinas virtuales del clúster. El valor predeterminado es: **Windows**.
    3. Especifique un nombre para el **administrador** para el clúster. 
    4. Especifique un **contraseña** para el administrador. 
    5. Para el **certificado**, escriba la información del certificado como una cadena codificada en Base64. Para crear un certificado, realice los pasos siguientes:
        1. Descargue el **crear ClusterCertificate.ps1** de archivos desde el [repositorio de Git](https://github.com/Azure/azure-devtestlab/tree/master/Environments/ServiceFabric-LabCluster). Como alternativa, clone el repositorio en su equipo. 
        2. Inicie **PowerShell**. 
        3. Ejecute el **ps1** de archivos mediante el comando `.\Create-ClusterCertificate.ps1`. Consulte un archivo de texto abierto en el Bloc de notas con la información que necesita para rellenar los campos relacionados con los certificados en esta página. . 
    6. Escriba el **contraseña del certificado**.
    7. Especifique el **huella digital** para el certificado.
    8. Seleccione **agregar** en el **configuración** página. 

        ![Configurar opciones de clúster](./media/create-environment-service-fabric-cluster/configure-settings.png)
5. Una vez creado el clúster, verá un grupo de recursos con el nombre del entorno que proporcionó en el paso anterior. Cuando se expande, verá que el clúster de Service Fabric en él. Si el estado del grupo de recursos está atascado en **crear**, seleccione **actualizar** en la barra de herramientas. El **clúster de Service Fabric** environment crea un clúster de 5 nodos 1-nodetype en Linux o Windows.

    En el ejemplo siguiente, **mysfabricclusterrg** es el nombre del grupo de recursos que se crea específicamente para el clúster de Service Fabric. Es importante tener en cuenta que los entornos de laboratorio son autónomas dentro del grupo de recursos en el que se crean. Esto significa que la plantilla que define el entorno, que solo se puede tener acceso a recursos en el grupo de recursos recién creado o [redes virtuales configuradas para ser utilizada por el laboratorio](devtest-lab-configure-vnet.md). Este ejemplo anterior crea todos los recursos necesarios en el mismo grupo de recursos.

    ![Clúster creado](./media/create-environment-service-fabric-cluster/cluster-created.png)

## <a name="start-or-stop-the-cluster"></a>Iniciar o detener el clúster
Puede iniciar o detener el clúster desde la página de DevTest Lab sí mismo o desde la página de clúster de Service Fabric proporcionada DevTest Labs. 

### <a name="from-the-devtest-lab-page"></a>En la página de DevTest Lab
Puede iniciar o detener el clúster en la página de DevTest Lab para el laboratorio. 

1. Seleccione **puntos suspensivos (...)**  para el clúster de Service Fabric tal como se muestra en la siguiente imagen: 

    ![Iniciar y detener los comandos para el clúster](./media/create-environment-service-fabric-cluster/start-stop-on-devtest-lab-page.png)

2. Consulte los dos comandos en el menú contextual para **iniciar** y **detener** el clúster. El comando start inicia todos los nodos en un clúster. El comando stop detiene todos los nodos de un clúster. Una vez que un clúster se detiene, el propio clúster de Service Fabric permanece en estado listo, pero ningún nodo está disponible hasta que se vuelve a emitir el comando de inicio en el clúster en el laboratorio.

    Hay algunas consideraciones a tener en cuenta al usar un clúster de Service Fabric en un entorno de prueba. Puede tardar algún tiempo para el clúster de Service Fabric rehidratar completamente después de haberse reiniciados los nodos. Para conservar los datos de apagado, inicio, los datos se deben guardar en un disco administrado conectado a la máquina virtual. Cuando se usa un disco administrado conectado, por lo que se recomienda para entornos de prueba solamente, hay implicaciones de rendimiento. Si el disco que se usa para el almacenamiento de datos no está respaldado, a continuación, se perderán datos cuando se emite el comando stop en el clúster.

### <a name="from-the-service-fabric-cluster-page"></a>En la página clúster de Service Fabric 
Hay otra manera de iniciar o detener el clúster. 

1. Seleccione el clúster de Service Fabric en la vista de árbol en la página de DevTest Lab. 

    ![Seleccione el clúster](./media/create-environment-service-fabric-cluster/select-cluster.png)

2. En el **clúster de Service Fabric** página para el clúster, verá que los comandos de la barra de herramientas para iniciar o detener el clúster. 

    ![Iniciar o detener los comandos en la página clúster de Service Fabric](./media/create-environment-service-fabric-cluster/start-stop-on-cluster-page.png)

## <a name="configure-auto-startup-and-auto-shutdown-schedule"></a>Configurar el inicio automático y programación de apagado automático
Clústeres de Service Fabric también pueden iniciarse o detenidos en una programación. Esta experiencia es similar a la experiencia de las máquinas virtuales en un laboratorio. Para ahorrar dinero, de forma predeterminada, cada clúster que se crean automáticamente en un laboratorio cierra al tiempo definido por el laboratorio [directiva de apagado](devtest-lab-get-started-with-lab-policies.md#set-auto-shutdown). Puede invalidar mediante la especificación de si se debe cerrar el clúster o especificando la hora en que se apaga el clúster. 

![Programaciones existentes para el inicio automático y de cierre automático](./media/create-environment-service-fabric-cluster/existing-schedules.png)

### <a name="opt-in-to-the-auto-start-schedule"></a>Participar en la programación de inicio automático
Para participar en la programación de inicio, realice los pasos siguientes:

1. Seleccione **inicio automático** en el menú izquierdo
2. Seleccione **en** para **permitir que este clúster de service fabric programar el inicio automático**. Esta página solo está habilitada si el propietario de laboratorio permite a los usuarios iniciar sus máquinas virtuales o clústeres de Service Fabric.
3. Seleccione **Guardar** en la barra de herramientas. 

    ![Página de estrella automáticas](./media/create-environment-service-fabric-cluster/set-auto-start-settings.png)

### <a name="configure-auto-shutdown-settings"></a>Configurar opciones de apagado automático 
Para cambiar la configuración de cierre, realice los pasos siguientes:

1. Seleccione **apagado automático** en el menú izquierdo. 
2. En esta página, puede rechazar apagado automático seleccionando **desactivar** para **habilitado**. 
3. Si ha seleccionado **en** para **habilitado**, siga estos pasos:
    1. Especifique el **tiempo** de apagado.
    2. Especifique el **timezone** por vez. 
    3. Especifique si desea que DevTest Labs para enviar **notificaciones** antes del apagado automático. 
    4. Si seleccionó **Sí** para la opción de notificación, especifique la **dirección URL del Webhook** o **dirección de correo electrónico** para enviar notificaciones. 
    5. Seleccione **Guardar** en la barra de herramientas.

        ![Automáticamente al cerrar la página](./media/create-environment-service-fabric-cluster/auto-shutdown-settings.png)

## <a name="to-view-nodes-in-the-service-fabric-cluster"></a>Para ver los nodos del clúster de Service Fabric
La página del clúster de Service Fabric que ha visto en los pasos anteriores es específica para la página de DevTest Labs. No muestra los nodos del clúster. Para obtener más información sobre el clúster, siga estos pasos:

1. En el **DevTest Lab** página para el laboratorio, seleccione el **grupo de recursos** en la vista de árbol en el **Mis máquinas virtuales** sección.

    ![Selección de un grupo de recursos](./media/create-environment-service-fabric-cluster/select-resource-group.png)
2. En el **grupo de recursos** página, verá todos los recursos del grupo de recursos en una lista. Seleccione su **clúster de Service Fabric** en la lista. 

    ![Seleccione el clúster en la lista](./media/create-environment-service-fabric-cluster/select-cluster-resource-group-page.png)
3. Verá el **clúster de Service Fabric** página para el clúster. Esta es la página que proporciona Service Fabric. Vea toda la información sobre los clústeres como nodos, tipos de nodo, etcetera.

    ![Página principal de clúster de Service Fabric](./media/create-environment-service-fabric-cluster/service-fabric-cluster-page.png)

## <a name="next-steps"></a>Pasos siguientes
Consulte los siguientes artículos para obtener más información acerca de los entornos: 

- [Creación de entornos de varias máquinas virtuales y recursos de PaaS con plantillas de Azure Resource Manager](devtest-lab-create-environment-from-arm.md)
- [Configurar y usar entornos públicos en Azure DevTest Labs](devtest-lab-configure-use-public-environments.md)
