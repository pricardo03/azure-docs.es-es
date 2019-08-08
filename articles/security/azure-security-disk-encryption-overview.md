---
title: ¿Qué es Azure Disk Encryption?
description: En este artículo se proporciona una introducción a Azure Disk Encryption
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 07/29/2019
ms.custom: seodec18
ms.openlocfilehash: c5e568dd073376295e4865994fba8ae5b5ac59a0
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640996"
---
# <a name="azure-disk-encryption-overview"></a>Introducción a Azure Disk Encryption

Azure Disk Encryption ayuda a custodiar y proteger sus datos con el fin de satisfacer los compromisos de cumplimiento y seguridad de su organización. Usa la característica [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) de Windows y la característica [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) de Linux para ofrecer cifrado de volumen para los discos de datos y del sistema operativo en las máquinas virtuales (VM) de Azure. También se integra con [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) para ayudar a controlar y administrar las claves y los secretos de cifrado de discos, y garantiza que todos los datos de los discos de la VM se cifran en reposo mientras se encuentren en el almacenamiento de Azure. Azure Disk Encryption para VM Windows y Linux tiene disponibilidad general en todas las regiones públicas de Azure y regiones de Azure Government para VM estándar y VM con Azure Premium Storage. 

Si utiliza Azure Security Center, se le alertará si tiene máquinas virtuales que no estén cifradas. Estas alertas se muestran con gravedad alta y se recomienda cifrar estas máquinas virtuales.

