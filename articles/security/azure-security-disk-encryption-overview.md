---
title: Introducción a Azure Disk Encryption para máquinas virtuales IaaS| Microsoft Docs
description: En este artículo se proporciona una introducción a Microsoft Azure Disk Encryption para máquinas virtuales IaaS.
services: security
documentationcenter: na
author: mestew
manager: MBaldwin
ms.assetid: 30324adb-e24c-433a-9214-2d1465cbdf45
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2018
ms.author: mstewart
ms.openlocfilehash: 88500be4bae83049e8a7060719f4f85e7622c645
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886998"
---
# <a name="azure-disk-encryption-for-iaas-vms"></a>Azure Disk Encryption para máquinas virtuales IaaS 
Microsoft Azure se ha comprometido a garantizar la privacidad y soberanía de los datos, y a permitirle controlar los datos hospedados en Azure mediante una gama de tecnologías avanzadas que cifran, controlan y administran las claves de cifrado, y controlan y auditan el acceso de los datos. Este control proporciona a los clientes de Azure la flexibilidad necesaria para elegir la solución que mejor cubra sus necesidades empresariales. En este artículo, le presentamos una nueva solución de tecnología, "Azure Disk Encryption para máquinas virtuales IaaS de Windows y Linux", para ayudarle a proteger y salvaguardar sus datos para que satisfagan los compromisos de seguridad y cumplimiento de su organización. 

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]


## <a name="overview"></a>Información general
Azure Disk Encryption (ADE) es una funcionalidad que permite cifrar los discos de las máquinas virtuales IaaS de Windows y Linux. ADE aprovecha la característica [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) estándar del sector de Windows y la característica [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) de Linux para ofrecer cifrado de volumen para los discos de datos y del sistema operativo. La solución se integra con [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) para ayudarle a controlar y administrar las claves y los secreto de cifrado de discos. La solución también garantiza que todos los datos de los discos de máquinas virtuales se cifran en reposo en Azure Storage.

Azure Disk Encryption para máquinas virtuales IaaS de Windows y Linux tiene **disponibilidad general** en todas las regiones públicas de Azure y regiones de AzureGov para máquinas virtuales estándar y con Premium Storage. Cuando se aplica la solución de administración de Azure Disk Encryption, puede atender las siguientes necesidades empresariales:

* Las máquinas virtuales IaaS se protegen en reposo con la tecnología de cifrado estándar del sector para cumplir los requisitos de seguridad y cumplimiento de la organización.
* Las máquinas virtuales IaaS arrancan bajo directivas y claves controladas por el cliente, y puede auditar su uso en su almacén de claves.


