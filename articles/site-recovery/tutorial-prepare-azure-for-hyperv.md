---
title: Preparación de Azure para la recuperación ante desastres de máquinas virtuales locales de Hyper-V con Azure Site Recovery | Microsoft Docs
description: Aprenda a preparar Azure para la recuperación ante desastres de máquinas virtuales locales de Hyper-V mediante Azure Site Recovery.
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 48101e49429225018381ed2a3b1e8e4e351c15a6
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59362853"
---
# <a name="prepare-azure-resources-for-disaster-recovery-of-on-premises-machines"></a>Preparación de los recursos de Azure para la recuperación ante desastres de máquinas locales

 [Azure Site Recovery](site-recovery-overview.md) contribuye a la estrategia de recuperación ante desastres y continuidad empresarial (BCDR) al mantener sus aplicaciones empresariales al día y disponibles durante interrupciones planeadas y no planeadas. Azure Site Recovery administra y coordina la recuperación ante desastres de máquinas locales y máquinas virtuales de Azure, lo que incluye la replicación, la conmutación por error y la recuperación.

Este artículo es el primer tutorial de una serie que muestra cómo configurar la recuperación ante desastres para máquinas virtuales locales. Es relevante cuando va a proteger las máquinas virtuales de Hyper-V.

> [!NOTE]
> Los tutoriales están diseñados para mostrarle la ruta de implementación más sencilla para un escenario. Usan opciones predeterminadas siempre que es posible y no muestran todos los valores y las rutas de acceso posibles. Para instrucciones detalladas, consulte la sección del **procedimiento** para el escenario correspondiente.

En este artículo se muestra cómo preparar los componentes de Azure cuando se desean replicar máquinas virtuales locales (Hyper-V) en Azure. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Compruebe que la cuenta de Azure tiene permisos de replicación.
> * Cree una cuenta de Azure Storage. Las imágenes de máquinas replicadas se almacenarán en este.
> * Cree un almacén de Recovery Services. Un almacén contiene metadatos e información de configuración de las máquinas virtuales y otros componentes de replicación.
> * Configure una red de Azure. Cuando se crean máquinas virtuales de Azure después de la conmutación por error, se unen a esta red de Azure.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en el [Azure Portal](http://portal.azure.com).

## <a name="verify-account-permissions"></a>Comprobar los permisos de la cuenta

Si acaba de crear su cuenta de Azure gratis, ya es el administrador de la suscripción. Si no es el administrador de la suscripción, solicite al administrador que le asigne los permisos que necesita. Para habilitar la replicación de una nueva máquina virtual, debe tener permiso para:

- Crear una máquina virtual en el grupo de recursos seleccionado.
- Crear una máquina virtual en la red virtual seleccionada.
- Escribir en la cuenta de almacenamiento seleccionada.

Para completar estas tareas su cuenta debe tener asignado el rol integrado de colaborador de la máquina virtual. Además, para administrar las operaciones de Site Recovery en un almacén, su cuenta debe tener asignado el rol integrado de colaborador de Site Recovery.

## <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Las imágenes de máquinas replicadas se conservan en Azure Storage. Las máquinas virtuales de Azure se crean desde el almacenamiento cuando se realiza la conmutación por error desde el entorno local en Azure. La cuenta de almacenamiento debe estar en la misma región que el almacén de Recovery Services. En este tutorial vamos a usar Europa Occidental.

1. En el menú [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso** > **Almacenamiento** > **Cuenta de almacenamiento: blob, archivo, tabla, cola**.
2. En **Crear cuenta de almacenamiento**, escriba un nombre para la cuenta. En estos tutoriales se usa **contosovmsacct1910171607**. El nombre que elija debe ser único en Azure, tener entre 3 y 24 caracteres, y contener solo números y letras minúsculas.
3. En **Modelo de implementación**, seleccione **Resource Manager**.
4. En **Tipo de cuenta** seleccione **Storage (uso general v1)**. No seleccione Blob Storage.
5. En **Replicación**, seleccione el valor predeterminado **Almacenamiento con redundancia geográfica con acceso de lectura** como redundancia de almacenamiento. Se ha dejado la opción **Se requiere transferencia segura** en **Deshabilitado**.
6. En **Rendimiento**, seleccione **Estándar** y en **Nivel de acceso** elija la opción predeterminada de **Frecuente**.
7. En **Suscripción**, seleccione la suscripción en la que desea crear la nueva cuenta de almacenamiento.
8. En **Grupo de recursos**, especifique un nuevo grupo de recursos. Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. En estos tutoriales se usa **ContosoRG**.
9. En **Ubicación**, seleccione la ubicación geográfica de la cuenta de almacenamiento. 

   ![Crear una cuenta de almacenamiento](media/tutorial-prepare-azure/create-storageacct.png)

9. Seleccione **Crear** para crear la cuenta de almacenamiento.

## <a name="create-a-recovery-services-vault"></a>Creación de un almacén de Recovery Services

1. En Azure Portal, haga clic en **+Crear un recurso** y, en Marketplace, busque **Recovery Services**.
2. Haga clic en **Backup and Site Recovery (OMS)** y, una vez en esta página, haga clic en **Crear**. 
1. En **Almacén de Recovery Services** > **Nombre**, escriba un nombre descriptivo para identificar el almacén. Para este conjunto de tutoriales se usa **ContosoVMVault**.
2. En **Grupo de recursos**, seleccione un grupo de recursos existente o cree uno nuevo. En este tutorial se va a usar **contosoRG**.
3. En **Ubicación**, seleccione la región en la que debería estar el almacén. se va a usar **Europa Occidental**.
4. Para acceder rápidamente al almacén desde el panel, seleccione **Anclar al panel** > **Crear**.

   ![Crear un nuevo almacén](./media/tutorial-prepare-azure/new-vault-settings.png)

   El nuevo almacén aparecerá en **Panel** > **Todos los recursos** y en la página principal de **Almacenes de Recovery Services**.

## <a name="set-up-an-azure-network"></a>Configurar una red de Azure

Cuando se crean máquinas virtuales de Azure desde el almacenamiento después de la conmutación por error, se unen a esta red.

1. En [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso** > **Redes** > **Red virtual**.
2. Vamos a dejar **Resource Manager** seleccionado como modelo de implementación.
3. En **Nombre**, escriba un nombre de red. El nombre debe ser único dentro del grupo de recursos de Azure. Se va a usar **ContosoASRnet** en este tutorial.
4. Especifique el grupo de recursos en el que se creará la red. Se va a usar el grupo de recursos existente **contosoRG**.
5. En **Intervalo de direcciones**, escriba el intervalo de direcciones de red **10.0.0.0/24**. En esta red no se va a usar una subred.
6. En **Suscripción**, seleccione la suscripción en la que se creará la red.
7. En **Ubicación**, seleccione **Europa Occidental**. La red virtual de Azure debe estar en la misma región que el almacén de Recovery Services.
8. Se van a dejar las opciones predeterminadas de protección básica contra DDoS, con ningún punto de conexión de servicio en la red.
9. Haga clic en **Create**(Crear).

   ![Creación de una red virtual](media/tutorial-prepare-azure/create-network.png)

   La red virtual tarda unos segundos en crearse. Una vez creada, se ve en el panel de Azure Portal.

## <a name="useful-links"></a>Vínculos útiles

- [Más información](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) sobre las redes de Azure.
- [Más información](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) sobre discos administrados.



## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Preparación de la infraestructura de Hyper-V local para la recuperación ante desastres en Azure](hyper-v-prepare-on-premises-tutorial.md)
