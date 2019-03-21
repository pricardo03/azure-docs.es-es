---
title: Creación de un entorno de ejecución de integración autohospedado en Azure Data Factory | Microsoft Docs
description: Aprenda a crear un entorno de ejecución de integración autohospedado en Azure Data Factory, lo que permite que las factorías de datos accedan a almacenes de datos en una red privada.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/15/2019
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: 37e3dbb5f69d7319e0b56a5d209e0487e0562e00
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57838806"
---
# <a name="create-and-configure-a-self-hosted-integration-runtime"></a>Creación y configuración de un entorno de ejecución de integración autohospedado
El entorno de ejecución de integración (IR) es la infraestructura de proceso que Azure Data Factory usa para proporcionar funcionalidades de integración de datos en distintos entornos de red. Para más información acerca del entorno de ejecución de integración, consulte [Introducción al entorno de ejecución de integración](concepts-integration-runtime.md).

Un entorno de ejecución de integración autohospedado puede ejecutar actividades de copia entre un almacén de datos en la nube y un almacén de datos de una red privada, y puede distribuir actividades de transformación a los recursos de proceso de una red local o una red virtual de Azure. La instalación de un entorno de ejecución de integración autohospedado debe realizarse en un equipo local o en una máquina virtual (VM) dentro de una red privada.  

