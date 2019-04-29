---
title: Información general de Azure Disk Encryption para VM de IaaS | Microsoft Docs
description: En este artículo se proporciona una introducción a Microsoft Azure Disk Encryption para máquinas virtuales IaaS.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/16/2019
ms.custom: seodec18
ms.openlocfilehash: 66d788aec83e3e57a49b063f2ca80484360f639d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60612052"
---
# <a name="azure-disk-encryption-for-iaas-vms"></a>Azure Disk Encryption para máquinas virtuales IaaS

Microsoft Azure mantiene un compromiso con la garantía de privacidad y soberanía de sus datos. Azure permite controlar los datos hospedados en Azure mediante diferentes tecnologías avanzadas para cifrar, controlar y administrar las claves de cifrado, y controlar y auditar el acceso a los datos. Este control proporciona a los clientes de Azure la flexibilidad necesaria para elegir la solución que mejor cubra sus necesidades empresariales. En este artículo se presenta una solución tecnológica: "Azure Disk Encryption for Windows and Linux Azure Virtual Machines (VMs)" (Azure Disk Encryption para máquinas virtuales IaaS de Linux y Windows). Esta tecnología le ayuda a custodiar y proteger sus datos con el fin de satisfacer los compromisos de cumplimiento y seguridad de su organización. 

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]


## <a name="overview"></a>Información general

Azure Disk Encryption es una funcionalidad que permite cifrar los discos de las máquinas virtuales IaaS con Windows y Linux. Disk Encryption aprovecha la característica [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) de Windows, estándar en el sector, y la característica [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) de Linux para ofrecer cifrado de volumen para los discos de datos y del sistema operativo. La solución se integra con [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) para ayudarle a controlar y administrar las claves y los secreto de cifrado de discos. La solución también garantiza que todos los datos de los discos de máquinas virtuales se cifran en reposo en el almacenamiento de Azure.

Disk Encryption para máquinas virtuales IaaS Windows y Linux tiene disponibilidad general en todas las regiones públicas de Azure y regiones de Azure Government para máquinas virtuales estándar y con Azure Premium Storage. Cuando aplica la solución de administración Disk Encryption, puede satisfacer las siguientes necesidades empresariales:

* Las máquinas virtuales IaaS se protegen en reposo con la tecnología de cifrado estándar del sector para cumplir los requisitos de seguridad y cumplimiento de la organización.
* Las máquinas virtuales IaaS arrancan con directivas y claves controladas por el cliente. Puede auditar su uso en el almacén de claves.

Si utiliza Azure Security Center, se le alertará si tiene máquinas virtuales que no estén cifradas. Estas alertas se muestran con gravedad alta y se recomienda cifrar estas máquinas virtuales.

