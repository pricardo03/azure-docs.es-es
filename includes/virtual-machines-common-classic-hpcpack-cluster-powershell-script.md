---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 6f0d2d59ed50c743adb19027c404bfa83a1886f1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61485614"
---
En función del entorno y las opciones, el script puede crear toda la infraestructura del clúster, como la red virtual de Azure, las cuentas de almacenamiento, los servicios en la nube, los controladores de dominio, las bases de datos SQL locales o remotas, el nodo principal y los nodos de clúster adicionales. Como alternativa, el script puede usar la infraestructura de Azure ya existente y crear solo los nodos del clúster HPC.

Para más información general acerca de cómo planear un clúster de HPC Pack, consulte el contenido de [Product Evaluation and Planning](https://technet.microsoft.com/library/jj899596.aspx) (Planeamiento y evaluación del producto) y de [Getting Started](https://technet.microsoft.com/library/jj899590.aspx) (Introducción) en la Biblioteca de TechNet de HPC Pack 2012 R2.

## <a name="prerequisites"></a>Requisitos previos
* **Suscripción de Azure**: Puede usar una suscripción en el servicio Azure Global o Azure China. Los límites de su suscripción afectarán al número y al tipo de nodos de clúster que puede implementar. Para obtener información, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../articles/azure-subscription-service-limits.md).
* **Equipo cliente de Windows con Azure PowerShell 0.8.10 o posterior instalado y configurado**: Consulte [empezar a trabajar con Azure PowerShell](/powershell/azureps-cmdlets-docs) para obtener instrucciones de instalación y pasos para conectarse a su suscripción de Azure.
* **Script de implementación de HPC Pack IaaS**: Descargue y descomprima la versión más reciente del script desde el [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Compruebe la versión del script ejecutando `New-HPCIaaSCluster.ps1 –Version`. Este artículo se basa en la versión 4.5.2 del script.
* **Archivo de configuración de la secuencia de comandos**: Cree un archivo XML que usa la secuencia de comandos para configurar el clúster HPC. Para más información, junto con ejemplos, vea las secciones más adelante en este artículo y el archivo Manual.rtf que acompaña al script de implementación.

## <a name="syntax"></a>Sintaxis
```powershell
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
> [!NOTE]
> Ejecute el script como administrador.
> 
> 

### <a name="parameters"></a>Parámetros
* **ConfigFile**: Especifica la ruta de acceso del archivo de configuración para describir el clúster de HPC. Puede leer más sobre el archivo de configuración en este tema, o en el archivo Manual.rtf de la carpeta que contiene el script.
* **AdminUserName**: Especifica el nombre de usuario. Si el script crea el bosque de dominio, esto se convierte en el nombre de usuario del administrador local para todas las máquinas virtuales, así como el nombre del administrador de dominio. Si ya existe un bosque de dominio, esto especifica el usuario de dominio como nombre de usuario del administrador local para instalar HPC Pack.
* **AdminPassword**: Especifica la contraseña del administrador. Si no se especifica en la línea de comandos, el script solicitará que escriba la contraseña.
* **HPCImageName** (opcional): Especifica el nombre de la imagen de VM de HPC Pack usado para implementar el clúster HPC. Debe ser una imagen de HPC Pack proporcionada por Microsoft desde Azure Marketplace. Si no se especifica (recomendado en la mayoría de los casos), el script elige la última [imagen de HPC Pack 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) publicada. La última imagen se basa en Windows Server 2012 R2 Datacenter con HPC Pack 2012 R2 Update 3 instalado.
  
  > [!NOTE]
  > Se producirá un error en la implementación si no se indica una imagen de HPC Pack válida.
  > 
  > 
* **LogFile** (optional): Especifica la ruta de acceso de archivo de registro de implementación. Si no se especifica, el script creará un archivo de registro en el directorio temporal del equipo que ejecuta el script.
* **Forzar** (opcional): Suprime todos los mensajes de confirmación.
* **NoCleanOnFailure** (optional): Especifica que no se quitan las máquinas virtuales de Azure que no se implementaron correctamente. Quite manualmente estas máquinas virtuales antes de volver a ejecutar el script para continuar la implementación; de lo contrario, puede producirse un error en la implementación.
* **PSSessionSkipCACheck** (opcional): Para cada servicio en la nube con máquinas virtuales implementadas por este script, Azure genera automáticamente un certificado autofirmado y todas las máquinas virtuales en el servicio en la nube usan este certificado como el certificado de administración remota de Windows (WinRM) predeterminada. Para implementar características de HPC en estas máquinas virtuales de Azure, de manera predeterminada el script instala temporalmente dichos certificados en el equipo local\\almacén de entidades de certificación raíz de confianza del equipo cliente para que no aparezca el error de seguridad La entidad de certificación no es de confianza durante la ejecución del script. Los certificados se quitan cuando finaliza el script. Si se especifica este parámetro, los certificados no se instalan en el equipo cliente y se suprime la advertencia de seguridad.
  
  > [!IMPORTANT]
  > No se recomienda el uso de este parámetro para implementaciones de producción.
  > 
  > 

### <a name="example"></a>Ejemplo
En el ejemplo siguiente se crea un clúster de HPC Pack mediante el archivo de configuración *MyConfigFile.xml* y se especifican las credenciales administrativas para instalar el clúster.

```powershell
.\New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### <a name="additional-considerations"></a>Consideraciones adicionales
* El script puede, opcionalmente, habilitar el envío de trabajos mediante el portal web de HPC Pack o la API de REST de HPC Pack.
* El script puede, opcionalmente, ejecutar scripts personalizados de configuración previa y posterior en el nodo principal si desea instalar software adicional o configurar otras opciones.

## <a name="configuration-file"></a>Archivo de configuración
El archivo de configuración para el script de implementación es un archivo XML. El archivo de esquema HPCIaaSClusterConfig.xsd está en la carpeta de scripts de implementación de HPC Pack IaaS. **IaaSClusterConfig** es el elemento raíz del archivo de configuración que contiene los elementos secundarios descritos en detalle en el archivo Manual.rtf de la carpeta de scripts de implementación.

