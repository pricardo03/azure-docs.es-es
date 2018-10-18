---
title: Dispositivo del recopilador de Azure Migrate | Microsoft Docs
description: Proporciona información sobre el dispositivo del recopilador de Azure Migrate.
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/28/2018
ms.author: snehaa
services: azure-migrate
ms.openlocfilehash: b79045e54b9c2ee4846f2216704a419e0ff85501
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434439"
---
# <a name="about-the-collector-appliance"></a>Dispositivo del recopilador

 En este artículo se proporciona información sobre Azure Migrate Collector.

Azure Migrate Collector es un dispositivo ligero que sirve para detectar un entorno de vCenter local a efectos de evaluación con el servicio [Azure Migrate](migrate-overview.md) antes de realizar la migración a Azure.  


## <a name="deploying-the-collector"></a>Implementar el recopilador

Para implementar el dispositivo del recopilador mediante una plantilla OVF:

- Descargue la plantilla OVF desde un proyecto de Azure Migrate en Azure Portal. Importe el archivo descargado en vCenter Server para configurar la máquina virtual del dispositivo del recopilador.
- Desde OVF, VMware configura una máquina virtual con 4 núcleos, 8 GB de RAM y un disco de 80 GB. El sistema operativo es Windows Server 2012 R2 (64 bits).
- Cuando ejecute el recopilador, se ejecuta un número de comprobaciones de requisitos previos para asegurarse de que el recopilador puede conectarse a Azure Migrate.