![Alerta de cifrado de discos en Azure Security Center](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Algunas de las recomendaciones pueden provocar un aumento del uso de datos, de la red o de recursos de proceso, lo que incrementará los costos de las licencias o suscripciones.


## <a name="encryption-scenarios"></a>Escenarios de cifrado

La solución Disk Encryption admite los tres escenarios de cliente siguientes:

* Habilitación del cifrado en nuevas máquinas virtuales IaaS Windows creadas a partir de un disco duro virtual previamente cifrado y claves de cifrado.
* Habilitación del cifrado en las nuevas máquinas virtuales IaaS creadas a partir de imágenes compatibles de la Galería de Azure.
* Habilitación del cifrado en las máquinas virtuales IaaS existentes que se ejecutan en Azure.
* Habilitación del cifrado en conjuntos de escalado de máquinas virtuales Windows.
* Habilitación del cifrado en unidades de datos para conjuntos de escalado de máquinas virtuales Linux.
* Deshabilitación del cifrado en máquinas virtuales IaaS Windows.
* Deshabilitación del cifrado en unidades de datos para máquinas virtuales IaaS Linux.
* Deshabilitación del cifrado en conjuntos de escalado de máquinas virtuales Windows.
* Deshabilitación del cifrado en unidades de datos para conjuntos de escalado de máquinas virtuales Linux.
* Habilitación del cifrado en máquinas virtuales de discos administrados.
* Actualización de la configuración del cifrado de una máquina virtual cifrada con y sin Premium Storage existente.
* Copia de seguridad y restauración de máquinas virtuales cifradas.

La solución admite los siguientes escenarios para las máquinas virtuales IaaS cuando se habilitan en Microsoft Azure:

* Integración con Azure Key Vault.
* VM de nivel estándar: [máquinas virtuales IaaS de las series A, D, DS, G, GS, F, etc.](https://azure.microsoft.com/pricing/details/virtual-machines/) Las [máquinas virtuales Linux](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) dentro de estos niveles deben cumplir los requisitos mínimos de memoria de 7 GB.
* Habilitación del cifrado en las máquinas virtuales IaaS Windows y Linux, disco administrado y máquinas virtuales del conjunto de escalado a partir de imágenes compatibles de la Galería de Azure.
* Deshabilitación del cifrado en sistemas operativos y unidades de datos para máquinas virtuales IaaS Windows, máquinas virtuales de conjunto de escalado y máquinas virtuales de discos administrados.
* Deshabilitación del cifrado en unidades de datos para máquinas virtuales IaaS Windows, máquinas virtuales de conjunto de escalado y máquinas virtuales de discos administrados.
* Habilitación del cifrado en máquinas virtuales IaaS que ejecutan el sistema operativo cliente de Windows.
* Habilitación del cifrado en volúmenes con rutas de montaje.
* Habilitación del cifrado en máquinas virtuales Linux configuradas con seccionamiento de disco (RAID) mediante mdadm.
* Habilitación del cifrado en máquinas virtuales Linux mediante el uso de LVM para discos de datos.
* Habilitación del cifrado en discos de datos y sistema operativo de máquinas virtuales Linux.

   > [!NOTE]
   > No se admite el cifrado de unidades del sistema operativo para algunas distribuciones de Linux. Para más información, consulte el artículo de las [Preguntas más frecuentes de Azure Disk Encryption](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport).
   
* Habilitar el cifrado en máquinas virtuales configuradas con Windows Server 2016 a partir de los espacios de almacenamiento de Windows.
* Actualización de la configuración del cifrado de una máquina virtual cifrada con y sin Premium Storage existente.
* Copia de seguridad y restauración de máquinas virtuales cifradas, para escenarios de clave cifrado con clave (KEK) y otros.
* Se admiten todas las regiones públicas de Azure y las regiones de Azure Government.

La solución no admite los siguientes escenarios, características y tecnologías:

* Máquinas virtuales IaaS de nivel básico.
* Deshabilitación del cifrado en una unidad del sistema operativo para máquinas virtuales IaaS Linux.
* Deshabilitación del cifrado en una unidad de datos cuando la unidad del sistema operativo está cifrada para las máquinas virtuales IaaS Linux.
* Establece el cifrado de unidad del sistema operativo para el escalado de máquinas virtuales de Linux.
* Máquinas virtuales IaaS creadas con el método clásico de generación de máquinas virtuales.
* Habilitación del cifrado en imágenes personalizadas de cliente en máquinas virtuales IaaS Linux.
* Integración con el sistema de administración de claves local.
* Azure Files (sistema de archivos compartido).
* Network File System (NFS).
* Volúmenes dinámicos.
* Máquinas virtuales Windows que estén configuradas con sistemas RAID basados en software.

## <a name="encryption-features"></a>Características de cifrado

Al habilitar e implementar Azure Disk Encryption para las máquinas virtuales IaaS con Azure, se habilitan las funcionalidades siguientes, en función de la configuración proporcionada:

* Cifrado del volumen de sistema operativo para proteger el volumen de arranque en reposo en el almacenamiento.
* Cifrado de volúmenes de datos para proteger los volúmenes de datos en reposo en el almacenamiento.
* Deshabilitación del cifrado en unidades de datos y del sistema operativo para máquinas virtuales IaaS Windows.
* Deshabilitación del cifrado en las unidades de datos de las máquinas virtuales IaaS Linux (solo cuando la unidad del sistema operativo no está cifrada).
* Protección de las claves y secretos de cifrado en la suscripción de Azure Key Vault.
* Notificación del estado de cifrado de la máquina virtual IaaS cifrada.
* Eliminación de las opciones de configuración de cifrado de disco de la máquina virtual IaaS.
* Copia de seguridad y restauración de máquinas virtuales cifradas mediante el servicio Azure Backup.

La solución Azure Disk Encryption para máquinas virtuales IaaS para Windows y Linux incluye lo siguiente:

* La extensión de cifrado de disco para Windows.
* La extensión de cifrado de disco para Linux.
* Los cmdlets de cifrado de disco de PowerShell.
* Los cmdlets de cifrado de disco de la CLI de Azure.
* Las plantillas de cifrado de disco de Azure Resource Manager.

La solución Azure Disk Encryption es compatible con las máquinas virtuales IaaS que ejecutan los sistemas operativos Windows o Linux. Para más información sobre los sistemas operativos compatibles, consulte el artículo [Requisitos previos](azure-security-disk-encryption-prerequisites.md).

> [!NOTE]
> No hay cargo adicional por el cifrado de discos de máquinas virtuales con Azure Disk Encryption. Los [precios de Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) en versión estándar se aplican al almacén de claves usado para almacenar las claves de cifrado. 


## <a name="encryption-workflow"></a>Flujo de trabajo de cifrado

Para habilitar el cifrado de disco para máquinas virtuales Windows y Linux, siga estos pasos:

1. Elija un escenario de cifrado entre los mencionados en la sección [Escenarios de cifrado](#encryption-scenarios).

1. Opte por habilitar el cifrado de disco mediante la plantilla de Resource Manager de Azure Disk Encryption, los cmdlets de PowerShell o la CLI de Azure, y especifique la configuración del cifrado.

   * Para el escenario de VHD cifrado por el cliente, cargue el VHD cifrado a su cuenta de almacenamiento y el material de la clave de cifrado en su almacén de claves. A continuación, proporcione la configuración de cifrado para habilitar el cifrado en una nueva máquina virtual IaaS.
   * Para las nuevas máquinas virtuales creadas desde Marketplace y las máquinas virtuales existentes que ya se ejecutan en Azure, proporcione la configuración para habilitar el cifrado en la máquina virtual IaaS.

1. Conceda acceso a la plataforma Azure para leer el material de la clave de cifrado (sistemas de claves de cifrado de BitLocker para Windows y frase de contraseña para Linux) de su almacén de claves para habilitar el cifrado en la máquina virtual IaaS.

1. Azure actualiza el modelo de servicio de la máquina virtual con la configuración del cifrado y del almacén de claves y configura la máquina virtual cifrada.

   ![Microsoft Antimalware en Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Flujo de trabajo de descifrado
Para deshabilitar el cifrado de disco para las máquinas virtuales IaaS, complete los siguientes pasos de alto nivel:

1. Elija deshabilitar el cifrado (descifrado) en una máquina virtual IaaS en ejecución en Azure y especifique la configuración de descifrado. Puede deshabilitarlo mediante la plantilla de Resource Manager de Azure Disk Encryption, los cmdlets de PowerShell o la CLI de Azure.

   Este paso deshabilita el cifrado del volumen del sistema operativo o de datos (o ambos) en la máquina virtual IaaS con Windows en ejecución. Como se mencionó en la sección anterior, no se permite deshabilitar el cifrado de disco del sistema operativo para Linux. El paso de descifrado se permite solo para unidades de datos en máquinas virtuales Linux siempre que el disco del sistema operativo no esté cifrado.

1. Azure actualiza el modelo de servicio de la máquina virtual y la máquina virtual IaaS se marca como descifrada. El contenido de la máquina virtual ya no se cifra en reposo.

   > [!NOTE]
   > La operación de deshabilitación del cifrado no elimina el almacén de claves y el material de claves de cifrado (claves de cifrado de BitLocker para sistemas Windows y frase de contraseña para Linux).
   >
   > No se admite la deshabilitación del cifrado de disco del sistema operativo para Linux. El paso de descifrado se permite solo para las unidades de datos en máquinas virtuales con Linux.
   >
   > La deshabilitación del cifrado del disco de datos para Linux no se admite si la unidad del sistema operativo está cifrada.


## <a name="encryption-workflow-previous-release"></a>Flujo de trabajo de cifrado (versión anterior)

La nueva versión de Azure Disk Encryption elimina la necesidad de proporcionar un parámetro de aplicación de Azure Active Directory (Azure AD) para habilitar el cifrado de disco de máquina virtual. Con la nueva versión, ya no es necesario proporcionar una credencial de Azure AD durante el paso para habilitar el cifrado. Todas las nuevas máquinas virtuales deben cifrarse sin los parámetros de aplicación de Azure AD, cuando se usa la nueva versión. Las máquinas virtuales que ya se han cifrado con parámetros de la aplicación de Azure AD siguen siendo compatibles y deben continuar manteniéndose con la sintaxis de Azure AD. Para habilitar el cifrado de disco para máquinas virtuales Windows y Linux (versión anterior), siga estos pasos:

1. Elija un escenario de cifrado entre los mencionados en la sección [Escenarios de cifrado](#encryption-scenarios).

1. Opte por habilitar el cifrado de disco mediante la plantilla de Resource Manager de Azure Disk Encryption, los cmdlets de PowerShell o la CLI de Azure, y especifique la configuración del cifrado.

   * Para el escenario de VHD cifrado por el cliente, cargue el VHD cifrado a su cuenta de almacenamiento y el material de la clave de cifrado en su almacén de claves. A continuación, proporcione la configuración de cifrado para habilitar el cifrado en una nueva máquina virtual IaaS.
   * Para las nuevas máquinas virtuales creadas desde Marketplace y las máquinas virtuales existentes que ya se ejecutan en Azure, proporcione la configuración para habilitar el cifrado en la máquina virtual IaaS.

1. Conceda acceso a la plataforma Azure para leer el material de la clave de cifrado (sistemas de claves de cifrado de BitLocker para Windows y frase de contraseña para Linux) de su almacén de claves para habilitar el cifrado en la máquina virtual IaaS.

1. Proporcione la identidad de aplicación de Azure AD para escribir el material de clave de cifrado en su almacén de claves. Este paso habilita el cifrado en la máquina virtual IaaS para los escenarios mencionados en el paso 2.

1. Azure actualiza el modelo de servicio de la máquina virtual con la configuración del cifrado y del almacén de claves y configura la máquina virtual cifrada.


## <a name="terminology"></a>Terminología
En la tabla siguiente se definen algunos de los términos comunes que se usan en esta tecnología:

| Terminología | Definición |
| --- | --- |
| Azure AD | Una cuenta de [Azure AD](https://azure.microsoft.com/documentation/services/active-directory/) se usa para autenticar, almacenar y recuperar secretos de un almacén de claves. |
| Azure Key Vault | Key Vault es un servicio de administración de claves criptográficas basado en módulos de seguridad de hardware validados por el Estándar federal de procesamiento de información (FIPS). Estos estándares ayudan a proteger las claves criptográficas y los secretos confidenciales. Para más información, consulte la documentación de [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) es una tecnología de cifrado de volúmenes de Windows reconocida por el sector que se usa para habilitar el cifrado de disco en máquinas virtuales IaaS con Windows. |
| BEK | Las claves de cifrado de BitLocker (BEK) se usan para cifrar el volumen de arranque del sistema operativo y los volúmenes de datos. Las claves de BitLocker están protegidas en un almacén de claves como secretos. |
| Azure CLI | [La CLI de Azure](/cli/azure/install-azure-cli) está optimizada para administrar recursos de Azure desde la línea de comandos.|
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) es el subsistema de cifrado transparente de disco basado en Linux que se usa para habilitar el cifrado de disco en las máquinas virtuales IaaS con Linux. |
| KEK | La clave de cifrado de claves (KEK) es la clave asimétrica (RSA 2048) que puede usar para proteger o encapsular el secreto. Puede proporcionar una clave protegida mediante módulos de seguridad de hardware (HSM) o una clave protegida mediante software. Para más información, consulte la documentación de [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). |
| Cmdlets de PowerShell | Para más información, consulte [Cmdlets de Azure PowerShell](/powershell/azure/overview). |

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Requisitos previos de Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md)
