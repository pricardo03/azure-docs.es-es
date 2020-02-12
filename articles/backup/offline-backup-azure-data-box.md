---
title: Copia de seguridad sin conexión con Azure Data Box
description: Obtenga información sobre cómo puede usar Azure Data Box para propagar los datos de grandes copias de seguridad iniciales sin conexión desde el agente de MARS a un almacén de Azure Recovery Services.
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 553939df8aa7a1b097b2cd6c7f29365302101324
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77026496"
---
# <a name="azure-backup-offline-backup-using-azure-data-box"></a>Copia de seguridad sin conexión de Azure Backup con Azure Data Box

Puede usar el servicio [Azure Data Box](https://docs.microsoft.com/azure/databox/data-box-overview) para propagar sus grandes copias de seguridad de MARS iniciales sin conexión (sin usar la red) a un almacén de Azure Recovery Services.  Esto ahorra tiempo y ancho de banda de red que, de otro modo, se consumirían al mover grandes cantidades de datos de copia de seguridad en línea a través de una red de alta latencia. Esta mejora está actualmente en versión preliminar. La copia de seguridad sin conexión basada en Azure Data Box proporciona dos ventajas diferenciadas sobre la [copia de seguridad sin conexión basada en el servicio Azure Import/Export](https://docs.microsoft.com/azure/backup/backup-azure-backup-import-export).

1. No es necesario obtener sus propios discos y conectores compatibles con Azure. El servicio Azure Data Box envía los discos asociados a la [SKU de Data Box](https://azure.microsoft.com/services/databox/data/) seleccionada.

2. Azure Backup (agente de MARS) puede escribir directamente los datos de copia de seguridad en las SKU admitidas de Azure Data Box. Esto elimina la necesidad de aprovisionar una ubicación de almacenamiento provisional para los datos de copias de seguridad iniciales, así como la necesidad de que las utilidades formateen y copien los datos en los discos.

## <a name="azure-data-box-with-mars-agent"></a>Azure Data Box con el agente de MARS

En este artículo se explica cómo puede usar Azure Data Box para propagar los datos de grandes copias de seguridad iniciales sin conexión desde el agente de MARS a un almacén de Azure Recovery Services. El artículo se divide en las siguientes partes:

* Plataformas compatibles
* SKU de Data Box admitidas
* Requisitos previos
* Instalación del agente de MARS
* Instalación de Azure Data Box
* Transferencia de datos de copia de seguridad a Azure Data Box
* Pasos posteriores a la copia de seguridad

## <a name="supported-platforms"></a>Plataformas compatibles

El proceso para propagar los datos del agente de MARS mediante Azure Data Box se admite en las siguientes SKU de Windows:

| **SISTEMA OPERATIVO**                                 | **SKU**                                                      |
| -------------------------------------- | ------------------------------------------------------------ |
| **Estación de trabajo**                        |                                                              |
| Windows 10, 64 bits                     | Enterprise, Pro, Home                                       |
| Windows 8.1, 64 bits                    | Enterprise, Pro                                             |
| Windows 8, 64 bits                      | Enterprise, Pro                                             |
| Windows 7, 64 bits                      | Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| **Server**                             |                                                              |
| Windows Server 2019, 64 bits            | Standard, Datacenter, Essentials                            |
| Windows Server 2016, 64 bits            | Standard, Datacenter, Essentials                            |
| Windows Server 2012 R2, 64 bits         | Standard, Datacenter, Foundation                            |
| Windows Server 2012, 64 bits            | Datacenter, Foundation, Standard                            |
| Windows Storage Server 2016, 64 bits    | Standard, Workgroup                                         |
| Windows Storage Server 2012 R2, 64 bits | Standard, Workgroup, Essential                              |
| Windows Storage Server 2012, 64 bits    | Standard, Workgroup                                         |
| Windows Server 2008 R2 SP1, 64 bits     | Standard, Enterprise, Datacenter, Foundation                |
| Windows Server 2008 SP2, 64 bits        | Standard, Enterprise, Datacenter                            |

## <a name="backup-data-size-and-supported-data-box-skus"></a>Tamaño de los datos de copia de seguridad y SKU de Data Box admitidas

| Tamaño de los datos de copia de seguridad (luego de la compresión por MARS)* por servidor | SKU de Azure Data Box admitidas                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <= 7,2 TB                                                    | [Azure  Data Box Disk](https://docs.microsoft.com/azure/databox/data-box-disk-overview) |
| > 7,2 TB y <= 80 TB**                                      | [Azure Data Box (100 TB)](https://docs.microsoft.com/azure/databox/data-box-overview) |

\* Las tasas de compresión típicas varían entre el 10 y el 20 % <br>
** Póngase en contacto con [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com) si espera tener más de 80 TB de datos de copia de seguridad inicial para un único servidor MARS.

>[!IMPORTANT]
>Los datos de copia de seguridad inicial de un único servidor deben estar contenidos en una única instancia de Azure Data Box o Azure Data Box Disk, y no se pueden compartir entre varios dispositivos con una SKU igual o distinta. Por otro lado, un dispositivo Azure Data Box puede contener copias de seguridad iniciales de varios servidores.

## <a name="pre-requisites"></a>Requisitos previos

### <a name="azure-subscription-and-required-permissions"></a>Suscripción a Azure y permisos necesarios

* El proceso requiere una suscripción a Azure.
* El proceso requiere que el usuario designado para realizar la directiva de copia de seguridad sin conexión sea un "propietario" de la suscripción a Azure.
* El trabajo de Data Box y el almacén de Recovery Services (al que se deben propagar los datos) deben estar en las mismas suscripciones.
* Se recomienda que la cuenta de almacenamiento de destino asociada con el trabajo de Azure Data Box y el almacén de Recovery Services estén en la misma región. Sin embargo, esto no es necesario.

### <a name="get-azure-powershell-370"></a>Obtener Azure PowerShell 3.7.0

**Este es el requisito previo más importante para el proceso**. Antes de instalar Azure PowerShell (ver. 3.7.0), lleve a cabo las comprobaciones siguientes**.

#### <a name="step-1-check-powershell-version"></a>Paso 1: Comprobar la versión de PowerShell

* Abra Windows PowerShell y ejecute el siguiente comando:

    ```powershell
    Get-Module -ListAvailable AzureRM*
    ```

* Si el resultado muestra una versión superior a **3.7.0**, siga con el paso 2 a continuación. En caso contrario, vaya al paso 3.

#### <a name="step-2-uninstall-the-powershell-version"></a>Paso 2: Desinstalar la versión de PowerShell

Para desinstalar la versión actual de PowerShell, siga los pasos a continuación:

* Quite los módulos dependientes ejecutando el siguiente comando en PowerShell:

    ```powershell
    foreach ($module in (Get-Module -ListAvailable AzureRM*).Name |Get-Unique)  { write-host "Removing Module $module" Uninstall-module $module }
    ```

* Ejecute el siguiente comando para asegurar la correcta eliminación de todos los módulos dependientes:

    ```powershell
    Get-Module -ListAvailable AzureRM*
    ```

#### <a name="step-3-install-powershell-version-370"></a>Paso 3: Instalar PowerShell versión 3.7.0

Una vez que haya verificado que no haya presente ningún módulo de AzureRM, instale la versión 3.7.0 con uno de los métodos siguientes:

* Desde GitHub, [vínculo](https://github.com/Azure/azure-powershell/releases/tag/v3.7.0-March2017).

O BIEN

* Ejecute el comando siguiente en la ventana de PowerShell:

    ```powershell
    Install-Module -Name AzureRM -RequiredVersion 3.7.0
    ```

También se podría haber instalado Azure PowerShell mediante un archivo .msi. Para quitarlo, desinstálelo mediante la opción Desinstalar programas del Panel de control.

### <a name="order-and-receive-the-data-box-device"></a>Pedir y recibir el dispositivo Data Box

El proceso de copia de seguridad sin conexión que usa MARS y Azure Data Box requiere que los dispositivos Data Box necesarios se encuentren en el estado "Entregado" antes de desencadenar la copia de seguridad sin conexión mediante el agente de MARS. Consulte el [Tamaño de los datos de copia de seguridad y SKU de Data Box admitidas](#backup-data-size-and-supported-data-box-skus) para pedir la SKU más adecuada para sus requisitos. Siga los pasos de [este artículo](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-ordered) para pedir y recibir los dispositivos Data Box.

>[!IMPORTANT]
>No seleccione BlobStorage para el tipo de cuenta. El agente de MARS requiere una cuenta que admita blobs en páginas, lo que no se admite cuando se selecciona BlobStorage. Se recomienda encarecidamente seleccionar *Storage V2* (*general purpose v2*) (Almacenamiento V2 [uso general v2]) como tipo de cuenta al crear la cuenta de almacenamiento de destino para el trabajo de Azure Data Box.

![Elegir el tipo de cuenta en los detalles de la instancia](./media/offline-backup-azure-data-box/instance-details.png)

## <a name="install-and-setup-the-mars-agent"></a>Instalación y configuración del agente de MARS

* Asegúrese de desinstalar toda instalación anterior del agente de MARS.

* Descargue de la última versión del agente de MARS [aquí](https://aka.ms/azurebackup_agent).

* Ejecute *MARSAgentInstaller.exe* y siga ***solo** los pasos de [Instalación y registro del agente](https://docs.microsoft.com/azure/backup/backup-configure-vault#install-and-register-the-agent) en el almacén de Recovery Services en el que desea que se almacenen las copias de seguridad.

  >[!NOTE]
  > El almacén de Recovery Services debe estar en la misma suscripción que el trabajo de Azure Data Box.

* Una vez que registre el agente en el almacén de Recovery Services, siga los pasos de las secciones siguientes.

## <a name="setup-azure-data-box-devices"></a>Instalación de dispositivos Azure Data Box

En función de la SKU de Azure Data Box que haya pedido, siga los pasos descritos en las secciones correspondientes para instalar y preparar los dispositivos Data Box para que el agente de MARS identifique y transfiera los datos de copia de seguridad inicial.

### <a name="setup-azure-data-box-disk"></a>Instalación de Azure Data Box Disk

Si ha pedido una o más instancias de Azure Data Box Disk (hasta 8 TB cada disco), siga los pasos mencionados aquí sobre [Desempaquetado, conexión y desbloqueo de Azure Data Box Disk](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-set-up).

>[!NOTE]
>Es posible que el servidor con el agente de MARS no tenga un puerto USB. En tal caso, puede conectar Azure Data Box Disk a otro servidor o cliente y exponer la raíz del dispositivo como recurso compartido de red.

### <a name="setup-azure-data-box"></a>Instalación de Azure Data Box

Si pidió Azure Data Box (hasta 100 TB), siga los pasos mencionados aquí [para instalar Data Box](https://docs.microsoft.com/azure/databox/data-box-deploy-set-up).

#### <a name="mount-your-azure-data-box-as-local-system"></a>Montar Azure Data Box como sistema local

El agente de MARS funciona en el contexto del sistema local y, por tanto, requiere que se proporcione el mismo nivel de privilegios a la ruta de acceso de montaje donde se conecta Azure Data Box. Siga los pasos a continuación para asegurarse de que puede montar el dispositivo Data Box como sistema local mediante el protocolo NFS:

* Habilite la característica Cliente para NFS en la instancia de Windows Server (donde tiene instalado el agente de MARS).<br>
  Especifique un origen alternativo: *WIM: D: \Sources\Install.wim: 4*

* Descargue PSExec de <https://download.sysinternals.com/files/PSTools.zip> en el servidor con el agente de MARS instalado.

* Abra un símbolo del sistema con privilegios elevados y ejecute el comando siguiente con el directorio que contiene PSExec.exe como directorio actual:

    ```cmd
    psexec.exe  -s  -i  cmd.exe
    ```

* La ventana de comandos que se abre como consecuencia del comando anterior se encuentra en el contexto del sistema local. Use esta ventana de comandos para ejecutar los pasos para montar el recurso compartido de blobs en páginas de Azure como unidad de red en la instancia de Windows Server.

* Siga [estos](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-nfs#connect-to-data-box) pasos para conectar el servidor con el agente de MARS al dispositivo Data Box a través de NFS, y ejecute el siguiente comando en el símbolo del sistema local para montar el recurso compartido de blobs en páginas de Azure:

    ```cmd
    mount -o nolock \\<DeviceIPAddress>\<StorageAccountName_PageBlob X:  
    ```

* Una vez montado, compruebe si puede tener acceso a X: desde el servidor. En caso afirmativo, vaya a la sección siguiente de este artículo.

## <a name="transfer-initial-backup-data-to-azure-data-box-devices"></a>Transferencia de datos de copia de seguridad inicial a dispositivos Azure Data Box

* Abra la aplicación **Microsoft Azure Backup** en el servidor.

* Haga clic en **Programar copia de seguridad** en el panel **Acciones**.

    ![Haga clic en Programar una copia de seguridad](./media/offline-backup-azure-data-box/schedule-backup.png)

* Siga los pasos del **Asistente para programar copias de seguridad**.

* **Agregue elementos** de modo que el tamaño total de los elementos se encuentre dentro de los [límites de tamaño admitidos por la SKU de Azure Data Box](#backup-data-size-and-supported-data-box-skus) que pidió y recibió.

    ![Agregar elementos para la copia de seguridad](./media/offline-backup-azure-data-box/add-items.png)

* Seleccione el programa de copia de seguridad y la directiva de retención adecuados para archivos y carpetas y estado del sistema (el estado del sistema solo es aplicable para instancias de Windows Server y no para clientes Windows).

* En la pantalla **Elegir el tipo de copia de seguridad inicial (archivos y carpetas)** del asistente, elija la opción **Transfer using Microsoft Azure Data Box disks** (Transferir con discos de Microsoft Azure Data Box) y haga clic en **Siguiente**.

    ![Elección del tipo de copia de seguridad inicial](./media/offline-backup-azure-data-box/initial-backup-type.png)

* Inicie sesión en Azure cuando se le solicite con las credenciales de usuario que tienen acceso de propietario a la suscripción de Azure. Después de hacerlo correctamente, debería ver una pantalla que se parece a la siguiente:

    ![Creación de recursos y aplicación de los permisos necesarios](./media/offline-backup-azure-data-box/creating-resources.png)

* A continuación, el agente de MARS capturará los trabajos de Data Box de la suscripción que se encuentran en estado "Entregado".

    ![Captura de trabajos de Data Box para el id. de suscripción](./media/offline-backup-azure-data-box/fetching-databox-jobs.png)

* Seleccione el pedido correcto de Data Box para el que ha desempaquetado, conectado y desbloqueado el disco de Data Box. Haga clic en **Next**.

    ![Selección de los pedidos de Data Box](./media/offline-backup-azure-data-box/select-databox-order.png)

* Haga clic en **Detectar el dispositivo** en la pantalla **Detección de dispositivos Data Box**. Esto hace que el agente de MARS busque discos de Azure Data Box conectados localmente y los detecte como se muestra a continuación:

    ![Detección de dispositivos Data Box](./media/offline-backup-azure-data-box/databox-device-detection.png)

    Si ha conectado Azure Data Box como recurso compartido de red (debido a la falta de disponibilidad de puertos USB o porque ha pedido y montado el dispositivo Data Box de 100 TB), se producirá un error en la detección al principio, pero le dará la opción de especificar la ruta de acceso de red al dispositivo Data Box como se muestra a continuación:

    ![Especificación de la ruta de acceso de red](./media/offline-backup-azure-data-box/enter-network-path.png)

    >[!IMPORTANT]
    > Proporcione la ruta de acceso de red al directorio raíz del disco de Azure Data Box. Este directorio debe contener un directorio con el nombre *PageBlob*, como se muestra a continuación:
    >
    >![Directorio raíz del disco de Azure Data Box](./media/offline-backup-azure-data-box/root-directory.png)
    >
    >Por ejemplo, si la ruta de acceso del disco es `\\mydomain\myserver\disk1\`, y *disk1* contiene un directorio denominado *PageBlob*, la ruta de acceso que se debe proporcionar en el asistente del agente de MARS es `\\mydomain\myserver\disk1\`.
    >
    >Si [instala un dispositivo Azure Data Box de 100 TB](#setup-azure-data-box), proporcione la siguiente como ruta de acceso de red al dispositivo `\\<DeviceIPAddress>\<StorageAccountName>_PageBlob`.

* Haga clic en **Siguiente** y en **Finalizar** en la pantalla siguiente para guardar la directiva de retención y copia de seguridad con la configuración de copia de seguridad sin conexión mediante Azure Data Box.

* En la pantalla siguiente se confirma que la directiva se ha guardado correctamente:

    ![La directiva se guardó correctamente](./media/offline-backup-azure-data-box/policy-saved.png)

* Haga clic en **Cerrar** en la pantalla anterior.

* Haga clic en ***Hacer Copia Ahora** en el panel **Acciones** de la consola del agente de MARS y haga clic en **Copia de seguridad** en la pantalla del asistente, como se muestra a continuación:

    ![Asistente de Hacer Copia Ahora](./media/offline-backup-azure-data-box/backup-now.png)

* El agente de MARS comenzará a hacer la copia de seguridad de los datos seleccionados en el dispositivo Azure Data Box. Esto puede tardar entre varias horas y algunos días según el número de archivos y la velocidad de conexión entre el servidor con el agente de MARS y Azure Data Box Disk.

* Una vez que se complete la copia de seguridad de los datos, verá una pantalla en el agente de MARS que se parece a la siguiente:

    ![Progreso de la copia de seguridad](./media/offline-backup-azure-data-box/backup-progress.png)

## <a name="post-backup-steps"></a>Pasos posteriores a la copia de seguridad

En esta sección se explican los pasos que se deben llevar a cabo una vez que se complete correctamente la copia de seguridad de los datos en Azure Data Box Disk.

* Siga los pasos de este artículo para [enviar el disco de Azure Data Box a Azure](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-picked-up). Si usó un dispositivo Azure Data Box de 100 TB, siga estos pasos para [enviar Azure Data Box a Azure](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up).

* [Supervise el trabajo de Data Box](https://docs.microsoft.com/azure/databox/data-box-disk-deploy-upload-verify) en Azure Portal Una vez que el trabajo de Azure Data Box esté "Completo", el agente de MARS moverá automáticamente los datos de la cuenta de almacenamiento al almacén de Recovery Services en el momento de la siguiente copia de seguridad programada. A continuación, marcará el trabajo de copia de seguridad como "Trabajo completado" si se ha creado correctamente un punto de recuperación.

    >[!NOTE]
    >El agente de MARS desencadenará las copias de seguridad en los momentos programados durante la creación de la directiva. Sin embargo, estos trabajos se marcarán como "Waiting for Azure Data Box job to be completed" (Esperando a que se complete el trabajo de Azure Data Box) hasta que se complete el trabajo.

* Una vez que el agente de MARS cree correctamente un punto de recuperación correspondiente a la copia de seguridad inicial, podrá eliminar la cuenta de almacenamiento (o el contenido específico) asociada con el trabajo de Azure Data Box.

## <a name="troubleshooting"></a>Solución de problemas

El agente de Microsoft Azure Backup (MAB) crea automáticamente una aplicación de Azure AD en su inquilino. Esta aplicación requiere un certificado para la autenticación, que se crea y se carga al configurar la directiva de propagación sin conexión. Usamos Azure PowerShell para crear y cargar el certificado en la aplicación de Azure AD.

### <a name="issue"></a>Problema

En el momento de configurar la copia de seguridad sin conexión, es posible que se produzca un problema, donde, debido a un error en el cmdlet de Azure PowerShell, no se pueden agregar varios certificados a la misma aplicación de Azure AD creada por el agente de MAB. Esto le afectará si ha configurado la directiva de propagación sin conexión para el mismo servidor o para otro.

### <a name="how-to-verify-if-the-issue-is-caused-by-this-specific-root-cause"></a>Cómo verificar si el problema se debe a esta causa principal específica

Para asegurarse de que el error se debe al problema anterior, siga uno de los pasos a continuación:

#### <a name="step-1"></a>Paso 1

Compruebe si aparece el siguiente mensaje de error en la consola de MAB en el momento de configurar la copia de seguridad sin conexión:

![No se puede crear la directiva Copia de seguridad sin conexión para la cuenta actual de Azure](./media/offline-backup-azure-data-box/unable-to-create-policy.png)

#### <a name="step-2"></a>Paso 2

* Abra la carpeta **Temp** en la ruta de instalación (la ruta de acceso predeterminada de la carpeta Temp es *C:\Archivos de programa\Microsoft Azure Recovery Services Agent\Temp*). Busque el archivo **CBUICurr** y ábralo.

* En el archivo **CBUICurr**, desplácese hasta la última línea y compruebe si el error se debe a `Unable to create an Azure AD application credential in customer's account. Exception: Update to existing credential with KeyId <some guid> is not allowed`.

### <a name="workaround"></a>Solución alternativa

Como solución alternativa para resolver este problema, siga los pasos a continuación y vuelva a intentar la configuración de la directiva.

#### <a name="first-step"></a>Primer paso

Inicie sesión en la instancia de PowerShell que aparece en la UI de MAB con otra cuenta con acceso de administrador en la suscripción en la que se creará el trabajo de importación y exportación.

#### <a name="second-step"></a>Segundo paso

Si ningún otro servidor tiene configurada la propagación sin conexión y ningún otro servidor depende de la aplicación `AzureOfflineBackup_<Azure User Id>`, elimine esta aplicación de **Azure Portal** > **Azure Active Directory** > **Registros de aplicaciones**.

>[!NOTE]
> Compruebe si la aplicación `AzureOfflineBackup_<Azure User Id>` no tiene configurada ninguna otra propagación sin conexión y que tampoco haya otro servidor que dependa de esta aplicación. Vaya a **Configuración** > **Claves** en la sección **Claves públicas**, no debe tener ninguna otra clave pública agregada. Como referencia, observe la siguiente captura de pantalla:
>
>![Claves públicas](./media/offline-backup-azure-data-box/public-keys.png)

#### <a name="third-step"></a>Tercer paso

Des de el servidor donde intenta configurar la copia de seguridad sin conexión, realice las siguientes acciones:

1. Abra la **aplicación de administración de certificados del equipo** > pestaña **Personal** y busque el certificado denominado `CB_AzureADCertforOfflineSeeding_<ResourceId>`.

2. Seleccione el certificado anterior, haga clic con el botón derecho en **Todas las tareas** y **Exportar** sin clave privada y en formato .cer.

3. Vaya a la aplicación de copia de seguridad sin conexión de Azure mencionada en el **punto 2**. En **Configuración** > **Claves** > **Cargar clave pública**, cargue el certificado exportado en el paso anterior.

    ![Cargar clave pública](./media/offline-backup-azure-data-box/upload-public-key.png)

4. En el servidor, escriba **regedit** en la ventana Ejecutar para abrir el Registro.

5. Vaya a la entrada del Registro *Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider*. Haga clic con el botón derecho en **CloudBackupProvider** y agregue un nuevo valor de cadena con el nombre `AzureADAppCertThumbprint_<Azure User Id>`.

    >[!NOTE]
    > Para obtener el identificador de usuario de Azure, realice una de las acciones siguientes:
    >
    >1. Desde la instancia de PowerShell conectada a Azure, ejecute el comando `Get-AzureRmADUser -UserPrincipalName “Account Holder’s email as defined in the portal”`.
    > 2. Navegue a la ruta de acceso del Registro: *Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DbgSettings\OnlineBackup*; Nombre: *CurrentUserId*

6. Haga clic con el botón derecho en la cadena agregada en el paso anterior y seleccione **Modificar**. En el valor, proporcione la huella digital del certificado exportado en el **punto 2** y haga clic en **Aceptar**.

7. Para obtener el valor de la huella digital, haga doble clic en el certificado y, a continuación, seleccione la pestaña **Detalles** y desplácese hacia abajo hasta que vea el campo Huella digital. Haga clic en **Huella digital** y copie el valor.

    ![Campo Huella digital del certificado](./media/offline-backup-azure-data-box/thumbprint-field.png)

## <a name="questions"></a>Preguntas

Si tiene pregunta o necesita aclaraciones con respecto a cualquier problema que surja, ponerse en contacto con [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com)
