---
title: Agrupación de máquinas en Azure Migrate mediante la visualización de dependencias sin agentes
description: Describe cómo crear grupos mediante dependencias de máquina sin agente.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/18/2019
ms.author: hamusa
ms.openlocfilehash: ff267aabe2f9e4cec38c307fe4382a84ba6d62df
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288969"
---
# <a name="set-up-agentless-dependency-visualization-for-assessment"></a>Configuración de la visualización de dependencias sin agente para la evaluación

En este artículo se describe cómo configurar la asignación de dependencias sin agente en Azure Migrate: Server Assessment. 

> [!IMPORTANT]
> La visualización de dependencias sin agente se encuentra actualmente en versión preliminar para las VM de Azure VMware detectadas mediante un dispositivo con Azure Migrate.
> Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Esta versión preliminar está incluida en el soporte al cliente y se puede usar para cargas de trabajo de producción.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="about-dependency-mapping"></a>Acerca de la asignación de dependencias

La asignación de dependencias le permite visualizar las dependencias en las máquinas que quiere evaluar y migrar. La asignación de dependencias suele usarse cuando se quiere evaluar máquinas con niveles de confianza más altos.

- En Azure Migrate: Server Assessment, puede reunir las máquinas en grupos para evaluarlas. Normalmente, los grupos se componen de las máquinas que desea migrar, y la asignación de dependencias ayuda a comprobar las dependencias de las máquinas para que pueda agrupar las máquinas con precisión.
- Con la asignación, puede detectar los sistemas interdependientes que necesitan migrarse juntos. También puede identificar si un sistema en ejecución sigue ofreciendo servicio a los usuarios o si es un candidato para la retirada en lugar de la migración.
- La visualización de dependencias le ayuda a garantizar que no se olvida de nada y a evitar interrupciones inesperadas durante la migración.

## <a name="about-agentless-visualization"></a>Acerca de la visualización sin agente

La visualización de dependencias sin agente no requiere la instalación de ningún agente en las máquinas. Funciona capturando los datos de conexión TCP de las máquinas para las que está habilitada.

- Una vez iniciada la detección de dependencias, el dispositivo recopila datos de las máquinas en un intervalo de sondeo de cinco minutos.
- Se pueden recopilar los siguientes datos:
    - Conexiones TCP
    - Nombres de los procesos que tienen conexiones activas
    - Nombres de las aplicaciones instaladas que ejecutan los procesos anteriores
    - No. de conexiones detectadas en cada intervalo de sondeo

## <a name="current-limitations"></a>Limitaciones actuales

- La visualización de dependencias sin agente está disponible actualmente solo para las VM de VMware.
- En este momento, no se puede agregar un servidor a un grupo ni quitarlo de este en la vista de análisis de dependencias.
- Actualmente, no existe ningún mapa de dependencias disponible para un grupo de servidores.
- Actualmente, los datos de dependencias no se pueden descargar en formato tabular.

## <a name="before-you-start"></a>Antes de comenzar