Si usa Azure Security Center, le avisará si tiene máquinas virtuales que no estén cifradas. Estas alertas se mostrarán con gravedad alta y se recomienda cifrar estas máquinas virtuales.
![Alerta de cifrado de discos en Azure Security Center](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Algunas de las recomendaciones pueden provocar un aumento del uso de datos, de la red o de recursos de proceso, lo que incrementará los costes de las licencias o suscripciones.


## <a name="encryption-scenarios"></a>Escenarios de cifrado
La solución Azure Disk Encryption admite los tres escenarios de cliente siguientes:

* Habilitación del cifrado en nuevas máquinas virtuales IaaS de Windows creadas a partir de un disco duro virtual previamente cifrado y claves de cifrado 
* Habilitación del cifrado en las nuevas máquinas virtuales IaaS creadas a partir de imágenes compatibles de la Galería de Azure
* Habilitación del cifrado en las máquinas virtuales IaaS existentes que se ejecutan en Azure
* Habilitación del cifrado en conjuntos de escalado de máquinas virtuales Windows
* Habilitación del cifrado en unidades de datos para conjuntos de escalado de máquinas virtuales Linux
* Deshabilitación del cifrado en máquinas virtuales IaaS de Windows
* Deshabilitación del cifrado en unidades de datos para máquinas virtuales IaaS con Linux
* Deshabilitación del cifrado en conjuntos de escalado de máquinas virtuales Windows
* Deshabilitación del cifrado en unidades de datos para conjuntos de escalado de máquinas virtuales Linux
* Habilitación del cifrado en máquinas virtuales de discos administrados
* Actualización de la configuración del cifrado de una máquina virtual cifrada con y sin Premium Storage existente
* Copia de seguridad y restauración de máquinas virtuales cifradas

La solución admite los siguientes escenarios para las máquinas virtuales IaaS cuando se habilitan en Microsoft Azure:

* Integración con Azure Key Vault
* Máquinas virtuales de nivel estándar: [máquinas virtuales IaaS de las series A, D, DS, G, GS, F, etc.](https://azure.microsoft.com/pricing/details/virtual-machines/)
    * Las [máquinas virtuales Linux](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) dentro de estos niveles deben cumplir los requisitos mínimos de memoria de 7 GB
* Habilitación del cifrado en las máquinas virtuales IaaS Windows y Linux, disco administrado y máquinas virtuales del conjunto de escalado a partir de imágenes compatibles de la Galería de Azure
* Deshabilitación del cifrado en sistemas operativos y unidades de datos para máquinas virtuales IaaS Windows, máquinas virtuales de conjunto de escalado y máquinas virtuales de discos administrados
* Deshabilitación del cifrado en unidades de datos para máquinas virtuales IaaS Windows, máquinas virtuales de conjunto de escalado y máquinas virtuales de discos administrados
* Habilitación del cifrado en máquinas virtuales IaaS que ejecutan el sistema operativo cliente de Windows
* Habilitación del cifrado en volúmenes con rutas de montaje
* Habilitación del cifrado en máquinas virtuales de Linux configurado con seccionamiento de disco (RAID) mediante el uso de mdadm
* Habilitación del cifrado en máquinas virtuales de Linux mediante el uso de LVM para discos de datos
* Habilitación del cifrado en discos de datos y sistema operativo de máquinas virtuales Linux 
* Habilitación del cifrado en máquinas virtuales Windows configurado con espacios de almacenamiento
* Actualización de la configuración del cifrado de una máquina virtual cifrada con y sin Premium Storage existente
* Copia de seguridad y restauración de máquinas virtuales cifradas, para escenarios de KEK y no KEK (KEK: Key Encryption Key)
* Se admiten todas las regiones públicas de Azure y las regiones de AzureGov

La solución no admite los siguientes escenarios, características y tecnologías:

* Máquinas virtuales IaaS de nivel básico
* Deshabilitación del cifrado en una unidad del sistema operativo para máquinas virtuales IaaS Linux
* Deshabilitación del cifrado en una unidad de datos si la unidad del sistema operativo está cifrada para las máquinas virtuales Iaas Linux
* Máquinas virtuales IaaS creadas con el método clásico de generación de máquinas virtuales
* Habilitación del cifrado en imágenes personalizadas de cliente de máquinas virtuales IaaS Linux
* Integración con el Servicio de administración de claves local
* Azure Files (recurso compartido de archivos de Azure), Network File System (NFS), volúmenes dinámicos y máquinas virtuales Windows configuradas con sistemas RAID basados en software

## <a name="encryption-features"></a>Características de cifrado
Al habilitar e implementar Azure Disk Encryption para las máquinas virtuales IaaS con Azure, se habilitan las funcionalidades siguientes, dependiendo de la configuración proporcionada:

* Cifrado del volumen de sistema operativo para proteger el volumen de arranque en reposo en el almacenamiento
* Cifrado de volúmenes de datos para proteger los volúmenes de datos en reposo en el almacenamiento
* Deshabilitación del cifrado en unidades de datos y del sistema operativo para máquinas virtuales IaaS con Windows
* Deshabilitación del cifrado en las unidades de datos de las máquinas virtuales IaaS Linux (solo si la unidad del sistema operativo no está cifrada)
* Protección de las claves y secretos de cifrado en la suscripción de Key Vault
* Notificación del estado de cifrado de la máquina virtual IaaS cifrada
* Eliminación de las opciones de configuración de cifrado de disco de la máquina virtual IaaS
* Copia de seguridad y restauración de máquinas virtuales cifradas mediante el servicio Azure Backup

La solución Azure Disk Encryption para máquinas virtuales IaaS para Windows y Linux incluye lo siguiente:

* La extensión de cifrado de disco para Windows.
* La extensión de cifrado de disco para Linux.
* Los cmdlets de PowerShell de cifrado de disco.
* Los cmdlets de la interfaz de la línea de comandos (CLI) de Azure de cifrado de disco.
* Las plantillas de Azure Resource Manager de cifrado de disco.

La solución Azure Disk Encryption es compatible con las máquinas virtuales IaaS que ejecutan los sistemas operativos Windows o Linux. Para más información sobre los sistemas operativos compatibles, consulte el artículo [Requisitos previos](azure-security-disk-encryption-prerequisites.md).

> [!NOTE]
> No hay cargo adicional por el cifrado de discos de máquinas virtuales con Azure Disk Encryption. Los [precios de Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) en versión estándar se aplican al almacén de claves usado para almacenar las claves de cifrado. 


## <a name="encryption-workflow"></a>Flujo de trabajo de cifrado

 Para habilitar el cifrado de disco para máquinas virtuales Windows y Linux, siga estos pasos:

1. Elija un escenario de cifrado entre los escenarios de cifrado anteriores.
2. Opte por habilitar el cifrado de disco mediante la plantilla de Resource Manager de Azure Disk Encryption, los cmdlets PowerShell o el comando de la CLI, y especifique la configuración del cifrado.

   * Para el escenario de VHD cifrado por el cliente, cargue el VHD cifrado a su cuenta de almacenamiento y el material de la clave de cifrado en su almacén de claves. A continuación, proporcione la configuración de cifrado para habilitar el cifrado en una nueva máquina virtual IaaS.
   * Para las nuevas máquinas virtuales creadas a partir de Marketplace y las máquinas virtuales existentes que ya se ejecutan en Azure, proporcione la configuración para habilitar el cifrado en la máquina virtual IaaS.

3. Conceda acceso a la plataforma Azure para leer el material de la clave de cifrado (sistemas de claves de cifrado de BitLocker para Windows y frase de contraseña para Linux) de su almacén de claves para habilitar el cifrado en la máquina virtual IaaS.

4. Azure actualiza el modelo de servicio de la máquina virtual con la configuración del cifrado y del almacén de claves, y configura la máquina virtual cifrada.

 ![Microsoft Antimalware en Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Flujo de trabajo de descifrado
Para deshabilitar el cifrado de disco para las máquinas virtuales IaaS, complete los siguientes pasos de alto nivel:

1. Elija deshabilitar el cifrado (descifrado) en una máquina virtual IaaS en ejecución en Azure y especifique la configuración de descifrado. Puede deshabilitarlo mediante la plantilla de Resource Manager de Azure Disk Encryption, los cmdlets de PowerShell o la CLI de Azure.

 Este paso deshabilita el cifrado del volumen del sistema operativo o de datos (o ambos) en la máquina virtual IaaS con Windows en ejecución. Sin embargo, como se mencionó en la sección anterior, no se permite deshabilitar el cifrado del disco del sistema operativo. El paso de descifrado se permite solo para unidades de datos en máquinas virtuales Linux siempre que el disco del sistema operativo no esté cifrado.
2. Azure actualiza el modelo de servicio de la máquina virtual y la máquina virtual IaaS se marca como descifrada. El contenido de la máquina virtual ya no se cifra en reposo.

> [!NOTE]
> La operación de deshabilitación del cifrado no elimina el almacén de claves y el material de clave de cifrado (claves de cifrado de BitLocker para sistemas Windows y frase de contraseña para Linux).
 > No se admite la deshabilitación del cifrado de disco del sistema operativo para Linux. El paso de descifrado se permite solo para las unidades de datos en máquinas virtuales con Linux.
La deshabilitación del cifrado del disco de datos para Linux no se admite si la unidad del sistema operativo está cifrada.


## <a name="encryption-workflow-previous-release"></a>Flujo de trabajo de cifrado (versión anterior)

La nueva versión de cifrado de disco de Azure elimina la necesidad de proporcionar un parámetro de la aplicación de Azure AD para habilitar el cifrado de disco de máquina virtual. Con la nueva versión, ya no es necesario proporcionar una credencial de Azure AD durante el paso para habilitar el cifrado. Todas las nuevas máquinas virtuales deben estar cifradas sin los parámetros de aplicación de Azure AD con la nueva versión. Las máquinas virtuales que ya se han cifrado con parámetros de la aplicación de Azure AD siguen siendo compatibles y deben continuar manteniéndose con la sintaxis de AAD. Para habilitar el cifrado de disco para máquinas virtuales Windows y Linux (versión anterior), siga estos pasos:

1. Elija un escenario de cifrado entre los escenarios de cifrado anteriores.
2. Opte por habilitar el cifrado de disco mediante la plantilla de Resource Manager de Azure Disk Encryption, los cmdlets PowerShell o el comando de la CLI, y especifique la configuración del cifrado.

   * Para el escenario de VHD cifrado por el cliente, cargue el VHD cifrado a su cuenta de almacenamiento y el material de la clave de cifrado en su almacén de claves. A continuación, proporcione la configuración de cifrado para habilitar el cifrado en una nueva máquina virtual IaaS.
   * Para las nuevas máquinas virtuales creadas a partir de Marketplace y las máquinas virtuales existentes que ya se ejecutan en Azure, proporcione la configuración para habilitar el cifrado en la máquina virtual IaaS.

3. Conceda acceso a la plataforma Azure para leer el material de la clave de cifrado (sistemas de claves de cifrado de BitLocker para Windows y frase de contraseña para Linux) de su almacén de claves para habilitar el cifrado en la máquina virtual IaaS.

4. Proporcione la identidad de aplicación de Azure Active Directory (Azure AD) para escribir el material de clave de cifrado en su almacén de claves. De este modo, se habilita el cifrado en la máquina virtual IaaS para los escenarios mencionados en el paso 2.

5. Azure actualiza el modelo de servicio de la máquina virtual con la configuración del cifrado y del almacén de claves y configura la máquina virtual cifrada.


## <a name="terminology"></a>Terminología
Para comprender algunos de los términos comunes que usa esta tecnología, utilice la siguiente tabla de terminología:

| Terminología | Definición |
| --- | --- |
| Azure AD | Azure AD es [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Una cuenta de Azure AD se usa para autenticar, almacenar y recuperar secretos del almacén de claves. |
| Azure Key Vault | Key Vault es un servicio de administración de claves criptográficas basado en módulos de seguridad de hardware validados por el Estándar federal de procesamiento de información (FIPS), que ayuda a proteger las claves criptográficas y los secretos confidenciales. Para obtener más información, consulte la documentación de [Key Vault](https://azure.microsoft.com/services/key-vault/). |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) es una tecnología de cifrado de volúmenes de Windows reconocida por el sector que se usa para habilitar el cifrado de disco en máquinas virtuales IaaS con Windows. |
| BEK | Las claves de cifrado de BitLocker se usan para cifrar el volumen de arranque del sistema operativo y los volúmenes de datos. Las claves de BitLocker están protegidas en un almacén de claves como secretos. |
| CLI | Vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).|
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) es el subsistema de cifrado transparente de disco basado en Linux que se usa para habilitar el cifrado de disco en las máquinas virtuales IaaS con Linux. |
| KEK | La clave de cifrado de claves (KEK) es la clave asimétrica (RSA 2048) que puede usar para proteger o encapsular el secreto. Puede proporcionar una clave protegida mediante módulos de seguridad de hardware (HSM) o una clave protegida mediante software. Para más información, consulte la documentación de [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). |
| cmdlets de PS | Consulte [Cmdlets de Azure PowerShell](/powershell/azure/overview). |

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Requisitos previos de Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md)
