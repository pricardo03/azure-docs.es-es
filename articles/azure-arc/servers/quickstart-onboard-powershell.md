---
title: 'Inicio rápido: Conexión de máquinas a Azure con Azure Arc para servidores mediante PowerShell'
description: En este inicio rápido aprenderá a conectar máquinas a Azure con Azure Arc para servidores mediante PowerShell
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: bobbytreed
ms.author: robreed
keywords: azure automation, DSC, powershell, desired state configuration, update management, change tracking, inventory, runbooks, python, graphical, hybrid, onboard
ms.date: 11/04/2019
ms.custom: mvc
ms.topic: quickstart
ms.openlocfilehash: 91d8ddf7d8051baeb42ceb58673c93555908f03a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488181"
---
# <a name="quickstart-connect-machines-to-azure-using-azure-arc-for-servers---powershell"></a>Inicio rápido: Conexión de máquinas a Azure con Azure Arc para servidores mediante PowerShell

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Revise los clientes compatibles y la configuración de red necesaria en la [introducción de Azure Arc para servidores](overview.md).

## <a name="create-a-service-principal-for-onboarding-at-scale"></a>Creación de una entidad de servicio para la incorporación a escala

Una entidad de servicio es una identidad de administración limitada especial a la que solo se concede el permiso mínimo necesario para conectar máquinas a Azure. Esto es más seguro que usar una cuenta con más permisos, como la de administrador de inquilinos. La entidad de servicio solo se utiliza durante la incorporación. Puede eliminar la entidad de servicio de forma segura después de conectar los servidores deseados.

> [!NOTE]
> Este paso es recomendable, pero no obligatorio.

### <a name="steps-to-create-the-service-principal"></a>Pasos de creación de la entidad de servicio

En este ejemplo, vamos a utilizar [Azure PowerShell](/powershell/azure/install-az-ps) para crear un nombre de entidad de seguridad de servicio (SPN). Como alternativa, puede seguir los pasos que se indican en [Creación de una entidad de servicio con Azure Portal](../../active-directory/develop/howto-create-service-principal-portal.md) para esta tarea.

El rol `Azure Connected Machine Onboarding` contiene solo los permisos necesarios para la incorporación. Puede definir el permiso de un SPN para permitir que su ámbito incluya un grupo de recursos o una suscripción.

Debe almacenar la salida del cmdlet [`New-AzADServicePrincipal`](/powershell/module/az.resources/new-azadserviceprincipal) o no podrá recuperar la contraseña para usarla en un paso posterior.

```azurepowershell-interactive
$sp = New-AzADServicePrincipal -DisplayName "Arc-for-servers" -Role "Azure Connected Machine Onboarding"
$sp
```

```output
Secret                : System.Security.SecureString
ServicePrincipalNames : {ad9bcd79-be9c-45ab-abd8-80ca1654a7d1, https://Arc-for-servers}
ApplicationId         : ad9bcd79-be9c-45ab-abd8-80ca1654a7d1
ObjectType            : ServicePrincipal
DisplayName           : Hybrid-RP
Id                    : 5be92c87-01c4-42f5-bade-c1c10af87758
Type                  :
```

Ahora, recupere la contraseña con PowerShell.

```azurepowershell-interactive
$credential = New-Object pscredential -ArgumentList "temp", $sp.Secret
$credential.GetNetworkCredential().password
```

De la salida, copie los valores de **password** y **ApplicationId** (del paso anterior) y almacénelos para después en un lugar seguro como el almacén de secretos de la herramienta de configuración del servidor. Si olvida o pierde la contraseña de SPN, puede restablecerla mediante el cmdlet [`New-AzADSpCredential`](/powershell/module/azurerm.resources/new-azurermadspcredential).

En el script de incorporación del agente de instalación:

* La propiedad **ApplicationId** se utiliza para el parámetro `--service-principal-id` que se usa en el agente de instalación.
* La propiedad **password** se utiliza para el parámetro `--service-principal-secret` del agente de instalación.

## <a name="manually-install-the-agent-and-connect-to-azure"></a>Instalación manual del agente y conexión a Azure

La guía siguiente le permite conectar una máquina a Azure mediante el inicio de sesión en la máquina y la realización de los pasos. También puede conectar máquinas a Azure [desde el portal](quickstart-onboard-portal.md).

### <a name="download-and-install-the-agent"></a>Descarga e instalación del agente

La instalación del paquete del agente requiere acceso raíz o de administrador local en el servidor de destino, pero no acceso a Azure.

#### <a name="linux"></a>Linux