- Asegúrese de que ha [creado](how-to-add-tool-first-time.md) un proyecto de Azure Migrate.
- El análisis de dependencias sin agente está actualmente disponible solo para máquinas de VMware.
- Si ya ha creado un proyecto, asegúrese de que ha [agregado](how-to-assess.md) la herramienta Azure Migrate: Server Assessment.
- Compruebe que ha detectado sus máquinas de VMware en Azure Migrate; para hacerlo, configure un dispositivo de Azure Migrate para [VMware](how-to-set-up-appliance-vmware.md). El dispositivo detecta máquinas locales y envía metadatos y datos de rendimiento a Azure Migrate: Server Assessment. [Más información](migrate-appliance.md).
- [Revise los requisitos](migrate-support-matrix-vmware.md#agentless-dependency-visualization) para configurar la visualización de dependencias sin agente.



## <a name="create-a-user-account-for-discovery"></a>Creación de una cuenta de usuario para la detección

Configure una cuenta de usuario que tenga los permisos necesarios para que Server Assessment pueda acceder a la VM para la detección. Puede especificar una cuenta de usuario.

- **Permiso necesario en las VM de Windows**: la cuenta de usuario requiere el permiso de acceso "Invitado".
- **Permiso necesario en las VM de Linux**: se requiere el privilegio raíz en la cuenta. Como alternativa, la cuenta de usuario requiere estas dos funcionalidades en los archivos/bin/netstat y /bin/ls: CAP_DAC_READ_SEARCH y CAP_SYS_PTRACE.

## <a name="add-the-user-account-to-the-appliance"></a>Adición de la cuenta de usuario al dispositivo

Debe agregar la cuenta de usuario al dispositivo.

Agregue la cuenta como se indica a continuación:

1. Abra la aplicación de administración del dispositivo. Navegue hasta el panel **Provide vCenter details** (Proporcionar detalles de vCenter).
2. En **Discover applications and dependencies on VMs** (Detectar aplicaciones y dependencias en VM) haga clic en **Add credentials** (Agregar credenciales).
3. Elija la versión de **Sistema operativo**.
4. Proporcione un nombre descriptivo para la cuenta.
5. Rellene los campos **Nombre de usuario** y **Contraseña**.
6. Haga clic en **Save**(Guardar).
7. Haga clic en **Guardar e iniciar la detección**.

    ![Adición de una cuenta de usuario de VM](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)

## <a name="start-dependency-discovery"></a>Inicio de la detección de dependencias

Elija las máquinas en las que quiere habilitar la detección de dependencias.

1. En **Azure Migrate: Server Assessment**, haga clic en **Servidores detectados**.
2. Haga clic en el icono **Análisis de dependencias**.
3. Haga clic en **Agregar servidores**.
3. En la página **Agregar servidores**, elija el dispositivo que detecta las máquinas pertinentes.
4. En la lista de máquinas, seleccione las máquinas.
5. Haga clic en **Agregar servidores**.

    ![Inicio de la detección de dependencias](./media/how-to-create-group-machine-dependencies-agentless/start-dependency-discovery.png)

Podrá visualizar las dependencias 6 horas después de iniciar la detección de dependencias.

## <a name="visualize-dependencies"></a>Visualización de dependencias

1. En **Azure Migrate: Server Assessment**, haga clic en **Servidores detectados**.
2. Busque la máquina de la que quiere ver el mapa de dependencias.
3. Haga clic en **Ver dependencias** en la columna **Dependencias**.
4. Cambie el período de tiempo del que quiere ver el mapa mediante el menú desplegable **Duración**.
5. Expanda el grupo **Cliente** para mostrar las máquinas que tienen una dependencia en la máquina seleccionada.
6. Expanda el grupo **Puerto** para mostrar las máquinas que tienen una dependencia de la máquina seleccionada.
7. Para navegar a la vista de mapa de cualquiera de las máquinas dependientes, haga clic en el nombre del equipo y, a continuación, haga clic en **Cargar mapa del servidor**.

    ![Expansión del grupo de puertos del servidor y carga del mapa del servidor](./media/how-to-create-group-machine-dependencies-agentless/load-server-map.png)

    ![Expansión del grupo de clientes ](./media/how-to-create-group-machine-dependencies-agentless/expand-client-group.png)

8. Expanda la máquina seleccionada para ver los detalles de nivel de proceso de cada dependencia.

    ![Expansión del servidor para mostrar los procesos](./media/how-to-create-group-machine-dependencies-agentless/expand-server-processes.png)

> [!NOTE]
> La información del proceso para una dependencia no siempre está disponible. Si no está disponible, la dependencia se representa con el proceso marcado como "Proceso desconocido".

## <a name="stop-dependency-discovery"></a>Detención de la detección de dependencias

Elija las máquinas en las que quiere detener la detección de dependencias.

1. En **Azure Migrate: Server Assessment**, haga clic en **Servidores detectados**.
2. Haga clic en el icono **Análisis de dependencias**.
3. Haga clic en **Quitar servidores**.
3. En la página **Quitar servidores**, elija el **dispositivo** que está detectando las VM en las que quiere detener la detección de dependencias.
4. En la lista de máquinas, seleccione las máquinas.
5. Haga clic en **Quitar servidores**.


## <a name="next-steps"></a>Pasos siguientes

[Agrupación de las máquinas](how-to-create-a-group.md)
