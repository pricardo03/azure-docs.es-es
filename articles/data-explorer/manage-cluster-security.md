---
title: Protección de clústeres en Azure Data Explorer
description: En este artículo se describe cómo proteger un clúster en Azure Data Explorer dentro de Azure Portal.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: c6f7e921886a6acdaa31d0f69f57119c339c0b8b
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70172591"
---
# <a name="secure-your-cluster-in-azure-data-explorer"></a>Protección de clústeres en Azure Data Explorer

[Azure Disk Encryption](/azure/security/azure-security-disk-encryption-overview) ayuda a custodiar y proteger sus datos con el fin de satisfacer los compromisos de cumplimiento y seguridad de su organización. Proporciona cifrado de volúmenes tanto para los discos de datos como para los del sistema operativo de las máquinas virtuales del clúster. También se integra con [Azure Key Vault](/azure/key-vault/), lo que permite controlar y administrar los secretos y las claves de cifrado de discos, y garantiza que todos los datos de los discos de la máquina virtual se cifran en reposo mientras se encuentran en Azure Storage. 

La configuración de seguridad del clúster le permite habilitar el cifrado de disco en el clúster.
  
## <a name="enable-encryption-at-rest"></a>Habilitación del cifrado de datos en reposo
  
La habilitación del [cifrado en reposo](/azure/security/fundamentals/encryption-atrest) en su clúster proporciona protección de datos para los datos almacenados (en reposo). 

1. En Azure Portal, vaya al recurso de clúster de Azure Data Explorer. En el encabezado **Configuración**, seleccione **Seguridad**. 

    ![Activación del cifrado en reposo](media/manage-cluster-security/security-encryption-at-rest.png)

1. En la ventana **Seguridad**, seleccione **Activado** en el valor de seguridad de **Cifrado de disco**. 

1. Seleccione **Guardar**.
 
> [!NOTE]
> Seleccione **Desactivado** para deshabilitar el cifrado una vez que se ha habilitado.

## <a name="next-steps"></a>Pasos siguientes

[Comprobación del estado del clúster](/azure/data-explorer/check-cluster-health)