En el caso de los servidores de **Linux**, el agente se distribuye a través del [repositorio de paquetes de Microsoft](https://packages.microsoft.com) con el formato de paquete preferido para la distribución (.RPM o .DEB).

> [!NOTE]
> Durante la versión preliminar pública, solo se ha lanzado un paquete, que es adecuado para Ubuntu 16.04 o 18.04.

<!-- What about this aks? -->
La opción más sencilla es registrar el repositorio de paquetes y, a continuación, instalar el paquete mediante el administrador de paquetes de la distribución.
El script de bash que se encuentra en [https://aka.ms/azcmagent](https://aka.ms/azcmagent) realiza las acciones siguientes:

1. Configura la máquina host para la descarga desde `packages.microsoft.com`.
2. Instala el paquete del proveedor de recursos híbridos.
3. Opcionalmente, configura el agente para la operación de proxy, si se especifica `--proxy`.

El script también contiene comprobaciones para las distribuciones admitidas y no admitidas, así como para detectar los permisos necesarios para la instalación.

En el ejemplo siguiente se descarga el agente y se instala sin ninguna de las comprobaciones condicionales.

```bash
# Download the installation package
wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

# Install the connected machine agent. Omit the '--proxy "{proxy-url}"' parameters if proxy is not needed
bash ~/Install_linux_azcmagent.sh--proxy "{proxy-url}"
```

> [!NOTE]
> Si prefiere no hacer referencia al repositorio de paquetes de Microsoft, puede copiar el archivo del paquete desde allí en el repositorio interno.

#### <a name="windows"></a>Windows

En el caso de **Windows**, el agente se empaqueta en un archivo (`.MSI`) de Windows Installer y se puede descargar desde [https://aka.ms/AzureConnectedMachineAgent](https://aka.ms/AzureConnectedMachineAgent) que está hospedado en [https://download.microsoft.com](https://download.microsoft.com).

```powershell
# Download the package
Invoke-WebRequest -Uri https://aka.ms/AzureConnectedMachineAgent -OutFile AzureConnectedMachineAgent.msi

# Install the package
msiexec /i AzureConnectedMachineAgent.msi /l*v installationlog.txt /qn | Out-String
```

> [!NOTE]
> En Linux, ejecutar el script de instalación de nuevo hará que se actualice automáticamente a la versión más reciente. En Windows, debe desinstalar el "agente de máquinas conectadas de Azure" antes de volver a ejecutar el instalador para actualizar.

### <a name="connecting-to-azure"></a>Conexión con Azure

Una vez instalado, puede administrar y configurar el agente mediante una herramienta de línea de comandos denominada `azcmagent.exe`. El agente se encuentra en `/opt/azcmagent/bin` en Linux y en `$env:programfiles\AzureConnectedMachineAgent` en Windows.

En Windows, abra PowerShell como administrador en un nodo de destino y ejecute:

```powershell
& "$env:ProgramFiles\AzureConnectedMachineAgent\azcmagent.exe" connect `
  --service-principal-id "{your-spn-appid}" `
  --service-principal-secret "{your-spn-password}" `
  --resource-group "{your-resource-group-name}" `
  --tenant-id "{your-tenant-id}" `
  --location "{location-of-your-resource-group}" `
  --subscription-id "{your-subscription-id}"
```

En Linux, abra un shell y ejecute:

<!-- Same command for linux?-->
```bash
azcmagent connect \
  --service-principal-id "{your-spn-appid}" \
  --service-principal-secret "{your-spn-password}" \
  --resource-group "{your-resource-group-name}" \
  --tenant-id "{your-tenant-id}" \
  --location "{location-of-your-resource-group}" \
  --subscription-id "{your-subscription-id}"
```

Parámetros:

* `tenant-id`: el identificador único del inquilino. Para encontrarlo en Azure Portal, seleccione **Azure Active Directory** -> **Propiedades** -> **Id. de directorio**.
* `subscription-id`: el identificador único de la suscripción, en Azure, en la que desea conectar la máquina.
* `resource-group`: el grupo de recursos en el que desea que se conecte la máquina.
* `location`: consulte las [regiones y ubicaciones de Azure](https://azure.microsoft.com/global-infrastructure/regions/). Esta ubicación puede ser la misma, u otra diferente, que la del grupo de recursos. En la versión preliminar pública, el servicio se admite en **WestUS2** y **Oeste de Europa**.
* `resource-name`:  (*opcional*) Se usa para la representación de recursos de Azure de la máquina local. Si no especifica este valor, se usará el nombre de host de la máquina.

Puede encontrar más información sobre la herramienta "azcmagent" en la [referencia de Azcmagent](azcmagent-reference.md).
<!-- Isn't this still needed to view machines? -->

Una vez finalizado el proceso correctamente, la máquina se conecta a Azure. Para ver la máquina en Azure Portal visite [https://aka.ms/hybridmachineportal](https://aka.ms/hybridmachineportal).

![Incorporación correcta](./media/quickstart-onboard/arc-for-servers-successful-onboard.png)

### <a name="proxy-server-configuration"></a>Configuración del servidor proxy

#### <a name="linux"></a>Linux

<!-- New proxy name? -->

En el caso de **Linux**, si el servidor requiere un servidor proxy, puede:

* Ejecutar el script `install_linux_hybrid_agent.sh` desde la sección [Instalación del agente](#download-and-install-the-agent) anterior, con `--proxy`.
* Si ya ha instalado el agente, ejecute el comando `/opt/azcmagent/bin/hybridrp_proxy add https://{proxy-url}:{proxy-port}`, que configura el proxy y reinicia el agente.

#### <a name="windows"></a>Windows

En el caso de **Windows**, si el servidor requiere un servidor proxy para el acceso a los recursos de Internet, debe ejecutar el comando siguiente para establecer la variable de entorno del servidor proxy. Esto permite al agente usar el servidor proxy para el acceso a Internet.

```powershell
# If a proxy server is needed, execute these commands with actual proxy URL
[Environment]::SetEnvironmentVariable("https_proxy", "{https:\\proxy-url:proxyport}", "Machine")
$env:https_proxy = [System.Environment]::GetEnvironmentVariable("https_proxy","Machine")
# The agent service needs to be restarted after the proxy environment variable is set in order for the changes to take effect.
Restart-Service -Name himds
```

> [!NOTE]
> Los servidores proxy autenticados no se admiten en la versión preliminar pública.

## <a name="clean-up"></a>Limpieza

Para desconectar una máquina de Azure Arc para servidores, debe realizar dos pasos.

1. Seleccione la máquina en [Portal](https://aka.ms/hybridmachineportal), haga clic en los puntos suspensivos (`...`) y seleccione **Eliminar**.
1. Desinstale el agente de la máquina.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Asignación de una directiva a máquinas conectadas](../../governance/policy/assign-policy-portal.md)
