---
title: Conectarse a Azure Stack con CLI | Microsoft Docs
description: Obtenga información sobre cómo usar la interfaz de la línea de comandos (CLI) multiplataforma para administrar e implementar recursos en Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 6042aa4dd8b26a0986737edc3c89b8e165ae970a
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49067710"
---
# <a name="use-api-version-profiles-with-azure-cli-in-azure-stack"></a>Uso de los perfiles de la versión de la API con la CLI de Azure en Azure Stack

Puede seguir los pasos de este artículo para configurar la interfaz de la línea de comandos (CLI) de Azure para administrar los recursos del Kit de desarrollo de Azure Stack de las plataformas de cliente de Linux, Mac y Windows.

## <a name="install-cli"></a>Instalar la CLI

Inicie sesión en la estación de trabajo de desarrollo e instale la CLI. Azure Stack requiere la versión 2.0 o superior de la CLI de Azure. Puede instarla usando los pasos descritos en el artículo [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). Para comprobar si la instalación se realizó correctamente, abra un terminal o una ventana del símbolo del sistema y ejecute el siguiente comando:

```azurecli
az --version
```

Debería ver la versión de la CLI de Azure y otras bibliotecas dependientes instaladas en su equipo.

## <a name="trust-the-azure-stack-ca-root-certificate"></a>Confiar en el certificado de raíz de CA de Azure Stack