- [Más información](tutorial-assessment-vmware.md#create-the-collector-vm) sobre el recopilador.


## <a name="collector-prerequisites"></a>Requisitos previos del recopilador

El recopilador debe pasar algunas comprobaciones de requisitos previos para asegurarse de que puede conectarse al servicio Azure Migrate a través de Internet y cargar los datos detectados.

- **Compruebe la conexión a Internet**: el recopilador puede conectarse a Internet directamente o a través de un proxy.
    - La comprobación de requisitos previos comprueba la conectividad a las [direcciones URL obligatorias y opcionales](#connect-to-urls).
    - Si tiene una conexión directa a Internet, no es necesario realizar ninguna acción específica, salvo asegurarse de que el recolector puede llegar a las direcciones URL necesarias.
    - Si se conecta a través de un proxy, tenga en cuenta los [requisitos](#connect-via-a-proxy) que se indican más abajo.
- **Comprobar sincronización temporal**: el recopilador debe estar sincronizado con el servidor horario de Internet para asegurarse de que se autentiquen las solicitudes al servicio.
    - La dirección URL portal.azure.com debe ser accesible desde el recopilador para que se pueda validar la hora.
    - Si la máquina no está sincronizada, debe cambiar la hora del reloj de la máquina virtual del recopilador para que coincida con la hora actual. Para hacerlo, abra un símbolo del sistema de administración en la máquina virtual y ejecute **w32tm /tz** para comprobar la zona horaria. Ejecute **w32tm /resync** para sincronizar la hora.
- **Comprobar que el servicio del recopilador se está ejecutando**: el servicio Azure Migrate Collector debe estar ejecutándose en la máquina virtual del recopilador.
    - Este servicio se inicia automáticamente cuando se inicia la máquina.
    - Si el servicio no se está ejecutando, inícielo desde el Panel de control.
    - El servicio del recopilador se conecta a vCenter Server, recopila los metadatos y los datos de rendimiento de la máquina virtual, y los envía al servicio Azure Migrate.
- **Comprobar si está instalado VMware PowerCLI 6.5**: el módulo de VMware PowerCLI 6.5 de PowerShell debe estar instalado en la máquina virtual del recopilador para que pueda comunicarse con vCenter Server.
    - Si el recopilador puede tener acceso a las direcciones URL necesarias para instalar el módulo, se instala automáticamente durante la implementación del recopilador.
    - Si el recopilador no puede instalar el módulo durante la implementación, deberá [instalarlo manualmente](#install-vwware-powercli-module-manually).
- **Comprobar la conexión a vCenter Server**: el recopilador debe ser capaz de conectarse a vCenter Server y consultar las máquinas virtuales, sus metadatos y los contadores de rendimiento. [Comprobar los requisitos previos](#connect-to-vcenter-server) para la conexión.


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




### <a name="connect-to-urls"></a>Conectarse a direcciones URL

La comprobación de conectividad se valida mediante la conexión a una lista de direcciones URL.

**URL** | **Detalles**  | **Comprobación de requisitos previos**
--- | --- | ---
*.portal.azure.com | Comprueba la conectividad con el servicio de Azure y la sincronización horaria. | Se necesita acceso a URL.<br/><br/> Se produce un error de comprobación de requisitos previos si no hay conectividad.
*.oneget.org:443<br/><br/> *.windows.net:443<br/><br/> *.windowsazure.com:443<br/><br/> *.powershellgallery.com:443<br/><br/> *.msecnd.net:443<br/><br/> *.visualstudio.com:443| Sirve para descargar el módulo vCenter PowerCLI de PowerShell. | El acceso a direcciones URL es opcional.<br/><br/> No se producirá un error en la comprobación de requisitos previos.<br/><br/> Se producirá un error en la instalación automática del módulo en la máquina virtual del recopilador. Es necesario instalar manualmente el módulo.


### <a name="install-vmware-powercli-module-manually"></a>Instalar manualmente el módulo de VMware PowerCLI

1. Instale el módulo siguiendo [estos pasos](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html). Estos pasos describen la instalación tanto con conexión como sin ella.
2. Si la máquina virtual del recopilador está sin conexión y se instala en el módulo en una máquina diferente con acceso a Internet, deberá copiar los archivos VMware.* desde esa máquina a la máquina virtual de recopilador.
3. Después de la instalación, puede reiniciar las comprobaciones de requisitos previos para confirmar que PowerCLI está instalado.

### <a name="connect-to-vcenter-server"></a>Conectar a vCenter Server

El recopilador se conecta a vCenter Server y consulta los metadatos de la máquina virtual y los contadores de rendimiento. Esto es lo que necesita para la conexión.

- Solo se admiten las versiones 5.5, 6.0 y 6.5 de vCenter Server.
- Necesita una cuenta de solo lectura con los permisos para detección que se resumen más abajo. Para la detección, solo es posible acceder a los centros de datos accesibles con la cuenta.
- De forma predeterminada, se conecta a vCenter Server con una dirección IP o un FQDN. Si vCenter Server escucha en un puerto diferente, conéctese a él con el formato *IPAddress:Port_Number* o *FQDN:Port_Number*.
- Para recopilar datos de rendimiento para el almacenamiento y la conexión a red, la configuración de estadísticas de vCenter Server debe establecerse en el nivel tres.
- Si el nivel es menor que tres, la detección funciona pero no se recopilarán los datos de rendimiento. Es posible que se recopilen algunos contadores, pero otros se establecerán en cero.
- Si no se recopilan datos de rendimiento para el almacenamiento y la conexión a red, las recomendaciones de tamaño para la valoración se basarán en los datos de rendimiento para CPU y memoria, y en los datos de configuración para los adaptadores de red y de disco.
- El recopilador debe tener una línea de visión de la red con el servidor vCenter.

#### <a name="account-permissions"></a>Permisos de la cuenta

**Cuenta** | **permisos**
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


## <a name="discovery-methods"></a>Métodos de detección

El dispositivo del recopilador puede usar dos métodos para la detección: detección de una sola vez o detección continua.


### <a name="one-time-discovery"></a>Detección de una sola vez

El recopilador se comunica una sola vez con vCenter Server para recopilar metadatos sobre las máquinas virtuales. Con este método:

- El dispositivo no está conectado continuamente al proyecto de Azure Migrate.
- Los cambios en el entorno local no se reflejan Azure Migrate una vez terminada la detección. Para reflejar los cambios, deberá detectar el mismo entorno en el mismo proyecto de nuevo.
- Para este método de detección, deberá establecer la configuración de estadísticas en vCenter Server en el nivel tres.
- Después de establecer el nivel a tres, se tarda hasta un día en generar los contadores de rendimiento. Por tanto, se recomienda ejecutar la detección después de un día.
- Cuando se recopilan datos de rendimiento para una máquina virtual, el dispositivo se basa en los datos de rendimiento históricos almacenados en vCenter Server. Recopila el historial de rendimiento del mes pasado.
- Azure Migrate recopila el valor promedio de contador (en lugar de un valor máximo) para cada métrica.

### <a name="continuous-discovery"></a>Detección continua

El dispositivo del recopilador no está conectado continuamente al proyecto de Azure Migrate.

- El recopilador realiza un perfil del entorno local continuamente para recopilar datos de uso en tiempo real cada 20 segundos.
- Este modelo no depende de la configuración de estadísticas de vCenter Server para la recopilación de datos de rendimiento.
- El dispositivo acumula ejemplos de 20 segundos y crea un único punto de datos cada 15 minutos.
- Para crear el punto de datos, el dispositivo selecciona el valor máximo de los ejemplos de 20 segundos y lo envía a Azure.
- Puede detener la generación de perfiles continua en cualquier momento desde el recopilador.

> [!NOTE]
> La función de detección continua está en versión preliminar. Si la configuración de estadísticas de vCenter Server no está establecida en el nivel 3, se recomienda que use este método.


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

### <a name="collected-metadata"></a>Metadatos recopilados

El dispositivo de recopilación detecta estos metadatos estáticos para las máquinas virtuales:

- Nombre para mostrar de la máquina virtual (en vCenter Server)
- Ruta de acceso de inventario de la máquina virtual (el host o la carpeta en vCenter Server)
- Dirección IP
- Dirección MAC
- Sistema operativo
- Número de núcleos, discos, NIC
- Tamaño de memoria, tamaños de disco
- Contadores de rendimiento de la máquina virtual, el disco y la red.

#### <a name="performance-counters"></a>contadores de rendimiento

- **Detección de un solo uso**: cuando se recopilan contadores para una detección de un solo uso, tenga en cuenta lo siguiente:

    - Puede tardar hasta 15 minutos en recopilar y enviar metadatos de configuración al proyecto.
    - Después de recopilar datos de configuración, los datos de rendimiento pueden tardar hasta una hora en estar disponibles en el portal.
    - Una vez que los metadatos están disponibles en el portal, aparece la lista de máquinas virtuales y puede empezar a crear grupos para su evaluación.
- **Detección continua**: para la detección continua, tenga en cuenta lo siguiente:
    - Los datos de configuración de la máquina virtual están disponibles una hora después de iniciar la detección.
    - Los datos de rendimiento empiezan a estar disponibles después de 2 horas.
    - Después de iniciar la detección, espere al menos un día para que el dispositivo genere el perfil del entorno, antes de crear las evaluaciones.

**Contador** | **Level** | **Nivel por dispositivo** | **Impacto en la evaluación**
--- | --- | --- | ---
cpu.usage.average | 1 | N/D | Costo y tamaño de VM recomendados  
mem.usage.average | 1 | N/D | Costo y tamaño de VM recomendados  
virtualDisk.read.average | 2 | 2 | Calcula el tamaño del disco, el costo de almacenamiento y el tamaño de máquina virtual
virtualDisk.write.average | 2 | 2  | Calcula el tamaño del disco, el costo de almacenamiento y el tamaño de máquina virtual
virtualDisk.numberReadAveraged.average | 1 | 3 |  Calcula el tamaño del disco, el costo de almacenamiento y el tamaño de máquina virtual
virtualDisk.numberWriteAveraged.average | 1 | 3 |   Calcula el tamaño del disco, el costo de almacenamiento y el tamaño de máquina virtual
net.received.average | 2 | 3 |  Calcula el tamaño de máquina virtual y el costo de la red                        |
net.transmitted.average | 2 | 3 | Calcula el tamaño de máquina virtual y el costo de la red    

## <a name="next-steps"></a>Pasos siguientes

[Configuración de una evaluación para máquinas virtuales VMware locales](tutorial-assessment-vmware.md)