![Alerta de cifrado de discos en Azure Security Center](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Algunas de las recomendaciones pueden provocar un aumento del uso de datos, de la red o de recursos de proceso, lo que incrementará los costos de las licencias o suscripciones.


## <a name="encryption-scenarios"></a>Escenarios de cifrado

Con Azure Disk Encryption, puede abordar los requisitos de cumplimiento y seguridad de la organización al proteger las VM de Azure en reposo con tecnología de cifrado estándar del sector. También puede configurar las VM para que arranquen según las directivas claves controladas por el cliente (BYOK) y auditar el uso de estas claves en el almacén de claves.

Azure Disk Encryption admite los tres escenarios de cliente siguientes:

* Habilitación y deshabilitación del cifrado en nuevas VM creadas a partir de imágenes admitidas de la galería de Azure.
* Habilitación y deshabilitación del cifrado en VM existentes que se ejecutan en Azure.
* Habilitación y deshabilitación del cifrado en nuevas VM Windows creadas a partir de un disco duro virtual previamente cifrado y claves de cifrado.
* Habilitación y deshabilitación del cifrado en conjunto de escalado de máquinas virtuales Windows.
* Habilitación y deshabilitación del cifrado en unidades de datos para conjuntos de escalado de máquinas virtuales Linux.
* Habilitación y deshabilitación del cifrado de VM de disco administrado.
* Actualización de la configuración del cifrado de una VM cifrada existente con y sin Premium Storage.
* Copia de seguridad y restauración de VM cifradas.
* Escenarios de Bring Your Own Encryption (BYOE) y Bring Your Own Key (BYOK), en los que los clientes usan sus propias claves de cifrado y las almacenan en un almacén de claves de Azure.

También admite los siguientes escenarios para las VM cuando se habilitan en Microsoft Azure:

* Integración con Azure Key Vault.
* [VM de nivel estándar](https://azure.microsoft.com/pricing/details/virtual-machines/) que cumplen el [requisito mínimo de memoria](azure-security-disk-encryption-prerequisites.md#supported-vm-sizes). 
* Habilitación del cifrado en VM Windows y Linux, disco administrado y VM del conjunto de escalado a partir de imágenes admitidas de la galería de Azure.
* Deshabilitación del cifrado en unidades de SO y datos para VM Windows, VM de conjunto de escalado y VM de discos administrados.
* Deshabilitación del cifrado en unidades de datos para VM Linux, VM de conjunto de escalado y VM de discos administrados.
* Habilitación del cifrado en VM que ejecutan el sistema operativo cliente Windows.
* Habilitación del cifrado en volúmenes con rutas de montaje.
* Habilitación del cifrado en VM Linux configuradas con seccionamiento de disco (RAID) mediante mdadm.
* Habilitación del cifrado en VM Linux mediante el uso de LVM para discos de datos.
* Habilitación del cifrado en discos de SO y datos de VM Linux.

   > [!NOTE]
   > No se admite el cifrado de unidades del sistema operativo para algunas distribuciones de Linux. Para más información, consulte [Sistemas operativos compatibles con Azure Disk Encryption: Linux](azure-security-disk-encryption-prerequisites.md#linux).
   
* Habilitación del cifrado en VM configuradas con Espacios de almacenamiento de Windows a partir de Windows Server 2016. Los Espacios de almacenamiento directo (S2D) aún no son compatibles.
* Copia de seguridad y restauración de VM cifradas, para escenarios de clave de cifrado con clave (KEK) y otros.

Azure Disk Encryption no funciona para los siguientes escenarios, características ni tecnologías:

* VM de cifrado de nivel básico o VM creadas a través del método de creación de VM clásico.
* Deshabilitación del cifrado en una unidad de SO o datos de una VM Linux cuando se cifra la unidad de SO.
* Cifrado de la unidad de SO para el conjunto de escalado de máquinas virtuales.
* VM Windows que configuradas con sistemas RAID basados en software.
* Cifrado de imágenes personalizadas en VM Linux.
* Integración con un sistema de administración de claves local.
* Azure Files (sistema de archivos compartido).
* Network File System (NFS).
* Volúmenes dinámicos.
* Discos de sistema operativo efímero.

## <a name="encryption-features"></a>Características de cifrado

Al habilitar e implementar Azure Disk Encryption para VM de Azure, puede configurar las siguientes funcionalidades para que se habiliten:

* Cifrado del volumen del SO para proteger el volumen de arranque en reposo en el almacenamiento.
* Cifrado de volúmenes de datos para proteger los volúmenes de datos en reposo en el almacenamiento.
* Deshabilitación del cifrado en las unidades del SO y datos para VM Windows.
* Deshabilitación del cifrado en las unidades de datos para VM Linux (solo cuando la unidad del SO no está cifrada).
* Protección de las claves y secretos de cifrado en la suscripción de Azure Key Vault.
* Notificación del estado de cifrado de la VM cifrada.
* Eliminación de las opciones de configuración de cifrado de disco de la VM.
* Copia de seguridad y restauración de VM cifradas mediante el servicio Azure Backup.

Azure Disk Encryption para VM Windows y Linux incluye lo siguiente:

* [La extensión de cifrado de disco para Windows](../virtual-machines/extensions/azure-disk-enc-windows.md).
* [La extensión de cifrado de disco para Linux](../virtual-machines/extensions/azure-disk-enc-linux.md).
* [Los cmdlets de cifrado de disco de PowerShell](/powershell/module/az.compute/set-azvmdiskencryptionextension?view=azps-2.2.0).
* [Los cmdlets de cifrado de disco de la CLI de Azure](/cli/azure/vm/encryption?view=azure-cli-latest).
* [Las plantillas de cifrado de disco de Azure Resource Manager](azure-security-disk-encryption-appendix.md#resource-manager-templates).

> [!NOTE]
> No hay cargo adicional por el cifrado de discos de máquinas virtuales con Azure Disk Encryption. Los [precios de Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) en versión estándar se aplican al almacén de claves usado para almacenar las claves de cifrado. 

## <a name="encryption-workflow"></a>Flujo de trabajo de cifrado

Para habilitar el cifrado de disco para máquinas virtuales Windows y Linux, siga estos pasos:

1. Opte por habilitar el cifrado de disco mediante la plantilla de Resource Manager de Azure Disk Encryption, los cmdlets de PowerShell o la CLI de Azure, y especifique la configuración del cifrado.

   * Para el escenario de VHD cifrado por el cliente, cargue el VHD cifrado a su cuenta de almacenamiento y el material de la clave de cifrado en su almacén de claves. Luego, proporcione la configuración de cifrado para habilitar el cifrado en una nueva VM.
   * Para las nuevas VM creadas a partir de imágenes admitidas de la galería y las VM existentes que ya se ejecutan en Azure, proporcione la configuración de cifrado para habilitar el cifrado en la VM.

1. Conceda acceso a la plataforma de Azure para leer el material de la clave de cifrado (sistemas de claves de cifrado de BitLocker para Windows y frase de contraseña para Linux) de su almacén de claves para habilitar el cifrado en la VM.

1. Azure actualiza el modelo de servicio de la máquina virtual con la configuración del cifrado y del almacén de claves y configura la máquina virtual cifrada.

   ![Microsoft Antimalware en Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Flujo de trabajo de descifrado
Para deshabilitar el cifrado de disco para las VM, complete los siguientes pasos de alto nivel:

1. Elija deshabilitar el cifrado (descifrado) en una VM en ejecución en Azure y especifique la configuración de descifrado. Puede deshabilitarlo mediante la plantilla de Resource Manager de Azure Disk Encryption, los cmdlets de PowerShell o la CLI de Azure.

   Este paso deshabilita el cifrado del volumen del SO o datos (o ambos) en la VM Windows en ejecución. Como se mencionó en la sección anterior, no se permite deshabilitar el cifrado de disco del sistema operativo para Linux. El paso de descifrado se permite solo para unidades de datos en máquinas virtuales Linux siempre que el disco del sistema operativo no esté cifrado.

1. Azure actualiza el modelo de servicio de la VM y la VM se marca como descifrada. El contenido de la máquina virtual ya no se cifra en reposo.

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

   * Para el escenario de VHD cifrado por el cliente, cargue el VHD cifrado a su cuenta de almacenamiento y el material de la clave de cifrado en su almacén de claves. Luego, proporcione la configuración de cifrado para habilitar el cifrado en una nueva VM.
   * Para las nuevas VM creadas desde Marketplace y las VM existentes que ya se ejecutan en Azure, proporcione la configuración para habilitar el cifrado en la VM.

1. Conceda acceso a la plataforma de Azure para leer el material de la clave de cifrado (sistemas de claves de cifrado de BitLocker para Windows y frase de contraseña para Linux) de su almacén de claves para habilitar el cifrado en la VM.

1. Proporcione la identidad de aplicación de Azure AD para escribir el material de clave de cifrado en su almacén de claves. Este paso habilita el cifrado en la VM para los escenarios mencionados en el paso 2.

1. Azure actualiza el modelo de servicio de la máquina virtual con la configuración del cifrado y del almacén de claves y configura la máquina virtual cifrada.


## <a name="terminology"></a>Terminología
En la siguiente tabla se definen algunos de los términos comunes que se usan en la documentación de cifrado de disco de Azure:

| Terminología | Definición |
| --- | --- |
| Azure AD | Una cuenta de [Azure AD](https://azure.microsoft.com/documentation/services/active-directory/) se usa para autenticar, almacenar y recuperar secretos de un almacén de claves. |
| Azure Key Vault | Key Vault es un servicio de administración de claves criptográficas basado en módulos de seguridad de hardware validados por el Estándar federal de procesamiento de información (FIPS). Estos estándares ayudan a proteger las claves criptográficas y los secretos confidenciales. Para más información, consulte la documentación de [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) es una tecnología de cifrado de volúmenes de Windows reconocida por el sector que se usa para habilitar el cifrado de disco en VM Windows. |
| BEK | Las claves de cifrado de BitLocker (BEK) se usan para cifrar el volumen de arranque del sistema operativo y los volúmenes de datos. Las claves de BitLocker están protegidas en un almacén de claves como secretos. |
| CLI de Azure | [La CLI de Azure](/cli/azure/install-azure-cli) está optimizada para administrar recursos de Azure desde la línea de comandos.|
| DM-Crypt |[DM-Crypt](https://gitlab.com/cryptsetup/cryptsetup/wikis/DMCrypt) es el subsistema de cifrado transparente de disco basado en Linux que se usa para habilitar el cifrado de disco en las VM Linux. |
| Clave de cifrado de claves (KEK) | La clave asimétrica (RSA 2048) que puede usar para proteger o encapsular el secreto. Puede proporcionar una clave protegida mediante módulos de seguridad de hardware (HSM) o una clave protegida mediante software. Para más información, consulte la documentación de [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). |
| Cmdlets de PowerShell | Para más información, consulte [Cmdlets de Azure PowerShell](/powershell/azure/overview). |

## <a name="next-steps"></a>Pasos siguientes

Para empezar, consulte los [requisitos previos de Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md).