En este documento se describe cómo crear y configurar un IR autohospedado.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="high-level-steps-to-install-a-self-hosted-ir"></a>Pasos de alto nivel para instalar un IR autohospedado
1. Cree una instancia de Integration Runtime autohospedada. Para esta tarea puede usar la interfaz de usuario de Azure Data Factory. Este es un ejemplo con PowerShell:

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntimeName -Type SelfHosted -Description "selfhosted IR description"
    ```
  
2. [Descargue](https://www.microsoft.com/download/details.aspx?id=39717) e instale el entorno de ejecución de integración autohospedado en un equipo.

3. Recupere la clave de autenticación y registre el entorno de ejecución de integración autohospedado con la clave. Este es un ejemplo con PowerShell:

    ```powershell
    Get-AzDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime.  
    ```

## <a name="setting-up-a-self-hosted-ir-on-an-azure-vm-by-using-an-azure-resource-manager-template-automation"></a>Configuración de un IR autohospedado en una máquina virtual de Azure mediante una plantilla de Azure Resource Manager (automatización)
Puede automatizar la configuración del IR autohospedado en una máquina virtual de Azure mediante [esta plantilla de Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vms-with-selfhost-integration-runtime). Esta plantilla proporciona una manera sencilla de tener un IR autohospedado completamente funcional en una virtual de Azure con las características de escalabilidad y alta disponibilidad (siempre que establezca el número de nodos en 2, o más).

## <a name="command-flow-and-data-flow"></a>Flujo de comandos y flujo de datos
Cuando mueve datos entre un entorno local y la nube, la actividad utiliza un entorno de ejecución de integración autohospedado para transferir los datos desde un origen de datos local a la nube, y viceversa.

A continuación se muestra un flujo de datos de alto nivel y el resumen de los pasos para copiar con un IR autohospedado:

![Información general de alto nivel](media/create-self-hosted-integration-runtime/high-level-overview.png)

1. El desarrollador de datos crea un entorno de ejecución de integración autohospedado en una instancia de Azure Data Factory mediante un cmdlet de PowerShell. Actualmente, Azure Portal no admite esta característica.
2. El desarrollador de datos crea un servicio vinculado para un almacén de datos local mediante la especificación de la instancia del entorno de ejecución de integración autohospedado que debe usar para conectarse a los almacenes de datos. Como parte de la configuración del servicio vinculado, el desarrollador de datos usa la aplicación Credential Manager (que actualmente no es compatible) para establecer las credenciales y los tipos de autenticación. La aplicación Credential Manager se comunica con el almacén de datos para probar la conexión y con el entorno de ejecución de integración autohospedado para guardar las credenciales.
3. El nodo del entorno de ejecución de integración autohospedado cifra las credenciales mediante la interfaz de programación de aplicaciones de protección de datos de Windows (DPAPI) y las guarda localmente. Si se establecen varios nodos para la alta disponibilidad, las credenciales se sincronizan de nuevo en otros nodos. Cada nodo cifra las credenciales mediante DPAPI y las almacena localmente. La sincronización de credenciales es transparente para el desarrollador de datos y la controla el IR autohospedado.    
4. El servicio Data Factory se comunica con el entorno de ejecución de integración autohospedado para la programación y administración de trabajos a través de un *canal de control* que usa una cola de Azure Service Bus compartida. Cuando es necesario ejecutar un trabajo de actividad, Data Factory pone en cola la solicitud junto con cualquier información de credenciales (en caso de que las credenciales no estén ya almacenadas en el entorno Integration Runtime autohospedado). El entorno de ejecución de integración autohospedado inicia el trabajo después de sondear la cola.
5. El entorno de ejecución de integración autohospedado copia datos de un almacén local a un almacenamiento en la nube, o viceversa, en función de cómo esté configurada la actividad de copia en la canalización de datos. En este paso, el entorno de ejecución de integración autohospedado se comunica directamente con servicios de almacenamiento basados en la nube, como Azure Blob Storage, a través de un canal seguro (HTTPS).

## <a name="considerations-for-using-a-self-hosted-ir"></a>Consideraciones a la hora de usar un IR autohospedado

- Es posible utilizar un solo entorno de Integration Runtime autohospedado para varios orígenes de datos locales. Un entorno de ejecución de integración autohospedado individual puede compartirse con otra factoría de datos del mismo inquilino de Azure Active Directory. Para más información, consulte [Uso compartido de un entorno de ejecución de integración autohospedado](#sharing-the-self-hosted-integration-runtime-with-multiple-data-factories).
- En cada equipo no puede haber más de una instancia del entorno de ejecución de integración autohospedado instalada. Si tiene dos factorías de datos que necesitan tener acceso a orígenes de datos locales, deberá instalar el entorno integration runtime autohospedado en dos equipos de un entorno local por cada desde ambos las factorías de datos o usar el [autohospedadodelacaracterísticadeusocompartido](#sharing-the-self-hosted-integration-runtime-with-multiple-data-factories)para compartir un entorno integration runtime autohospedado con otra factoría de datos.  
- No es preciso que el entorno de ejecución de integración autohospedado se encuentre en la misma máquina que el origen de datos. Sin embargo, cuanto más cerca estén ambos, menos tiempo necesitará el primero para conectarse al segundo. Le recomendamos que instale el entorno Integration Runtime autohospedado en una máquina diferente de la que hospeda el origen de datos local. Si el entorno de ejecución de integración autohospedado y el origen de datos están en equipos diferentes, el primero no compite por recursos con el segundo.
- Puede tener varios entornos de ejecución de integración autohospedados en diferentes equipos que se conecten al mismo origen de datos local. Por ejemplo, puede tener dos entornos de ejecución de integración autohospedados que sirvan a dos factorías de datos, pero el mismo origen de datos local puede estar registrado en ambas factorías de datos.
- Si ya tiene una puerta de enlace instalada en el equipo que sirve a un escenario de Power BI, instale un entorno de ejecución de integración autohospedado independiente para Azure Data Factory en otro equipo.
- El entorno de ejecución de integración autohospedado debe usarse para admitir la integración de datos en una red virtual de Azure.
- Considere el origen de datos como un origen de datos local, que está detrás de un firewall, incluso cuando use Azure ExpressRoute. Use el entorno de ejecución de integración autohospedado para establecer la conectividad entre el servicio y el origen de datos.
- El entorno de ejecución de integración autohospedado se debe usar aunque el almacén de datos esté en la nube en una máquina virtual IaaS de Azure.
- Las tareas pueden generar error en un entorno de ejecución de integración autohospedado que esté instalado en un equipo con Windows Server en el que está habilitado el cifrado compatible con FIPS. Para solucionar este problema, deshabilite el cifrado compatible con FIPS en el servidor. Para deshabilitar el cifrado compatible con FIPS, cambie el valor del Registro siguiente de 1 (habilitado) a 0 (deshabilitado): `HKLM\System\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy\Enabled`.

## <a name="prerequisites"></a>Requisitos previos

- Las versiones de sistema operativo compatibles son: Windows 7 Service Pack 1, Windows 8.1, Windows 10, Windows Server 2008 R2 SP1, Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016. No se admite la instalación del entorno de ejecución de integración autohospedado en un controlador de dominio.
- Se requiere .NET Framework 4.6.1, o cualquier versión posterior. Si va a instalar el entorno de ejecución de integración autohospedado en un equipo con Windows 7, instale .NET Framework 4.6.1, o cualquier versión posterior. Consulte [Requisitos de sistema de .NET Framework](/dotnet/framework/get-started/system-requirements) para más información.
- La configuración recomendada del equipo del entorno de ejecución de integración autohospedado es de, al menos, 2 GHz, 4 núcleos, 8 GB de RAM y un disco de 80 GB.
- Si la máquina host está en hibernación, el entorno de ejecución de integración autohospedado no responde a las solicitudes de datos. Configure un plan de energía adecuado en el equipo antes de instalar el entorno de ejecución de integración autohospedado. Si la máquina está configurada para hibernar, se mostrará un mensaje durante la instalación del entorno Integration Runtime autohospedado.
- Debe ser administrador del equipo para instalar y configurar correctamente el entorno de ejecución de integración autohospedado.
- Las ejecuciones de la actividad de copia se producen con una frecuencia concreta. El uso de los recursos (CPU, memoria) en el equipo sigue el mismo patrón en los momentos de máxima actividad y en los de inactividad. El uso de recursos también depende en gran medida de la cantidad de datos que se mueven. Cuando hay varios trabajos de copia en curso, puede ver que el uso de los recursos aumenta durante las horas pico.

## <a name="installation-best-practices"></a>Procedimientos recomendados de instalación
Para instalar el entorno de ejecución de integración autohospedado, descargue un paquete de instalación MSI del [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=39717). Consulte el artículo [Movimiento de datos entre orígenes locales y la nube con Data Management Gateway](tutorial-hybrid-copy-powershell.md) para obtener instrucciones detalladas.

- Configure un plan de energía en el equipo host para el entorno de ejecución de integración autohospedado, con el fin de que el equipo no hiberne. Si el equipo host hiberna, el entorno de ejecución de integración autohospedado se pone en modo sin conexión.
- Realice regularmente una copia de las credenciales asociadas con el entorno Integration Runtime autohospedado.

## <a name="install-and-register-self-hosted-ir-from-the-download-center"></a>Instalación y registro de IR autohospedado desde el Centro de descarga

1. Vaya a la [página de descarga del entorno de ejecución de integración Microsoft](https://www.microsoft.com/download/details.aspx?id=39717).
2. Seleccione **Descargar**, haga clic en la versión de 64 bits (no se admite la de 32 bits) y, finalmente, seleccione **Siguiente**.
3. Ejecute el archivo MSI directamente o guárdelo en el disco duro para ejecutarlo más adelante.
4. En la página **principal**, seleccione un idioma y, después, seleccione **Siguiente**.
5. Acepte los términos de licencia del software de Microsoft y seleccione **Siguiente**.
6. Seleccione la **carpeta** en la que se va a instalar el entorno de ejecución de integración autohospedado y seleccione **Siguiente**.
7. En la página **Preparado para instalar**, seleccione **Instalar**.
8. Haga clic en **Finalizar** para completar la instalación.
9. Para obtener la clave de autenticación use Azure PowerShell. Este es un ejemplo de PowerShell para recuperar la clave de autenticación:

    ```powershell
    Get-AzDataFactoryV2IntegrationRuntimeKey -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Name $selfHostedIntegrationRuntime
    ```
11. En la página **Registro de Integration Runtime (autohospedado)** de Microsoft Integration Runtime Configuration Manager que se ejecuta en el equipo, siga estos pasos:

     a. Pegue la clave de autenticación en el área de texto.

    b. Si lo desea, seleccione **Show authentication key** (Mostrar clave de autenticación) para ver el texto de la clave.

    c. Seleccione **Registrar**.


## <a name="high-availability-and-scalability"></a>Alta disponibilidad y escalabilidad
Un entorno de ejecución de integración autohospedado puede asociarse con varios equipos locales. Estas máquinas se llaman nodos. Puede tener hasta cuatro nodos asociados con un entorno de ejecución de integración autohospedado. Las ventajas de tener varios nodos (equipos locales con una puerta de enlace instalada) para una puerta de enlace lógica son:
* Mayor disponibilidad del entorno de ejecución de integración autohospedado para que deje de ser el único punto de error de la solución de Big Data o la integración de datos en la nube con Azure Data Factory, lo que garantiza la continuidad con un máximo de cuatro nodos.
* Rendimiento mejorado durante el movimiento de datos entre almacenes de datos locales y en la nube. Obtenga más información sobre las [comparaciones de rendimiento](copy-activity-performance.md).

Puede asociar varios nodos mediante la instalación del software del entorno de ejecución de integración autohospedado desde el [Centro de descarga](https://www.microsoft.com/download/details.aspx?id=39717). A continuación, regístrese mediante cualquiera de las claves de autenticación obtenido de la **New AzDataFactoryV2IntegrationRuntimeKey** cmdlet, como se describe en el [tutorial](tutorial-hybrid-copy-powershell.md).

> [!NOTE]
> No es preciso crear un nuevo entorno de ejecución de integración autohospedado para asociar cada nodo. Puede instalar el entorno de ejecución de integración autohospedado en otro equipo y registrarlo con la misma clave de autenticación. 

> [!NOTE]
> Antes de agregar otro nodo para aumentar la disponibilidad y escalabilidad, asegúrese de que la opción **Remote access to intranet** (Acceso remoto a la intranet) está habilitada en el primer nodo [**Microsoft Integration Runtime Configuration Manager** > **Configuración** > **Remote access to intranet** (Acceso remoto a la intranet)]. 

### <a name="scale-considerations"></a>Consideraciones de escala

#### <a name="scale-out"></a>Escalado horizontal

Cuando queda poca memoria disponible en el IR autohospedado y el uso de la CPU es alto, la incorporación de un nuevo nodo ayuda a escalar horizontalmente la carga en las máquinas. Si se producen errores en las actividades porque se agota su tiempo de espera o porque el nodo del IR autohospedado está sin conexión, será útil agregar un nodo a la puerta de enlace.

#### <a name="scale-up"></a>Escalado vertical

Cuando la memoria disponible y la CPU no se utilizan correctamente, pero la ejecución de trabajos simultáneos está alcanzando el límite, debe realizar un escalado vertical aumentando el número de trabajos simultáneos que se pueden ejecutar en un nodo. También puede realizar el escalado verticalmente cuando se agota el tiempo de espera de las actividades porque el IR autohospedado está sobrecargado. Como se muestra en la siguiente imagen, puede aumentar la capacidad máxima de los nodos:  

![Aumento del número de trabajos simultáneos que se pueden ejecutar en un nodo](media/create-self-hosted-integration-runtime/scale-up-self-hosted-IR.png)

### <a name="tlsssl-certificate-requirements"></a>Requisitos del certificado TLS/SSL

Estos son los requisitos para el certificado TLS/SSL que se usa para proteger las comunicaciones entre los nodos del entorno de ejecución de integración:

- El certificado debe ser un certificado de confianza pública X509 v3. Se recomienda utilizar los certificados que emita alguna entidad de certificación (asociado) pública (CA).
- Todos los nodos del entorno de ejecución de integración deben confiar en este certificado.
- No se recomienda usar certificados del nombre alternativo del firmante, ya que solo se utilizará el último elemento de SAN, mientras que los restantes se ignorarán debido a las limitaciones actuales. Por ejemplo, si tiene un certificado de SAN cuyos nombres alternativos del firmante son **node1.domain.contoso.com** y **node2.domain.contoso.com**, puede usarlo en el equipo cuyo FQDN es **node2.domain.contoso.com**.
- El certificado admite cualquier tamaño de clave compatible con Windows Server 2012 R2 para los certificados SSL.
- No se admiten certificados que utilicen claves CNG.  

> [!NOTE]
> Este certificado se usa para cifrar los puertos en el nodo de Integration Runtime autohospedado, utilizado para **comunicación de nodo a nodo** (para sincronización de estado, que incluye los servicios vinculados credenciales sincronización entre los nodos) y while **mediante PowerShell cmdlet para el servicio vinculado de configuración de credenciales** desde dentro de la red local. Es conveniente usar este certificado si el entorno de la red privada no es seguro o si desea proteger la comunicación entre nodos también en la red privada. El movimiento de los datos en tránsito desde el IR autohospedado y otros almacenes de datos siempre tiene lugar en un canal privado, con independencia de si este certificado está configurado o no. 

## <a name="sharing-the-self-hosted-integration-runtime-with-multiple-data-factories"></a>Uso compartido del entorno de ejecución de integración autohospedado con varias factorías de datos

Puede volver a usar una infraestructura de entorno de ejecución de integración autohospedado existente que ya ha configurado en una factoría de datos. Esto le permite crear un *entorno de ejecución de integración autohospedado vinculado* en una factoría de datos diferente haciendo referencia a otro entorno de ejecución de IR autohospedado existente (compartido).

Para compartir un entorno de ejecución de integración autohospedado mediante PowerShell, consulte [Create a shared self-hosted integration runtime in Azure Data Factory with PowerShell](create-shared-self-hosted-integration-runtime-powershell.md) (Creación de un entorno de ejecución de integración autohospedado compartido en Azure Data Factory con PowerShell).

Si desea ver una demostración y una introducción de doce minutos de esta característica, vea el siguiente vídeo:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Hybrid-data-movement-across-multiple-Azure-Data-Factories/player]

### <a name="terminology"></a>Terminología

- **Entorno de ejecución de integración compartido**: el IR autohospedado original que se ejecuta en una infraestructura física.  
- **Entorno de ejecución de integración vinculado**: el entorno de ejecución de integración que hace referencia a otro entorno compartido. Este es un entorno de ejecución de integración lógico que usa la infraestructura de otro entorno de ejecución de integración autohospedado (compartido).

### <a name="high-level-steps-for-creating-a-linked-self-hosted-ir"></a>Pasos de alto nivel para la creación de un IR autohospedado vinculado

1. En el IR autohospedado que se va a compartir, conceda permiso a la factoría de datos en la que desea crear el entorno de ejecución de integración vinculado. 

   ![El botón para conceder el permiso en la pestaña Compartir](media/create-self-hosted-integration-runtime/grant-permissions-IR-sharing.png)

   ![Selecciones para asignar permisos](media/create-self-hosted-integration-runtime/3_rbac_permissions.png)

2. Anote el identificador de recurso del IR autohospedado que se va a compartir.

   ![Ubicación del identificador de recurso](media/create-self-hosted-integration-runtime/4_ResourceID_self-hostedIR.png)

3. En la factoría de datos en la que se concedieron los permisos, cree un nuevo IR autohospedado (vinculado) y escriba el identificador de recurso.

   ![Botón para crear un entorno de ejecución de integración autohospedado vinculado](media/create-self-hosted-integration-runtime/6_create-linkedIR_2.png)

   ![Cuadros para nombre y el identificador de recurso](media/create-self-hosted-integration-runtime/6_create-linkedIR_3.png)

### <a name="monitoring"></a>Supervisión 

- **IR compartido**

  ![Opciones para buscar un entorno de ejecución de integración compartido](media/create-self-hosted-integration-runtime/Contoso-shared-IR.png)

  ![Pestaña para la supervisión](media/create-self-hosted-integration-runtime/contoso-shared-ir-monitoring.png)

- **IR vinculado**

  ![Opciones para buscar un entorno de ejecución de integración vinculado](media/create-self-hosted-integration-runtime/Contoso-linked-ir.png)

  ![Pestaña para la supervisión](media/create-self-hosted-integration-runtime/Contoso-linked-ir-monitoring.png)

### <a name="known-limitations-of-self-hosted-ir-sharing"></a>Limitaciones conocidas del uso compartido de un entorno de ejecución de integración autohospedado

* La factoría de datos en la que se creará un entorno de ejecución de integración vinculado debe tener una instancia de [MSI](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview). De forma predeterminada, las factorías de datos que se crean en Azure Portal o mediante los cmdlets de PowerShell incluyen una instancia de MSI creada implícitamente. Pero cuando se crea una factoría de datos mediante una plantilla de Azure Resource Manager o un SDK, la propiedad **Identity** se debe establecer explícitamente para asegurarse de que Azure Resource Manager crea una factoría de datos que contiene una instancia de MSI. 

* El SDK de .NET de Azure Data Factory que admite esta característica es la versión 1.1.0 o posterior.

* Para conceder permiso, el usuario necesitará el rol propietario o el rol propietario heredado en la factoría de datos en la que exista el entorno de ejecución de integración compartido.

* La característica de uso compartido solo funciona para las instancias de Data Factory dentro del mismo inquilino de Azure Active Directory.

* Para los [usuarios invitados](https://docs.microsoft.com/azure/active-directory/governance/manage-guest-access-with-access-reviews) de Active Directory, la funcionalidad de búsqueda (lista de todas las factorías de datos mediante el uso de una palabra clave de búsqueda) en la interfaz de usuario [no funciona](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#SearchLimits). Pero siempre y cuando el usuario invitado sea el propietario de la factoría de datos, puede compartir el entorno de ejecución de integración sin la funcionalidad de búsqueda. Para ello, debe escribir directamente la instancia de MSI de la factoría de datos con el que el entorno de ejecución de integración debe compartirse en el cuadro de texto**Asignar permiso** y seleccionar **Agregar** en la interfaz de usuario de Azure Data Factory. 

  > [!NOTE]
  > Esta característica solo está disponible en la versión 2 de Azure Data Factory. 

## <a name="notification-area-icons-and-notifications"></a>Iconos y notificaciones del área de notificación

Si mueve el cursor sobre el icono o el mensaje en el área de notificación, podrá ver los detalles del estado del entorno de ejecución de integración autohospedado.

![Notificaciones en el área de notificación](media/create-self-hosted-integration-runtime/system-tray-notifications.png)

## <a name="ports-and-firewall"></a>Puertos y firewall
Hay dos firewalls que deben tenerse en cuenta: el *firewall corporativo*, que se ejecuta en el enrutador central de la organización, y el *firewall de Windows*, que se configura como un demonio en el equipo local en la que está instalado el entorno de ejecución de integración autohospedado.

![Firewall](media/create-self-hosted-integration-runtime/firewall.png)

A nivel de *firewall corporativo*, es preciso configurar los siguientes dominios y puertos de salida:

Nombres de dominio | Puertos | DESCRIPCIÓN
------------ | ----- | ------------
*.servicebus.windows.net | 443 | Se usa para la comunicación con el servicio de movimiento de datos del back-end
*.core.windows.net | 443 | Usado para la copia almacenada provisionalmente a través de Azure Blob Storage (si está configurado)
*.frontend.clouddatahub.net | 443 | Se usa para la comunicación con el servicio de movimiento de datos del back-end
download.Microsoft.com | 443 | Usado para descargar las actualizaciones

En el nivel del *firewall de Windows* (nivel de máquina), normalmente estos puertos de salida están habilitados. Si no lo están, puede configurar los puertos y los dominios según corresponda en el equipo del entorno de ejecución de integración autohospedado.

> [!NOTE]
> En función del origen y los receptores, es posible que tenga que agregar a la lista blanca más dominios y puertos salientes al firewall corporativo o firewall de Windows.
>
> Para algunas bases de datos en la nube (por ejemplo: Azure SQL Database, y Azure Data Lake), es posible que necesite agregar a la lista blanca las direcciones IP de los equipos del entorno de ejecución de integración autohospedado en la configuración de su firewall.

### <a name="copy-data-from-a-source-to-a-sink"></a>Copia de datos desde un origen a un receptor
Asegúrese de que las reglas del firewall se han habilitado correctamente en el firewall corporativo, en el firewall de Windows de la equipo del entorno de ejecución de integración autohospedado y en el propio almacén de datos. De este modo, el entorno de ejecución de integración autohospedado podrá conectarse al origen y al receptor correctamente. Habilite las reglas de cada almacén de datos que participe en la operación de copia.

Por ejemplo, para copiar información desde un almacén de datos local a un receptor de Azure SQL Database o de Azure SQL Data Warehouse, siga estos pasos:

1. Permita la comunicación TCP saliente en el puerto 1433 tanto para el firewall corporativo como para el firewall de Windows.
2. Configure los valores del firewall de Azure SQL Database para agregar la dirección IP del equipo del entorno de ejecución de integración autohospedado a la lista de direcciones IP permitidas.

> [!NOTE]
> Si el firewall no permite el puerto de salida 1433, el entorno de ejecución de integración autohospedado no podrá acceder directamente a Azure SQL Database. En este caso, puede usar un [copia almacenada provisionalmente](copy-activity-performance.md) en Azure SQL Database y Azure SQL Data Warehouse. En este escenario, requeriría solo HTTPS (puerto 443) para el movimiento de datos.


## <a name="proxy-server-considerations"></a>Consideraciones acerca del servidor proxy
Si en su entorno de red corporativo se usa un servidor proxy para acceder a Internet, configure el entorno de ejecución de integración autohospedado para utilizar la configuración del proxy adecuada. Puede establecer el proxy durante la fase de registro inicial.

![Especificación del servidor proxy](media/create-self-hosted-integration-runtime/specify-proxy.png)

El entorno de ejecución de integración autohospedado utiliza el servidor proxy para conectarse al servicio en la nube. Seleccione **Cambiar vínculo** durante la configuración inicial. Verá el cuadro de diálogo de configuración del proxy.

![Establecimiento de proxy](media/create-self-hosted-integration-runtime/set-http-proxy.png)

Hay tres opciones de configuración:

- **No utilizar proxy**: el entorno de ejecución de integración autohospedado no usa expresamente ningún proxy para conectarse a servicios en la nube.
- **Usar proxy del sistema**: el entorno Integration Runtime autohospedado utiliza la configuración de proxy de diahost.exe.config y diawp.exe.config. Si no hay ningún proxy configurado en diahost.exe.config y diawp.exe.config, el entorno de ejecución de integración autohospedado se conecta al servicio en la nube directamente sin pasar por ningún proxy.
- **Usar proxy personalizado**: establezca la configuración del proxy HTTP que se utilizará para el entorno de ejecución de integración autohospedado, en lugar de usar las configuraciones de diahost.exe.config y diawp.exe.config. Se requieren los valores de **Dirección** y **Puerto**. Los valores de **Nombre de usuario** y **Contraseña** son opcionales, dependen de la configuración de la autenticación del proxy. Todas las configuraciones se cifran con DPAPI de Windows en el entorno de ejecución de integración autohospedado y se almacenan localmente en el equipo.

El servicio host de Integration Runtime se reinicia automáticamente después de guardar la configuración de proxy actualizada.

Cuando el entorno de ejecución de integración autohospedado se haya registrado correctamente, si desea ver o actualizar la configuración de proxy, utilice Integration Runtime Configuration Manager.

1. Inicie el **Administrador de configuración de Microsoft Integration Runtime**.
2. Cambie a la pestaña **Configuración** .
3. Seleccione el vínculo **Cambiar** en la sección **Proxy HTTP** para abrir el cuadro de diálogo **Establecer proxy HTTP**.
4. Seleccione **Next** (Siguiente). En ese momento ve una advertencia en la que se le pide permiso la configuración del proxy y reinicie el servicio de host del entorno de ejecución de integración.

Puede ver y actualizar el proxy HTTP mediante la herramienta Configuration Manager.

![Visualización de proxy](media/create-self-hosted-integration-runtime/view-proxy.png)

> [!NOTE]
> Si configura un servidor proxy con la autenticación NTLM, el servicio de host del entorno de ejecución de integración se ejecutará en la cuenta del dominio. Si más adelante cambia la contraseña de dicha cuenta, no olvide actualizar la configuración del servicio y reiniciarlo en consecuencia. Debido a este requisito, se recomienda usar una cuenta de dominio dedicada para acceder al servidor proxy, una cuenta que no requiera que se actualice la contraseña con frecuencia.

### <a name="configure-proxy-server-settings"></a>Configuración de un servidor proxy

Si selecciona el valor **Use system proxy** (Usar proxy del sistema) para el proxy HTTP, el entorno de ejecución de integración autohospedado utilizará la configuración de proxy de diahost.exe.config y diawp.exe.config. Si no hay ningún proxy especificado en diahost.exe.config y diawp.exe.config, el entorno de ejecución de integración autohospedado se conectará al servicio en la nube directamente, sin pasar por el proxy. En el procedimiento siguiente se proporcionan instrucciones para actualizar el archivo diahost.exe.config:

1. En el Explorador de archivos, cree una copia segura de C:\Archivos de programa\Microsoft Integration Runtime\3.0\Shared\diahost.exe.config para realizar una copia de seguridad del archivo original.
2. Abra Notepad.exe como administrador y abra el archivo de texto C:\Archivos de programa\Microsoft Integration Runtime\3.0\Shared\diahost.exe.config. Encuentre la etiqueta predeterminada de system.net como se muestra en el siguiente código:

    ```xml
    <system.net>
        <defaultProxy useDefaultCredentials="true" />
    </system.net>
    ```
    Después, puede agregar los detalles del servidor proxy tal y como se muestran en el ejemplo siguiente:

    ```xml
    <system.net>
        <defaultProxy enabled="true">
              <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
        </defaultProxy>
    </system.net>
    ```

    Se permiten más propiedades en la etiqueta de proxy para especificar la configuración requerida, como `scriptLocation`. Consulte [proxy (Elemento, Configuración de red)](https://msdn.microsoft.com/library/sa91de1e.aspx) para ver la sintaxis.

    ```xml
    <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
    ```
3. Guarde el archivo de configuración en la ubicación original. Luego reinicie el servicio de host del entorno de ejecución de integración autohospedado, lo que aplica los cambios. 

   Para reiniciar el servicio, use el applet de servicios del panel de control. O bien en Integration Runtime Configuration Manager, seleccione el botón **Detener servicio** y, después, seleccione **Iniciar servicio**. 
   
   Si el servicio no se inicia, es probable que se deba a que se ha agregado una sintaxis de etiqueta XML incorrecta en el archivo de configuración de la aplicación que se ha editado.

> [!IMPORTANT]
> No olvide actualizar tanto diahost.exe.config como diawp.exe.config.

También tiene que asegurarse de que Microsoft Azure se encuentra en la lista blanca de su compañía. La lista de direcciones IP válidas de Microsoft Azure se puede descargar del [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Posibles síntomas de problemas relacionados con el firewall y el servidor proxy
Si se producen errores como los siguientes, es probable que se deban a una configuración incorrecta del servidor proxy o del firewall, lo que impide que el entorno de ejecución de integración autohospedado se conecte a Data Factory para autenticarse. Para asegurarse de que tanto el firewall como el servidor proxy están configurados correctamente, consulte la sección anterior.

* Al intentar registrar el entorno de ejecución de integración autohospedado, recibe el siguiente error: "No se pudo registrar este nodo de Integration Runtime. Confirme que la clave de autenticación es válida y que el servicio host del servicio de integración se ejecuta en este equipo.
* Al abrir Integration Runtime Configuration Manager, se ve uno de estos dos estados, **Desconectado** o **Conectando**. Cuando se ven los registros de eventos de Windows, en **Visor de eventos** > **Registros de aplicaciones y servicios** > **Microsoft Integration Runtime** aparecen mensajes de error como este:

    ```
    Unable to connect to the remote server
    A component of Integration Runtime has become unresponsive and restarts automatically. Component name: Integration Runtime (Self-hosted).
    ```

### <a name="enabling-remote-access-from-an-intranet"></a>Habilitación del acceso remoto desde una intranet  
Si utiliza PowerShell o la aplicación Administrador de credenciales para cifrar credenciales desde una máquina (de la red) que no sea la que tiene instalado el entorno de ejecución de integración autohospedado, puede habilitar la opción **Acceso remoto desde la intranet**. Si utiliza PowerShell o la aplicación Administrador de credenciales para cifrar credenciales en la misma máquina (de la red) en la que esté instalado el entorno de ejecución de integración autohospedado, no podrá habilitar **Acceso remoto desde la intranet**.

Debe habilitar **Acceso remoto desde la intranet** para agregar otro nodo para que tanto la disponibilidad como la escalabilidad sean altas.  

Durante la instalación del entorno de ejecución de integración autohospedado (versión 3.3.xxxx.x o posterior), de forma predeterminada, el entorno de ejecución de integración autohospedado deshabilita la opción **Acceso remoto desde la intranet** en el equipo de dicho entorno.

Si usa un firewall de terceros, puede abrir manualmente el puerto 8060 (o el puerto configurado por el usuario). Si se presenta un problema de firewall durante la configuración del entorno de ejecución de integración autohospedado, pruebe a usar el comando siguiente para instalarlo sin configurar el firewall:

```
msiexec /q /i IntegrationRuntime.msi NOFIREWALL=1
```
> [!NOTE]
> La aplicación Administrador de credenciales aún no está disponible para cifrar credenciales en Azure Data Factory V2.  

Si elige no abrir el puerto 8060 en la máquina del entorno de ejecución de integración autohospedado, use mecanismos que no sean la aplicación de Establecer credenciales para configurar las credenciales del almacén de datos. Por ejemplo, puede usar el **New AzDataFactoryV2LinkedServiceEncryptCredential** cmdlet de PowerShell.


## <a name="next-steps"></a>Pasos siguientes
Consulte el siguiente tutorial para obtener instrucciones detalladas: [Tutorial: Copia de datos locales en la nube](tutorial-hybrid-copy-powershell.md).