1. Obtenga el certificado raíz de CA de Azure Stack del [operador de Azure Stack](..\azure-stack-cli-admin.md#export-the-azure-stack-ca-root-certificate) y confíe en él. Para confiar en el certificado de raíz de CA de Azure Stack, debe anexarlo al certificado existente de Python.

1. Busque la ubicación del certificado en la máquina. La ubicación puede variar dependiendo de dónde haya instalado Python. Debe tener [pip](https://pip.pypa.io) y el módulo [certifi](https://pypi.org/project/certifi/) instalado. Puede usar el siguiente comando de Python desde el símbolo del sistema de bash:

  ```bash  
    python -c "import certifi; print(certifi.where())"
  ```

  Tome nota de la ubicación del certificado. Por ejemplo, `~/lib/python3.5/site-packages/certifi/cacert.pem`. La ruta de acceso particular dependerá del sistema operativo y de la versión de Python que esté instalada.

### <a name="set-the-path-for-a-development-machine-inside-the-cloud"></a>Establecimiento de la ruta de acceso para una máquina de desarrollo dentro de la nube

Si está ejecutando la CLI desde una máquina Linux creada dentro del entorno de Azure Stack, ejecute el siguiente comando de bash con la ruta de acceso al certificado.

```bash
sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
```

### <a name="set-the-path-for-a-development-machine-outside-the-cloud"></a>Establecimiento de la ruta de acceso para una máquina de desarrollo fuera de la nube

Si va a ejecutar la CLI en una máquina **fuera** del entorno de Azure Stack:  

1. Debe configurar la [conectividad VPN a Azure Stack](azure-stack-connect-azure-stack.md).

1. Copie el certificado PEM que obtuvo del operador de Azure Stack y anote la ubicación del archivo (PATH_TO_PEM_FILE).

1. Ejecute los siguientes comandos, dependiendo del sistema operativo de la estación de trabajo de desarrollo.

#### <a name="linux"></a>Linux

```bash
sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
```

#### <a name="macos"></a>macOS

```bash
sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
```

#### <a name="windows"></a>Windows

```powershell
$pemFile = "<Fully qualified path to the PEM certificate Ex: C:\Users\user1\Downloads\root.pem>"

$root = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$root.Import($pemFile)

Write-Host "Extracting needed information from the cert file"
$md5Hash    = (Get-FileHash -Path $pemFile -Algorithm MD5).Hash.ToLower()
$sha1Hash   = (Get-FileHash -Path $pemFile -Algorithm SHA1).Hash.ToLower()
$sha256Hash = (Get-FileHash -Path $pemFile -Algorithm SHA256).Hash.ToLower()

$issuerEntry  = [string]::Format("# Issuer: {0}", $root.Issuer)
$subjectEntry = [string]::Format("# Subject: {0}", $root.Subject)
$labelEntry   = [string]::Format("# Label: {0}", $root.Subject.Split('=')[-1])
$serialEntry  = [string]::Format("# Serial: {0}", $root.GetSerialNumberString().ToLower())
$md5Entry     = [string]::Format("# MD5 Fingerprint: {0}", $md5Hash)
$sha1Entry    = [string]::Format("# SHA1 Fingerprint: {0}", $sha1Hash)
$sha256Entry  = [string]::Format("# SHA256 Fingerprint: {0}", $sha256Hash)
$certText = (Get-Content -Path $pemFile -Raw).ToString().Replace("`r`n","`n")

$rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
$serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

Write-Host "Adding the certificate content to Python Cert store"
Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

Write-Host "Python Cert store was updated for allowing the azure stack CA root certificate"
```

## <a name="get-the-virtual-machine-aliases-endpoint"></a>Obtención del punto de conexión de alias de máquina virtual

Antes de que los usuarios puedan crear máquinas virtuales mediante la CLI, deben ponerse en contacto con el operador de Azure Stack y obtener el identificador URI del punto de conexión de alias de máquina virtual. Por ejemplo, Azure usa el URI siguiente: https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json. El administrador de la nube debe configurar un punto de conexión similar para Azure Stack con las imágenes que están disponibles en Marketplace de Azure Stack. Los usuarios deben pasar el identificador URI del punto de conexión en el parámetro `endpoint-vm-image-alias-doc` del comando `az cloud register` tal como se muestra en la siguiente sección. 
   

## <a name="connect-to-azure-stack"></a>Conexión a Azure Stack

Use los pasos siguientes para conectarse a Azure Stack:

1. Registre el entorno de Azure Stack. Para ello, ejecute el comando `az cloud register`.
   
   a. Para registrar el entorno *administrativo de nube*, use:

      ```azurecli
      az cloud register \ 
        -n AzureStackAdmin \ 
        --endpoint-resource-manager "https://adminmanagement.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".adminvault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```

   b. Para registrar el entorno de *usuario*, use:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```

1. Establezca el entorno activo mediante los comandos siguientes.

   a. Para el entorno *administrativo de nube*, use:

      ```azurecli
      az cloud set \
        -n AzureStackAdmin
      ```

   b. Para el entorno de *usuario*, use:

      ```azurecli
      az cloud set \
        -n AzureStackUser
      ```

1. Actualice la configuración del entorno para usar el perfil de la versión de API específico de Azure Stack. Para actualizar la configuración, ejecute el comando siguiente:

   ```azurecli
   az cloud update \
     --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Si ejecuta una versión de Azure Stack anterior a la compilación 1808, tendrá que tener que usar el perfil de la versión de API **2017-03-09-profile** en lugar de **2018-03-01-hybrid**.

1. Inicie sesión en el entorno de Azure Stack. Para ello, use el comando `az login`. Puede iniciar sesión en el entorno de Azure Stack como un usuario o una [entidad de servicio](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects). 

    * Entornos de AAD
      * Iniciar sesión como un *usuario*: puede especificar el nombre de usuario y la contraseña directamente en el comando `az login` o autenticarse mediante un explorador. Deberá hacer esto último, si la cuenta tiene habilitada la autenticación multifactor.

      ```azurecli
      az login \
        -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
      ```

      > [!NOTE]
      > Si su cuenta de usuario tiene la autenticación multifactor habilitada, puede usar `az login command` sin proporcionar el parámetro `-u`. Al ejecutar el comando, se le proporciona una URL y un código que debe usar para la autenticación.
   
      * Iniciar sesión como una *entidad de servicio*: antes de iniciar sesión, [cree una entidad de servicio a través de Azure Portal](azure-stack-create-service-principals.md) o la CLI y asígnele un rol. Ahora, inicie sesión con el siguiente comando:

      ```azurecli
      az login \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
        --service-principal \
        -u <Application Id of the Service Principal> \
        -p <Key generated for the Service Principal>
      ```
    * Entornos de AD FS

        * Inicie sesión como una *entidad de servicio*: 
          1.    Prepare el archivo .pem que se va a usar para el inicio de sesión de la entidad de servicio.
                * En el equipo cliente donde se ha creado la entidad de seguridad, exporte el certificado de la entidad de servicio como un archivo pfx con la clave privada (ubicada en cert:\CurrentUser\My; el nombre del certificado es igual que el de la entidad de seguridad).

                *   Convierta el archivo pfx a pem (use la utilidad OpenSSL).

          1.    Inicie sesión en la CLI. :
                ```azurecli
                az login --service-principal \
                 -u <Client ID from the Service Principal details> \
                 -p <Certificate's fully qualified name. Eg. C:\certs\spn.pem>
                 --tenant <Tenant ID> \
                 --debug 
                ```

## <a name="test-the-connectivity"></a>Prueba de la conectividad

Ahora que todo está configurado, vamos a usar la CLI para crear recursos en Azure Stack. Por ejemplo, puede crear un grupo de recursos para una aplicación y agregar una máquina virtual. Use el comando siguiente para crear un grupo de recursos denominado "MyResourceGroup":

```azurecli
az group create \
  -n MyResourceGroup -l local
```

Si el grupo de recursos se crea correctamente, el comando anterior da como resultado las siguientes propiedades del recurso recién creado:

![Resultado de creación del grupo de recursos](media/azure-stack-connect-cli/image1.png)

## <a name="known-issues"></a>Problemas conocidos
Hay algunos problemas conocidos que debe tener en cuenta cuando use la CLI en Azure Stack:

 - El modo interactivo de la CLI, por ejemplo, el comando `az interactive` no se admite todavía en Azure Stack.
 - Para obtener la lista de imágenes de máquinas virtuales disponibles en Azure Stack, use el comando `az vm images list --all` en lugar del comando `az vm image list`. Al especificar la opción `--all`, se asegura de que la respuesta devuelve únicamente las imágenes que estén disponibles en el entorno de Azure Stack.
 - Los alias de las imágenes de máquinas virtuales que están disponibles en Azure pueden no ser aplicables a Azure Stack. Al utilizar imágenes de máquinas virtuales, debe utilizar todo el parámetro URN (Canonical:UbuntuServer:14.04.3-LTS:1.0.0) en lugar del alias de la imagen. Este URN debe coincidir con las especificaciones de la imagen como se derivan del comando `az vm images list`.

## <a name="next-steps"></a>Pasos siguientes

[Implementación de plantillas con la CLI de Azure](azure-stack-deploy-template-command-line.md)

[Habilitación de la CLI de Azure para usuarios de Azure Stack (operador)](..\azure-stack-cli-admin.md)

[Administración de permisos de usuario](azure-stack-manage-permissions.md)
