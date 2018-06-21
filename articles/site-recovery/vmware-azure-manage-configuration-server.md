---
title: Administración del servidor de configuración para realizar la recuperación ante desastres de VMware con Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo administrar un servidor de configuración existente para la recuperación ante desastres de VMware en Azure con Azure Site Recovery.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 753e123c660b1aacea1157157f0e580e15c47536
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287412"
---
# <a name="manage-the-configuration-server-for-vmware-vms"></a>Administración del servidor de configuración para máquinas virtuales de VMware

Se configura un servidor de configuración local cuando se usa [Azure Site Recovery](site-recovery-overview.md) para realizar la recuperación ante desastres de servidores físicos y máquinas virtuales de VMware en Azure. El servidor de configuración coordina la comunicación entre Azure y VMware local, además de administrar la replicación de datos. En este artículo se resumen las tareas comunes para administrar el servidor de configuración después de que se haya implementado.



## <a name="modify-vmware-settings"></a>Modificación de la configuración de VMware

Puede acceder al servidor de configuración como se indica a continuación:
    - Inicie sesión en la máquina virtual en que se implementa e inicie el administrador de configuración de Azure Site Recovery desde el acceso directo del escritorio.
    - Como alternativa, puede acceder al servidor de configuración de forma remota desde **https://*NombreDeServidorDeConfiguración*/:44315 /**. Inicie sesión con las credenciales de administrador.
   
### <a name="modify-vmware-server-settings"></a>Modificación de la configuración del servidor de VMware

1. Para asociar otro servidor de VMware con el servidor de configuración, después de iniciar sesión seleccione **Adición de servidor de vCenter Server o vSphere ESXi**.
2. Escriba los detalles y seleccione **Aceptar**.


### <a name="modify-credentials-for-automatic-discovery"></a>Modificación de las credenciales para la detección automática

1. Para actualizar las credenciales usadas para conectarse al servidor de VMware para detectar automáticamente máquinas virtuales de VMware, después de iniciar sesión seleccione **Edit** (Editar).
2. Escriba las nuevas credenciales y, a continuación, seleccione **OK** (Aceptar).

    ![Modificación para VMware](./media/vmware-azure-manage-configuration-server/modify-vmware-server.png)


## <a name="modify-credentials-for-mobility-service-installation"></a>Modificación de las credenciales para la instalación de Mobility Service

Modifique las credenciales usadas para instalar automáticamente Mobility Service en las máquinas virtuales de VMware que habilite para la replicación.

1. Después de iniciar sesión, seleccione **Administrar las credenciales de la máquina virtual**
2. Escriba las nuevas credenciales y, a continuación, seleccione **OK** (Aceptar).

    ![Modificación de las credenciales de Mobility Service](./media/vmware-azure-manage-configuration-server/modify-mobility-credentials.png)

## <a name="modify-proxy-settings"></a>Modificación de la configuración de proxy

Modifique la configuración de proxy que utiliza la máquina del servidor de configuración para acceder por Internet a Azure. Si, además del servidor de procesos predeterminado que se ejecuta en la máquina del servidor de configuración, tiene una máquina de servidor de procesos adicional, modifique la configuración en ambas máquinas.

1. Después de iniciar sesión en el servidor de configuración, seleccione **Administrar la conectividad**.
2. Actualice los valores del proxy. A continuación, seleccione **Save** (Guardar) para actualizar la configuración.

## <a name="add-a-network-adapter"></a>Incorporación de un adaptador de red

La plantilla OVF (Open Virtualization Format) implementa la máquina virtual del servidor de configuración con un único adaptador de red.

