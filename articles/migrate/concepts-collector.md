---
title: Dispositivo del recopilador de Azure Migrate | Microsoft Docs
description: Proporciona información sobre el dispositivo del recopilador de Azure Migrate.
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 02/04/2019
ms.author: snehaa
services: azure-migrate
ms.openlocfilehash: 0568df92db2114c57a0aa027ade369e4b256af84
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55813337"
---
# <a name="about-the-collector-appliance"></a>Dispositivo del recopilador

 En este artículo se proporciona información sobre Azure Migrate Collector.

Azure Migrate Collector es un dispositivo ligero que sirve para detectar un entorno de vCenter local a efectos de evaluación con el servicio [Azure Migrate](migrate-overview.md) antes de realizar la migración a Azure.  

## <a name="discovery-method"></a>Método de detección

Anteriormente, el dispositivo del recopilador podía usar dos opciones: detección de una sola vez o detección continua. El modelo de detección de una sola vez está en desuso, ya que se basaba en la configuración de las estadísticas de vCenter Server para la recopilación de datos de rendimiento (requería que la configuración de las estadísticas se estableciese en el nivel 3) y también recopilaba los contadores promedio (en lugar del máximo), lo que daba lugar a un cálculo inferior. El modelo de detección continua garantiza la recopilación de datos pormenorizados y da como resultado un tamaño preciso, debido a la recopilación de contadores de máximo. Así funciona:

El dispositivo del recopilador está conectado constantemente al proyecto de Azure Migrate y recopila continuamente datos de rendimiento de las VM.

- El recopilador realiza un perfil del entorno local continuamente para recopilar datos de uso en tiempo real cada 20 segundos.
- El dispositivo acumula ejemplos de 20 segundos y crea un único punto de datos cada 15 minutos.
- Para crear el punto de datos, el dispositivo selecciona el valor máximo de los ejemplos de 20 segundos y lo envía a Azure.
- Este modelo no depende de la configuración de estadísticas de vCenter Server para la recopilación de datos de rendimiento.
- Puede detener la generación de perfiles continua en cualquier momento desde el recopilador.

**Valoraciones rápidas:** Con el dispositivo de detección continua, una vez que la detección se ha completado (tarda un par de horas según el número de VM), podrá crear valoraciones de forma instantánea. Puesto que la recopilación de datos de rendimiento se inicia al comenzar la detección, si lo que busca es obtener valoraciones rápidas, sebe de seleccionar *local* como criterio de tamaño en la valoración. Para obtener valoraciones basadas en el rendimiento, se recomienda esperar al menos un día después de iniciar la detección para obtener recomendaciones de tamaño que sean confiables.

El dispositivo solo recopila datos de rendimiento de forma continua, no detecta ningún cambio de configuración en el entorno local (por ejemplo, adición de VM, eliminación o adición de disco, entre otros). Si se produce un cambio de configuración en el entorno local, puede hacer lo siguiente para reflejar los cambios en el portal:

- Adición de elementos (máquinas virtuales, discos, núcleos, etc.): para reflejar estos cambios en Azure Portal, puede detener la detección desde la aplicación y después iniciarla de nuevo. Así se asegurará de que los cambios se actualizan en el proyecto de Azure Migrate.

- Eliminación de máquinas virtuales: debido a la forma en que está diseñada la aplicación, la eliminación de las máquinas virtuales no se refleja aunque detenga e inicie la detección. Esto se debe a que los datos de las detecciones posteriores se agregan a las detecciones más antiguas y no se reemplazan. En este caso, puede simplemente omitir la máquina virtual en el portal quitándola del grupo y recalculando la valoración.

> [!NOTE]
> El dispositivo de detección de una sola vez está en desuso, ya que este método dependía de la configuración de las estadísticas de vCenter Server para la disponibilidad de punto de datos de rendimiento y la media recopilada de los contadores de rendimiento, lo que daba lugar a un cálculo de tamaño insuficiente de las máquinas virtuales para la migración a Azure.

