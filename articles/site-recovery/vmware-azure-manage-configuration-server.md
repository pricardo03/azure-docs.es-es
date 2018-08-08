---
title: Administración del servidor de configuración para realizar la recuperación ante desastres de VMware con Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo administrar un servidor de configuración existente para la recuperación ante desastres de VMware en Azure con Azure Site Recovery.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: df5b2ecce2a5c9d7c263ee0acc3a49b859b93f7f
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346127"
---
# <a name="manage-the-configuration-server-for-vmware-vms"></a>Administración del servidor de configuración para máquinas virtuales de VMware

Se configura un servidor de configuración local cuando se usa [Azure Site Recovery](site-recovery-overview.md) para realizar la recuperación ante desastres de servidores físicos y máquinas virtuales de VMware en Azure. El servidor de configuración coordina la comunicación entre Azure y VMware local, además de administrar la replicación de datos. En este artículo se resumen las tareas comunes para administrar el servidor de configuración después de que se haya implementado.



## <a name="modify-vmware-settings"></a>Modificación de la configuración de VMware

Puede acceder al servidor de configuración como se indica a continuación:
    - Inicie sesión en la máquina virtual en que se implementa e inicie el administrador de configuración de Azure Site Recovery desde el acceso directo del escritorio.
    - Como alternativa, puede acceder al servidor de configuración de forma remota desde **https://*NombreDeServidorDeConfiguración*/:44315 /**. Inicie sesión con las credenciales de administrador.
   
### <a name="modify-vmware-server-settings"></a>Modificación de la configuración del servidor de VMware

1. Para asociar otro servidor de VMware con el servidor de configuración, después de iniciar sesión seleccione **Add vCenter Server/vSphere ESXi server** (Agregar servidor vCenter/servidor vSphere ESXi).
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
   ```

      >[!NOTE] 
      >Para **extraer los últimos certificados** del servidor de configuración para la escalabilidad horizontal del servidor de procesos, ejecute el comando *“<Installation Drive\Microsoft Azure Site Recovery\agent\cdpcli.exe>" --registermt*

  8. Por último, reinicie el servicio OBEngine con la ejecución del siguiente comando.
  ```
          net stop obengine
          net start obengine

## Upgrade the configuration server

You run update rollups to update the configuration server. Updates can be applied for up to N-4 versions. For example:

- If you run 9.7, 9.8, 9.9, or 9.10, you can upgrade directly to 9.11.
- If you run 9.6 or earlier and you want to upgrade to 9.11, you must first upgrade to version 9.7. before 9.11.

Links to update rollups for upgrading to all versions of the configuration server are available in the [wiki updates page](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

Upgrade the server as follows:

1. In the vault, go to **Manage** > **Site Recovery Infrastructure** > **Configuration Servers**.
2. If an update is available, a link appears in the **Agent Version** > column.
    ![Update](./media/vmware-azure-manage-configuration-server/update2.png)
3. Download the update installer file to the configuration server.

    ![Update](./media/vmware-azure-manage-configuration-server/update1.png)

4. Double-click to run the installer.
5. The installer detects the current version running on the machine. Click **Yes** to start the upgrade.
6. When the upgrade completes the server configuration validates.

    ![Update](./media/vmware-azure-manage-configuration-server/update3.png)
    
7. Click **Finish** to close the installer.

## Delete or unregister a configuration server

1. [Disable protection](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) for all VMs under the configuration server.
2. [Disassociate](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) and [delete](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) all replication policies from the configuration server.
3. [Delete](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) all vCenter servers/vSphere hosts that are associated with the configuration server.
4. In the vault, open **Site Recovery Infrastructure** > **Configuration Servers**.
5. Select the configuration server that you want to remove. Then, on the **Details** page, select **Delete**.

    ![Delete configuration server](./media/vmware-azure-manage-configuration-server/delete-configuration-server.png)
   

### Delete with PowerShell

You can optionally delete the configuration server by using PowerShell.

1. [Install](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) the Azure PowerShell module.
2. Sign in to your Azure account by using this command:
    
    `Connect-AzureRmAccount`
3. Select the vault subscription.

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Set the vault context.
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault> Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Retrieve the configuration server.

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Delete the configuration server.

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> You can use the **-Force** option in Remove-AzureRmSiteRecoveryFabric for forced deletion of the configuration server.
 
## Generate configuration server Passphrase

1. Sign in to your configuration server, and then open a command prompt window as an administrator.
2. To change the directory to the bin folder, execute the command **cd %ProgramData%\ASR\home\svsystems\bin**
3. To generate the passphrase file, execute **genpassphrase.exe -v > MobSvc.passphrase**.
4. Your passphrase will be stored in the file located at **%ProgramData%\ASR\home\svsystems\bin\MobSvc.passphrase**.

## Renew SSL certificates

The configuration server has an inbuilt web server, which orchestrates activities of the Mobility Service, process servers, and master target servers connected to it. The web server uses an SSL certificate to authenticate clients. The certificate expires after three years and can be renewed at any time.

### Check expiry

For configuration server deployments before May 2016, certificate expiry was set to one year. If you have a certificate that is going to expire, the following occurs:

- When the expiry date is two months or less, the service starts sending notifications in the portal, and by email (if you subscribed to Site Recovery notifications).
- A notification banner appears on the vault resource page. For more information, select the banner.
- If you see an **Upgrade Now** button, it indicates that some components in your environment haven't been upgraded to 9.4.xxxx.x or higher versions. Upgrade the components before you renew the certificate. You can't renew on older versions.

### Renew the certificate

1. In the vault, open **Site Recovery Infrastructure** > **Configuration Server**. Select the required configuration server.
2. The expiry date appears under **Configuration Server health**.
3. Select **Renew Certificates**. 


## Next steps

Review the tutorials for setting up disaster recovery of [VMware VMs](vmware-azure-tutorial.md) to Azure.