- Puede [agregar otro adaptador a la máquina virtual](vmware-azure-deploy-configuration-server.md#add-an-additional-adapter), pero debe hacerlo antes de registrar el servidor de configuración en el almacén.
- Para agregar un adaptador después de registrar el servidor de configuración en el almacén, agregue el adaptador en las propiedades de la máquina virtual. Luego hay que volver a registrar el servidor en el almacén.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Proceso para volver a registrar el servidor de configuración en el mismo almacén

Si lo necesita, puede volver a registrar el servidor de configuración en el mismo almacén. Si, además del servidor de procesos predeterminado que se ejecuta en la máquina del servidor de configuración, tiene una máquina del servidor de procesos adicional, vuelva a registrar ambas máquinas.


  1. En el almacén, abra **Administrar** > **Infraestructura de Site Recovery** > **Servidores de configuración**.
  2. En **Servers** (Servidores), seleccione **Download registration key** (Descargar clave de registro) para descargar el archivo de credenciales de almacén.
  3. Inicie sesión en la máquina del servidor de configuración.
  4. En **%ProgramData%\ASR\home\svsystems\bin**, abra **cspsconfigtool.exe**.
  5. En la pestaña **Vault Registration** (Registro del almacén), seleccione **Browse** (Examinar) y busque el archivo de credenciales de almacén que ha descargado.
  6. Si lo necesita, proporcione los detalles del servidor proxy. Después, seleccione **Register** (Registrar).
  7. Abra la ventana de comandos de administrador de PowerShell y ejecute el comando siguiente:

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

## <a name="upgrade-the-configuration-server"></a>Actualización del servidor de configuración

Los paquetes acumulativos se ejecutan para actualizar el servidor de configuración. Las actualizaciones se pueden aplicar para hasta las versiones N-4. Por ejemplo: 

- Si ejecuta 9.7 9.8, 9,9 o 9.10, puede actualizar directamente a 9.11.
- Si ejecuta la versión 9.6 o anterior, y desea actualizar a 9.11, primero debe actualizar a la versión 9.7 antes de hacerlo a la versión 9.11.

En la [página wiki de actualizaciones](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx), hay disponibles vínculos a paquetes acumulativos para actualizar a todas las versiones del servidor de configuración.

Actualice el servidor como se indica a continuación:

1. En el almacén, vaya a **Manage** > **Site Recovery Infrastructure** > **Configuration Servers** (Administrar > Infraestructura de Site Recovery > Servidores de configuración).
2. Si hay alguna actualización disponible, aparecerá un vínculo en la columna **Agent Version** (Versión del agente) >.

    ![Actualizar](./media/vmware-azure-manage-configuration-server/update2.png)

1. Descargue el archivo del instalador de actualizaciones en el servidor de configuración.

    ![Actualizar](./media/vmware-azure-manage-configuration-server/update1.png)

4. Haga doble clic para ejecutar el instalador.
2. El instalador detecta la versión actual que se ejecuta en la máquina. Haga clic en **Sí** para iniciar la actualización. 
3. Cuando la actualización finaliza se valida la configuración del servidor.

    ![Actualizar](./media/vmware-azure-manage-configuration-server/update3.png)

4. Haga clic en **Finish** (Finalizar) para cerrar el instalador.


## <a name="delete-or-unregister-a-configuration-server"></a>Eliminación o anulación del registro de un servidor de configuración

1. [Deshabilite la protección](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) de todas las máquinas virtuales del servidor de configuración.
2. [Desasocie](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) y [elimine](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) todas las directivas de replicación del servidor de configuración.
3. [Elimine](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) todos los servidores vCenter y hosts de vSphere que estén asociados con el servidor de configuración.
4. En el almacén, abra **Site Recovery Infrastructure** >  (Infraestructura de Site Recovery) **Configuration Servers** (Servidores de configuración).
5. Seleccione el servidor de configuración que quiere eliminar. En la página **Details** (Detalles), haga clic en **Delete** (Eliminar).

    ![Eliminación de un servidor de configuración](./media/vmware-azure-manage-configuration-server/delete-configuration-server.png)
   

### <a name="delete-with-powershell"></a>Eliminación con PowerShell

También puede eliminar el servidor de configuración con PowerShell.

1. [Instale](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) el módulo de Azure PowerShell.
2. Inicie sesión en su cuenta de Azure mediante este comando:
    
    `Connect-AzureRmAccount`
3. Seleccione la suscripción de almacén.

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Establezca el contexto de almacén.
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Recupere el servidor de configuración.

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Elimine el servidor de configuración.

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> Puede usar la opción **-Force** de Remove-AzureRmSiteRecoveryFabric para forzar la eliminación del servidor de configuración.
 


## <a name="renew-ssl-certificates"></a>Renovación de certificados SSL

El servidor de configuración lleva integrado un servidor web, que coordina las actividades de Mobility Service, los servidores de procesos y los servidores de destino maestros conectados a él. El servidor web usa un certificado SSL para autenticar clientes. El certificado expira después de tres años y se puede renovar en cualquier momento.

### <a name="check-expiry"></a>Comprobación de expiración

En lo que respecta a las implementaciones de servidores de configuración antes de mayo de 2016, la expiración del certificado se estableció en 1 año. Si tiene un certificado que va a expirar, ocurre lo siguiente:

- Cuando la fecha de expiración es dos meses o menos, el servicio comienza a enviar notificaciones en el portal y por correo electrónico (si está suscrito a las notificaciones de Azure Site Recovery).
- Aparece un banner de notificación en la página de recursos de almacén. Para más información, seleccione el banner.
- Si ve el botón **Actualizar ahora**, quiere decir que algunos componentes en el entorno no se han actualizado a la versión 9.4.xxxx.x o superior. Actualice los componentes antes de renovar el certificado. La renovación no es posible en versiones anteriores.

### <a name="renew-the-certificate"></a>Renovación del certificado

1. En el almacén, abra **Site Recovery Infrastructure** >  (Infraestructura de Site Recovery) **Configuration Servers** (Servidores de configuración). Seleccione el servidor de configuración necesario.
2. La fecha de expiración aparece en **Configuration Server health** (Mantenimiento del servidor de configuración).
3. Haga clic en **Renew Certificates** (Renovar certificados). 


## <a name="next-steps"></a>Pasos siguientes

Revise los tutoriales para configurar la recuperación ante desastres de [máquinas virtuales de VMware](vmware-azure-tutorial.md) en Azure.