## <a name="deploying-the-collector"></a>Implementar el recopilador

Para implementar el dispositivo del recopilador mediante una plantilla OVF:

- Descargue la plantilla OVF desde un proyecto de Azure Migrate en Azure Portal. Importe el archivo descargado en vCenter Server para configurar la máquina virtual del dispositivo del recopilador.
- Desde OVF, VMware configura una máquina virtual con 8 núcleos, 16 GB de RAM y un disco de 80 GB. El sistema operativo es Windows Server 2016 (64 bits).
- Cuando ejecute el recopilador, se ejecuta un número de comprobaciones de requisitos previos para asegurarse de que el recopilador puede conectarse a Azure Migrate.

- [Más información](tutorial-assessment-vmware.md#create-the-collector-vm) sobre el recopilador.


## <a name="collector-prerequisites"></a>Requisitos previos del recopilador

El recopilador debe pasar algunas comprobaciones de requisitos previos para asegurarse de que puede conectarse al servicio Azure Migrate a través de Internet y cargar los datos detectados.

- **Verifique la nube de Azure**: El recopilador debe conocer la nube de Azure a la que se va a migrar.
    - Si va a migrar a la nube de Azure Government, seleccione Azure Government.
    - Si va a migrar a la nube comercial de Azure, seleccione Azure Global.
    - Según la nube especificada aquí, el dispositivo enviará metadatos detectados a los puntos de conexión respectivos.
- **Compruebe la conexión a Internet**: El recopilador puede conectarse a Internet directamente o a través de un proxy.
    - La comprobación de requisitos previos comprueba la conectividad a las [direcciones URL obligatorias y opcionales](#urls-for-connectivity).
    - Si tiene una conexión directa a Internet, no es necesario realizar ninguna acción específica, salvo asegurarse de que el recolector puede llegar a las direcciones URL necesarias.
    - Si se conecta a través de un proxy, tenga en cuenta los requisitos que se indican más abajo.
- **Compruebe la sincronización temporal**: El recopilador debe estar sincronizado con el servidor horario de Internet para asegurarse de que se autentiquen las solicitudes al servicio.
    - La dirección URL portal.azure.com debe ser accesible desde el recopilador para que se pueda validar la hora.
    - Si la máquina no está sincronizada, debe cambiar la hora del reloj de la máquina virtual del recopilador para que coincida con la hora actual. Para hacerlo, abra un símbolo del sistema de administración en la máquina virtual y ejecute **w32tm /tz** para comprobar la zona horaria. Ejecute **w32tm /resync** para sincronizar la hora.
- **Compruebe que el servicio del recopilador se está ejecutando**:  El servicio Azure Migrate Collector debe estar ejecutándose en la VM del recopilador.
    - Este servicio se inicia automáticamente cuando se inicia la máquina.
    - Si el servicio no se está ejecutando, inícielo desde el Panel de control.
    - El servicio del recopilador se conecta a vCenter Server, recopila los metadatos y los datos de rendimiento de la máquina virtual, y los envía al servicio Azure Migrate.
- **Compruebe si está instalado VMware PowerCLI 6.5**: El módulo de VMware PowerCLI 6.5 de PowerShell debe estar instalado en la VM del recopilador para que pueda comunicarse con vCenter Server.
    - Si el recopilador puede tener acceso a las direcciones URL necesarias para instalar el módulo, se instala automáticamente durante la implementación del recopilador.
    - Si el recopilador no puede instalar el módulo durante la implementación, deberá instalarlo manualmente.
- **Compruebe la conexión a vCenter Server**: El recopilador debe ser capaz de conectarse a vCenter Server y consultar las VM, sus metadatos y los contadores de rendimiento. [Comprobar los requisitos previos](#connect-to-vcenter-server) para la conexión.


### <a name="connect-to-the-internet-via-a-proxy"></a>Conectarse a Internet a través de un servidor proxy

- Si el servidor proxy necesita autenticación, puede especificar el nombre de usuario y la contraseña al configurar el recopilador.
- La dirección IP o el FQDN del servidor proxy deben especificarse como *http://IPaddress* o *http://FQDN*.
- Solo se admite un proxy HTTP. El recopilador no admite servidores proxy basados en HTTPS.
- Si el servidor proxy es un proxy de interceptación, debe importar el certificado de proxy en la máquina virtual del recopilador.
    1. En la máquina virtual del recopilador, vaya a **Menú Inicio** > **Administrar certificados de equipo**.
    2. En la herramienta Certificados, en **Certificados - Equipo local**, busque **Editores de confianza** > **Certificados**.

        ![Herramienta Certificados](./media/concepts-intercepting-proxy/certificates-tool.png)

    3. Copie el certificado de proxy en la máquina virtual del recopilador. Es posible que deba obtenerlo desde el administrador de red.
    4. Haga doble clic en el certificado para abrirlo y haga clic en **Instalar certificado**.
    5. En el Asistente para importación de certificados > Ubicación del almacén, elija **Máquina Local**.

    ![Ubicación del almacén de certificados](./media/concepts-intercepting-proxy/certificate-store-location.png)

    6. Seleccione **Colocar todos los certificados en el siguiente almacén** > **Examinar** > **Editores de confianza**. Haga clic en **Finalizar** para importar el certificado.

    ![Almacén de certificados](./media/concepts-intercepting-proxy/certificate-store.png)

    7. Compruebe que el certificado se importa según lo previsto y que las comprobaciones de requisitos previos de conectividad de Internet funcionan según lo esperado.


### <a name="urls-for-connectivity"></a>Direcciones URL para la conectividad

La comprobación de conectividad se valida mediante la conexión a una lista de direcciones URL.

**URL** | **Detalles**  | **Comprobación de requisitos previos**
--- | --- | ---
*.portal.azure.com | Se aplica a Azure Global. Comprueba la conectividad con el servicio de Azure y la sincronización horaria. | Se necesita acceso a URL.<br/><br/> Se produce un error de comprobación de requisitos previos si no hay conectividad.
*.portal.azure.us | Se aplica solo a Azure Government. Comprueba la conectividad con el servicio de Azure y la sincronización horaria. | Se necesita acceso a URL.<br/><br/> Se produce un error de comprobación de requisitos previos si no hay conectividad.
*.oneget.org:443<br/><br/> *.windows.net:443<br/><br/> *.windowsazure.com:443<br/><br/> *.powershellgallery.com:443<br/><br/> *.msecnd.net:443<br/><br/> *.visualstudio.com:443| Sirve para descargar el módulo vCenter PowerCLI de PowerShell. | El acceso a direcciones URL es opcional.<br/><br/> No se producirá un error en la comprobación de requisitos previos.<br/><br/> Se producirá un error en la instalación automática del módulo en la máquina virtual del recopilador. Es necesario instalar manualmente el módulo.


### <a name="install-vmware-powercli-module-manually"></a>Instalar manualmente el módulo de VMware PowerCLI

1. Instale el módulo siguiendo [estos pasos](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html). Estos pasos describen la instalación tanto con conexión como sin ella.
2. Si la máquina virtual del recopilador está sin conexión y se instala en el módulo en una máquina diferente con acceso a Internet, deberá copiar los archivos VMware.* desde esa máquina a la máquina virtual de recopilador.
3. Después de la instalación, puede reiniciar las comprobaciones de requisitos previos para confirmar que PowerCLI está instalado.

### <a name="connect-to-vcenter-server"></a>Conectar a vCenter Server

El recopilador se conecta a vCenter Server y consulta los metadatos de la máquina virtual y los contadores de rendimiento. Esto es lo que necesita para la conexión.

- Solo se admiten las versiones 5.5, 6.0, 6.5 y 6.7 de vCenter Server.
- Necesita una cuenta de solo lectura con los permisos para detección que se resumen más abajo. Para la detección, solo es posible acceder a los centros de datos accesibles con la cuenta.
- De forma predeterminada, se conecta a vCenter Server con una dirección IP o un FQDN. Si vCenter Server escucha en un puerto diferente, conéctese a él con el formato *IPAddress:Port_Number* o *FQDN:Port_Number*.
- El recopilador debe tener una línea de visión de la red con el servidor vCenter.

#### <a name="account-permissions"></a>Permisos de la cuenta

**Cuenta** | **Permisos**
--- | ---
Al menos una cuenta de usuario de solo lectura | Objeto de centro de datos  –> Propagar al objeto secundario, rol = solo lectura   


## <a name="collector-communications"></a>Comunicaciones del recopilador

El recopilador se comunica como se resume en el diagrama y la tabla siguientes.

![Diagrama de comunicación de recopilador](./media/tutorial-assessment-vmware/portdiagram.PNG)


**El recopilador se comunica con** | **Puerto** | **Detalles**
--- | --- | ---
Servicio Azure Migrate | TCP 443 | El recopilador se comunica con el servicio Azure Migrate a través de SSL 443.
vCenter Server | TCP 443 | El recopilador debe ser capaz de comunicarse con vCenter Server.<br/><br/> De manera predeterminada, se conecta a vCenter en el puerto 443.<br/><br/> Si vCenter Server escucha en otro puerto, debe estar disponible como puerto de salida en el recopilador.
RDP | TCP 3389 |

## <a name="collected-metadata"></a>Metadatos recopilados

El dispositivo del recopilador detecta los siguientes metadatos de configuración para cada VM. Los datos de configuración de las VM están disponibles una hora después de iniciar la detección.

- Nombre para mostrar de la máquina virtual (en vCenter Server)
- Ruta de acceso de inventario de la máquina virtual (el host o la carpeta en vCenter Server)
- Dirección IP
- Dirección MAC
- Sistema operativo
- Número de núcleos, discos, NIC
- Tamaño de memoria, tamaños de disco
- Contadores de rendimiento de la máquina virtual, el disco y la red.

### <a name="performance-counters"></a>contadores de rendimiento

 El dispositivo del recopilador recopila los siguientes contadores de rendimiento para cada VM desde el host ESXi en un intervalo de 20 segundos. Estos son los contadores de vCenter y, aunque la terminología indica el promedio, los ejemplos de 20 segundos son contadores en tiempo real. Los datos de rendimiento para las VM comienzan a estar disponibles en el portal dos horas después de haberse iniciado la detección. Se recomienda encarecidamente esperar al menos un día antes de crear evaluaciones basadas en el rendimiento para obtener recomendaciones de tamaño precisas. Si busca una gratificación instantánea, puede crear evaluaciones con criterio de tamaño, por ejemplo *como local*, que no tendrá en cuenta los datos de rendimiento para el ajuste del tamaño.

**Contador** |  **Impacto en la evaluación**
--- | ---
cpu.usage.average | Costo y tamaño de VM recomendados  
mem.usage.average | Costo y tamaño de VM recomendados  
virtualDisk.read.average | Calcula el tamaño del disco, el costo de almacenamiento y el tamaño de máquina virtual
virtualDisk.write.average | Calcula el tamaño del disco, el costo de almacenamiento y el tamaño de máquina virtual
virtualDisk.numberReadAveraged.average | Calcula el tamaño del disco, el costo de almacenamiento y el tamaño de máquina virtual
virtualDisk.numberWriteAveraged.average | Calcula el tamaño del disco, el costo de almacenamiento y el tamaño de máquina virtual
net.received.average | Calcula el tamaño de la máquina virtual                          
net.transmitted.average | Calcula el tamaño de la máquina virtual     

La lista completa de contadores de VMware recopilados por Azure Migrate está disponible a continuación:

**Categoría** |  **Metadata** | **Elemento DataPoint de vCenter**
--- | --- | ---
Detalles de la máquina | Id. de VM | vm.Config.InstanceUuid
Detalles de la máquina | Nombre de la máquina virtual | vm.Config.Name
Detalles de la máquina | Id. de vCenter Server | VMwareClient.InstanceUuid
Detalles de la máquina |  Descripción de la VM |  vm.Summary.Config.Annotation
Detalles de la máquina | Nombre del producto de licencia | vm.Client.ServiceContent.About.LicenseProductName
Detalles de la máquina | Tipo de sistema operativo | vm.Summary.Config.GuestFullName
Detalles de la máquina | Versión del sistema operativo | vm.Summary.Config.GuestFullName
Detalles de la máquina | Tipo de arranque | vm.Config.Firmware
Detalles de la máquina | Número de núcleos | vm.Config.Hardware.NumCPU
Detalles de la máquina | Megabytes de memoria | vm.Config.Hardware.MemoryMB
Detalles de la máquina | Número de discos | vm.Config.Hardware.Device.ToList().FindAll(x => x es VirtualDisk).count
Detalles de la máquina | Lista de tamaños de los discos | vm.Config.Hardware.Device.ToList().FindAll(x => x es VirtualDisk)
Detalles de la máquina | Lista de adaptadores de red | vm.Config.Hardware.Device.ToList().FindAll(x => x es VirtualEthernetCard)
Detalles de la máquina | Uso de CPU | cpu.usage.average
Detalles de la máquina | Uso de memoria | mem.usage.average
Detalles del disco (por disco) | Valor de clave del disco | disk.Key
Detalles del disco (por disco) | Número de unidad del disco | disk.UnitNumber
Detalles del disco (por disco) | Valor de clave de controladora de disco | disk.ControllerKey.Value
Detalles del disco (por disco) | Gigabytes aprovisionados | virtualDisk.DeviceInfo.Summary
Detalles del disco (por disco) | Nombre del disco | Este valor se genera mediante disk.UnitNumber, disk.Key y disk.ControllerKey.Value
Detalles del disco (por disco) | Número de operaciones de lectura por segundo | virtualDisk.numberReadAveraged.average
Detalles del disco (por disco) | Número de operaciones de escritura por segundo | virtualDisk.numberWriteAveraged.average
Detalles del disco (por disco) | Megabytes por segundo de rendimiento de lectura | virtualDisk.read.average
Detalles del disco (por disco) | Megabytes por segundo de rendimiento de escritura | virtualDisk.write.average
Detalles del adaptador de red (por NIC) | Nombre del adaptador de red | nic.Key
Detalles del adaptador de red (por NIC) | Dirección MAC | ((VirtualEthernetCard)nic).MacAddress
Detalles del adaptador de red (por NIC) | Direcciones IPv4 | vm.Guest.Net
Detalles del adaptador de red (por NIC) | Direcciones IPv6 | vm.Guest.Net
Detalles del adaptador de red (por NIC) | Megabytes por segundo de rendimiento de lectura | net.received.average
Detalles del adaptador de red (por NIC) | Megabytes por segundo de rendimiento de escritura | net.transmitted.average
Detalles de la ruta de acceso de inventario | NOMBRE | container.GetType().Name
Detalles de la ruta de acceso de inventario | Tipo de objeto secundario | container.ChildType
Detalles de la ruta de acceso de inventario | Información de referencia | container.MoRef
Detalles de la ruta de acceso de inventario | Ruta de acceso del inventario completo | container.Name con ruta de acceso completa
Detalles de la ruta de acceso de inventario | Detalles del objeto primario | Container.Parent
Detalles de la ruta de acceso de inventario | Detalles de carpeta para cada VM | ((Folder)container).ChildEntity.Type
Detalles de la ruta de acceso de inventario | Detalles del centro de datos para cada carpeta de la VM | ((Datacenter)container).VmFolder
Detalles de la ruta de acceso de inventario | Detalles del centro de datos para cada carpeta de host | ((Datacenter)container).HostFolder
Detalles de la ruta de acceso de inventario | Detalles de clúster para cada host | ((ClusterComputeResource)container).Host)
Detalles de la ruta de acceso de inventario | Detalles de host para cada VM | ((HostSystem)container).Vm


## <a name="securing-the-collector-appliance"></a>Protección del dispositivo del recopilador

Recomendamos seguir estos pasos para proteger el dispositivo del recopilador:

- No pierda las contraseñas de administrador ni las comparta con otras personas no autorizadas.
- Apagar la aplicación cuando no se esté usando.
- Colocar la aplicación en una red protegida.
- Una vez finalizada la migración, elimine la instancia del dispositivo.
- Tras la migración, asegúrese también de eliminar los archivos de copia de seguridad del disco (VMDK), ya que es posible que dichos discos tengan credenciales de vCenter almacenadas en caché.

## <a name="os-license-in-the-collector-vm"></a>Licencia de sistema operativo en la máquina virtual del recopilador

El recopilador viene con una licencia de evaluación de Windows Server 2012 R2 que es válida durante 180 días. Si va a expirar el período de evaluación para la máquina virtual del recopilador, se recomienda descargar un nuevo archivo OVA y crear un nuevo dispositivo.

## <a name="updating-the-os-of-the-collector-vm"></a>Actualizar el sistema operativo de la máquina virtual del recopilador

Aunque el dispositivo del recopilador tiene una licencia de evaluación de 180 días, deberá actualizar continuamente el sistema operativo del dispositivo para evitar que el dispositivo se apague automáticamente.

- Si el recopilador no se actualiza durante 60 días, se inicia el apagado de la máquina automáticamente.
- Si se está ejecutando una detección, la máquina no se desactivará, aunque hayan pasado 60 días. La máquina se apagará cuando se complete el trabajo de detección.
- Si ha usado el recopilador durante más de 60 días, recomendamos que mantenga la máquina actualizada en todo momento mediante la ejecución de Windows Update.

## <a name="upgrading-the-collector-appliance-version"></a>Actualizar la versión del dispositivo del recopilador

Puede actualizar el recopilador a la versión más reciente sin tener que descargar un archivo OVA de nuevo.

1. Descargue el [paquete de actualización más reciente que se muestre](concepts-collector-upgrade.md).
2. Para asegurarse de que la revisión descargada es segura, abra la ventana de comandos del administrador y ejecute el siguiente comando para generar el valor hash para el archivo ZIP. El código hash generado debe coincidir con el hash que se ha mencionado en la versión específica:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    (ejemplo de uso C:\>CertUtil -HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)
3. Copie el archivo zip en la máquina virtual del recopilador de Azure Migrate (aplicación del recopilador).
4. Haga clic con el botón derecho en el archivo ZIP y seleccione Extraer todo.
5. Haga clic con el botón derecho en Setup.ps1, seleccione Ejecutar con PowerShell y siga las instrucciones en pantalla para instalar la actualización.

## <a name="discovery-process"></a>Proceso de detección

Una vez configurado el dispositivo, puede ejecutar la detección. Así es como funciona:

- La detección se ejecuta por ámbito. Se detectarán todas las máquinas virtuales en la ruta de acceso de inventario de vCenter especificada.
    - Se establece un ámbito cada vez.
    - El ámbito puede incluir 1500 máquinas virtuales o menos.
    - El ámbito puede ser un centro de datos, una carpeta o un host ESXi.
- Después de conectarse a vCenter Server, debe conectarse especificando un proyecto de migración para la colección.
- Se detectan las máquinas virtuales y sus metadatos y datos de rendimiento se envían a Azure. Estas acciones forman parte de un trabajo de recopilación.
    - El dispositivo del recopilador tiene un identificador específico del recopilador que se conserva para una determinada máquina en las distintas detecciones.
    - A cada trabajo de recopilación en ejecución se le asigna un identificador de sesión específico. El identificador cambia para cada trabajo de recopilación y se puede usar para solucionar problemas.

## <a name="next-steps"></a>Pasos siguientes

[Configuración de una evaluación para máquinas virtuales VMware locales](tutorial-assessment-vmware.md)
