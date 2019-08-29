---
title: Protección de clústeres en Azure Data Explorer
description: En este artículo se describe cómo proteger un clúster en Azure Data Explorer dentro de Azure Portal.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: ad08bb19f96aadad42e973eebb8adce6875e07b1
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876590"
---
# <a name="secure-your-cluster-in-azure-data-explorer"></a>Protección de clústeres en Azure Data Explorer

[Azure Disk Encryption](/azure/security/azure-security-disk-encryption-overview) ayuda a custodiar y proteger sus datos con el fin de satisfacer los compromisos de cumplimiento y seguridad de su organización. Proporciona cifrado de volúmenes tanto para los discos de datos como para los del sistema operativo de las máquinas virtuales del clúster. También se integra con [Azure Key Vault](/azure/key-vault/) para ayudarle a controlar y administrar los secretos y las claves de cifrado de los discos, y garantiza que todos los datos de los discos de la máquina virtual se cifran en reposo mientras se encuentren en Azure Storage. La configuración de seguridad del clúster le permite habilitar el cifrado de disco en el clúster.
  
## <a name="enable-encryption-at-rest"></a>Habilitación del cifrado de datos en reposo
  
La habilitación del [cifrado en reposo](/azure/security/fundamentals/encryption-atrest) en su clúster proporciona protección de datos para los datos almacenados (en reposo). 

1. En Azure Portal, vaya al recurso de clúster de Azure Data Explorer. En el encabezado **Configuración**, seleccione **Seguridad**. 

    ![Activación del cifrado en reposo](media/manage-cluster-security/security-encryption-at-rest.png)

1. En la ventana **Seguridad**, seleccione **Activado** en el valor de seguridad de **Cifrado de disco**. 

1. Seleccione **Guardar**.
 
> [!NOTE]
> El cifrado se puede deshabilitar una vez activado.

## <a name="next-steps"></a>Pasos siguientes

[Comprobación del estado del clúster](/azure/data-explorer/check-cluster-health)
