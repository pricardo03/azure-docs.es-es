---
title: Guía de aprovisionamiento de máquinas virtuales Windows con SQL Server en Azure Portal | Microsoft Docs
description: En esta guía de procedimientos se describen las opciones para crear máquinas virtuales de Windows SQL Server 2017 en Azure Portal.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: 1aff691f-a40a-4de2-b6a0-def1384e086e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 05/04/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: bb051d37f3a1dd82d7d46bfe8b22c2ba1251be85
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129890"
---
# <a name="how-to-provision-a-windows-sql-server-virtual-machine-in-the-azure-portal"></a>Aprovisionamiento de una máquina virtual Windows con SQL Server en Azure Portal

En esta guía se proporcionan detalles sobre las distintas opciones disponibles cuando se crea una máquina virtual Windows con SQL Server en Azure Portal. En este artículo se describen más opciones de configuración que en la [guía de inicio rápido de VM con SQL Server](quickstart-sql-vm-create-portal.md), que trata más bien de una tarea de aprovisionamiento posible. 

Use esta guía para crear su propia VM con SQL Server. O bien úsela como referencia para las opciones disponibles en Azure Portal.

> [!TIP]
> Si tiene alguna pregunta sobre las máquinas virtuales de SQL Server, consulte las [Preguntas más frecuentes](virtual-machines-windows-sql-server-iaas-faq.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a id="select"></a> Imágenes de la galería de máquinas virtuales de SQL Server

Cuando crea una máquina virtual de SQL Server, puede seleccionar una de varias imágenes configuradas previamente en la galería de máquinas virtuales. Los pasos siguientes muestran cómo seleccionar una de las imágenes de SQL Server 2017.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con su cuenta.

1. En Azure Portal, haga clic en **Crear un recurso**. El portal abre la ventana **Nuevo**.

1. En la ventana **Nuevo**, haga clic en **Proceso** y después haga clic en **Ver todo**.

1. En el campo de búsqueda, escriba **SQL Server 2017** y presione ENTRAR.

1. En el filtro listas desplegables, seleccione _Windows Server 2016_ para el **del sistema operativo** y seleccione _Microsoft_ como el **Publisher**. 

     ![Menú Nuevo proceso](./media/virtual-machines-windows-portal-sql-server-provision/azure-new-compute-blade.png)

1. Consulte las imágenes de SQL Server disponibles. Cada imagen identifica una versión de SQL Server y un sistema operativo.

1. Seleccione la imagen denominada **Free SQL Server License: SQL Server 2017 Developer on Windows Server 2016** (Licencia gratuita de SQL Server: SQL Server 2017 Developer en Windows Server 2016).

   > [!TIP]
   > La edición Developer se usa en este tutorial porque se trata de una edición gratuita completa de SQL Server para pruebas de desarrollo. Solo paga por el costo de ejecutar la máquina virtual. Sin embargo, es libre de elegir cualquiera de las imágenes para usarla en este tutorial. Para una descripción de las imágenes disponibles, consulte la [introducción a SQL Server en máquinas virtuales Windows de Azure](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo).

   > [!TIP]
   > Los costos de licencia de SQL Server se incorporan en el precio por segundo de la VM que se crea y varía según edición y núcleos. Sin embargo, la edición SQL Server Developer es gratis para desarrollo/pruebas (no producción) y SQL Express es gratis para cargas de trabajo ligeras (menos de 1 GB de memoria, menos de 10 GB de almacenamiento). También existe la opción de traiga su propia licencia (BYOL) y que solo pague por la máquina virtual. Esos nombres de imagen tienen el prefijo {BYOL}. 
   >
   > Para más información sobre estas opciones, consulte [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Orientación de precios de máquinas virtuales de SQL Server en Azure).

1. En **Seleccionar un modelo de implementación**, compruebe que la opción **Resource Manager** está seleccionada. Resource Manager es el modelo de implementación recomendado para las máquinas virtuales nuevas. 

1. Seleccione **Crear**.


## <a id="configure"></a> Opciones de configuración

Hay varias pestañas para configurar una máquina virtual de SQL Server. Con el propósito de esta guía, nos centraremos en la siguiente: 

| Paso | DESCRIPCIÓN |
| --- | --- |
| **Aspectos básicos** |[Configuración básica](#1-configure-basic-settings) |
| **Características opcionales** |[Configuración de características opcionales](#2-configure-optional-features) |
| **Configuración de SQL Server** |[Configuración de SQL Server](#3-configure-sql-server-settings) |
| **Revisar y crear** | [Revisión del resumen](#4-review--create) |

## <a name="1-configure-basic-settings"></a>1. Configuración básica


En la pestaña **Básico**, especifique la siguiente información:

* En **Project Details**, asegúrese de que está seleccionada la suscripción correcta. 
*  En el **grupo de recursos** sección, seleccione un recurso existente en la lista de grupo o elija **crear nuevo** para crear un nuevo grupo de recursos. Un grupo de recursos es una colección de recursos relacionados de Azure (máquinas virtuales, cuentas de almacenamiento, redes virtuales, etc.). 

    ![Subscription](media/quickstart-sql-vm-create-portal/basics-project-details.png)

  > [!NOTE]
  > El uso de un grupo de recursos resulta útil si solo está probando o aprendiendo sobre las implementaciones de SQL Server en Azure. Cuando haya terminado la prueba, elimine el grupo de recursos. De ese modo, se eliminará automáticamente la máquina virtual y los recursos asociados a ese grupo. Para más información sobre los grupos de recursos, consulte [Información general de Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md).


* En **Detalles de instancia**:
    1. Escriba un único **nombre de máquina Virtual**.  
    1. Elija una ubicación para su **Región**. 
    1. Con el propósito de esta guía, deje **las opciones de disponibilidad** establecido en _ninguna redundancia de infraestructura necesaria_. Para más información acerca de las opciones de disponibilidad, consulte [Regiones de Azure y disponibilidad](../../windows/regions-and-availability.md). 
    1. En la lista **Imagen** seleccione la denominada _Free SQL Server License: SQL Server 2017 Developer on Windows Server 2016_ (Licencia gratuita de SQL Server: SQL Server 2017 Developer en Windows Server 2016).  
    1. Elija **Cambiar el tamaño** para el **Tamaño** de la máquina virtual y seleccione la oferta **Básico A2**. Asegúrese de limpiar los recursos cuando haya terminado con ellos para evitar cargos inesperados. Para cargas de trabajo de producción, consulte las recomendaciones de tamaños de máquina y la configuración en [Procedimientos recomendados de SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

    ![Detalles de instancia](media/quickstart-sql-vm-create-portal/basics-instance-details.png)

> [!IMPORTANT]
> El costo mensual estimado que se muestra en la ventana **Elegir un tamaño** no incluye los costos de licencias de SQL Server. Esta estimación es el costo solo de la máquina virtual. Para las ediciones Express y Developer de SQL Server, esta estimación es el costo total estimado. Para conocer el costo de otras ediciones, consulte la [página de precios de máquinas virtuales Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) y seleccione la edición de destino de SQL Server. Consulte también el [orientación para máquinas virtuales de Azure de SQL Server de precios](virtual-machines-windows-sql-server-pricing-guidance.md) y [tamaños de máquinas virtuales](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* En **cuenta de administrador**, proporcione un nombre de usuario y una contraseña. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../../windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

   ![Cuenta de administrador](media/quickstart-sql-vm-create-portal/basics-administrator-account.png)

* En **Reglas de puerto de entrada**, elija **Permitir los puertos seleccionados** y luego seleccione **RDP (3389)** en la lista desplegable. 

   ![Reglas de puerto de entrada](media/quickstart-sql-vm-create-portal/basics-inbound-port-rules.png)


## <a name="2-configure-optional-features"></a>2. Configuración de características opcionales

### <a name="disks"></a>Discos

En el **discos** pestaña, configure las opciones de disco. 

* En **tipo de disco del sistema operativo**, seleccione el tipo de disco que desee para su sistema operativo en la lista desplegable. Premium se recomienda para los sistemas de producción, pero no está disponible para una máquina virtual básica. Para usar SSD Premium, cambiar el tamaño de máquina virtual. 
* En **avanzadas**, seleccione **Sí** bajo uso **Managed Disks**.

   > [!NOTE]
   > Microsoft recomienda el uso de Managed Disks para SQL Server. Managed Disks controla el almacenamiento en segundo plano. Además, cuando las máquinas virtuales con Managed Disks están en el mismo conjunto de disponibilidad, Azure distribuye los recursos de almacenamiento para proporcionar la redundancia adecuada. Para más información, consulte [Introducción a Azure Managed Disks][../managed-disks-overview.md). Para obtener información específica acerca de los discos administrados en un conjunto de disponibilidad, consulte [uso de managed disks para máquinas virtuales en el conjunto de disponibilidad] (.. /Manage-Availability.MD.

![Configuración de disco de máquina virtual de SQL](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-disks.png)
  
  
### <a name="networking"></a>Redes

En el **redes** pestaña, configure las opciones de las red. 

* Cree un nuevo **red virtual**, o usar una red virtual existente para la máquina virtual de SQL Server. Designar un **subred** también. 

* En **grupo de seguridad de la NIC**, seleccione un grupo de seguridad básica o el grupo de seguridad avanzada. Si elige la opción básica le permite seleccionar los puertos de entrada para la máquina virtual de SQL Server (los mismos valores que se configuraron en el **básica** pestaña). Seleccionar la opción avanzada le permite elegir un grupo de seguridad de red existente o crear uno nuevo. 

* Puede realizar otros cambios en valores de configuración de la red o mantener los valores predeterminados.

![Configuración de red de máquina virtual de SQL](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-networking.png)

#### <a name="monitoring"></a>Supervisión

En el **supervisión** pestaña, configurar la supervisión y el apagado automático. 

* Azure permite **arranque supervisión** de forma predeterminada con la misma cuenta de almacenamiento designada para la máquina virtual. Puede cambiar estas opciones aquí, así como habilitar **del sistema operativo invitado diagnósticos**. 
* Puede habilitar **asignado por sistema identidad administrada** y **apagado automático** en esta pestaña también. 

![Configuración de administración de VM de SQL](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-management.png)


## <a name="3-configure-sql-server-settings"></a>3. Configuración de SQL Server

En el **configuración de SQL Server** pestaña, configure las optimizaciones de SQL Server y los valores específicos. La configuración que se puede configurar para SQL Server incluye lo siguiente:



| Configuración |
| --- |
| [Conectividad](#connectivity) |
| [Autenticación](#authentication) |
| [Integración de Azure Key Vault](#azure-key-vault-integration) |
| [Configuración de almacenamiento](#storage-configuration) |
| [Aplicación de revisiones automatizada](#automated-patching) |
| [Automated Backup](#automated-backup) |
| [Servicios de R (análisis avanzado)](#r-services-advanced-analytics) |


### <a name="connectivity"></a>Conectividad

En **Conectividad SQL**, especifique el tipo de acceso que desea para la instancia de SQL Server de esta máquina virtual. En este tutorial, seleccione **Público (Internet)** para permitir que se establezcan conexiones a SQL Server desde equipos o servicios de Internet. Con esta opción seleccionada, Azure configura automáticamente el firewall y el grupo de seguridad de red para permitir el tráfico en el puerto seleccionado.

> [!TIP]
> De forma predeterminada, SQL Server escucha en un puerto conocido, **1433**. Para mayor seguridad, cambie el puerto en el cuadro de diálogo anterior para que escuche en un puerto no predeterminado, como 1401. Si cambia el puerto, debe conectarse con ese puerto desde cualquiera de las herramientas de cliente, como SSMS.

![Seguridad de máquina virtual de SQL](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-security.png)

Para conectarse a SQL Server a través de Internet, también debe habilitar la autenticación de SQL Server, que se describe en la sección siguiente.

Si prefiere no permitir las conexiones al motor de base de datos a través de Internet, elija una de las siguientes opciones:

* **Local (solo dentro de la máquina virtual)** : elija esta opción para permitir conexiones a SQL Server únicamente desde dentro de la máquina virtual.
* **Privado (dentro de la red virtual)** : elija esta opción para permitir conexiones a SQL Server desde máquinas o servicios que se encuentren en la misma red virtual.

En general, mejore la seguridad al elegir la conectividad más restrictiva que permita su escenario. No obstante, todas las opciones se pueden proteger mediante reglas del grupo de seguridad de red y la autenticación de SQL o Windows. Puede editar el grupo de seguridad de red después de crear la máquina virtual. Para obtener más información, consulte [Consideraciones de seguridad para SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-security.md).



### <a name="authentication"></a>Authentication

Si la autenticación de SQL Server es necesaria, en **Habilitar** under **Habilitar**.

![Autenticación de SQL Server](./media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-authentication.png)

> [!NOTE]
> Si piensa obtener acceso a SQL Server a través de Internet (la opción de conectividad pública), debe habilitar aquí la autenticación de SQL. El acceso público a SQL Server requiere la utilización de autenticación de SQL.

Si habilita la autenticación de SQL Server, especifique los valores de **Nombre de inicio de sesión** y **Contraseña**. Este nombre de inicio de sesión se configura como un inicio de sesión de autenticación de SQL Server y un miembro de la **sysadmin** rol fijo de servidor. Para más información sobre los modos de autenticación, consulte [Elegir un modo de autenticación](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode).

Si no habilita la autenticación de SQL Server, puede utilizar la cuenta de administrador local en la máquina virtual para conectarse a la instancia de SQL Server.

![Autenticación de SQL Server](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-authentication.png)

### <a name="azure-key-vault-integration"></a>Integración de Azure Key Vault

Para almacenar secretos de seguridad en Azure para el cifrado, haga clic en **Integración de Azure Key Vault** y en **Habilitar**.

![Integración de Azure Key Vault](media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

En la tabla siguiente se enumeran los parámetros necesarios para configurar la integración de Azure Key Vault.

| PARÁMETRO | Description | EJEMPLO: |
| --- | --- | --- |
| **Dirección URL de Key Vault** |La ubicación del almacén de claves. |https:\//contosokeyvault.vault.azure.net/ |
| **Nombre de entidad de seguridad** |Nombre de la entidad de servicio de Azure Active Directory Esto se conoce también como Id. Este nombre también se conoce como «identificador de cliente». |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Secreto de entidad de seguridad** |Secreto de la entidad de seguridad de servicio de Azure Active Directory Este secreto también se conoce como «secreto de cliente». |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM= |
| **Nombre de credencial** |**Nombre de credencial**: La integración de AKV crea una credencial en SQL Server, permitiendo el acceso de la máquina virtual al Almacén de claves. Elija un nombre para esta credencial. |mycred1 |

Para más información, consulte [Configuración de la integración de Azure Key Vault para SQL Server en máquinas virtuales de Azure](virtual-machines-windows-ps-sql-keyvault.md).

### <a name="storage-configuration"></a>Configuración de almacenamiento

En **configuración de almacenamiento**, seleccione **cambiar la configuración de** para especificar los requisitos de almacenamiento.


> [!NOTE]
> Si ha configurado manualmente la máquina virtual para usar almacenamiento estándar, esta opción no está disponible. La optimización del almacenamiento automático está disponible solo para Premium Storage.

> [!TIP]
> El número de pasos y los límites superiores de cada control deslizante dependen del tamaño de la máquina virtual que seleccionó. Una máquina virtual más grande y más eficaz es capaz de escalar verticalmente más.

Puede especificar requisitos como operaciones de entrada/salida por segundo (IOPS), el rendimiento en MB/s y el tamaño de almacenamiento total. Configure estas opciones mediante las escalas deslizantes. Puede cambiar estos valores de almacenamiento en función de la carga de trabajo. El portal calcula automáticamente el número de discos a conectar y los configura según estos requisitos.

En **Storage optimized for**(Optimización de almacenamiento para), seleccione una de las siguientes opciones:

* **General** es la configuración predeterminada y admite la mayoría de las cargas de trabajo.
* **Transaccional** optimiza el almacenamiento en las cargas de trabajo OLTP de las bases de datos tradicionales.
* **Almacenamiento de datos** optimiza el almacenamiento para cargas de trabajo de informes y análisis.

![Configuración de almacenamiento de máquina virtual de SQL](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-storage-configuration.png)

### <a name="sql-server-license"></a>Licencia de SQL Server
Si es un cliente de Software Assurance, puede usar el [ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/) traiga su propia licencia de SQL Server y ahorrar en los recursos. 

![Licencia de la máquina virtual de SQL](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-license.png)

### <a name="automated-patching"></a>Aplicación de revisiones automatizada

**Automated patching** está habilitada de forma predeterminada. La aplicación de revisiones automatizada permite a Azure aplicar automáticamente las revisiones de SQL Server y del sistema operativo. Especifique un día de la semana, la hora y la duración de una ventana de mantenimiento. Azure realiza la aplicación de revisión en esta ventana de mantenimiento. La programación de la ventana de mantenimiento utiliza la configuración regional de la máquina virtual para la hora. Si no desea que Azure efectúe automáticamente la aplicación de revisiones de SQL Server y del sistema operativo, haga clic en **Deshabilitar**.  

![Aplicación de revisiones automatizada de VM de SQL](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-automated-patching.png)

Para más información, consulte [Automated Patching para SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Copia de seguridad automatizada

Habilite las copias de seguridad automáticas en todas las bases de datos en **Copia de seguridad automatizada**. La copia de seguridad automatizada está deshabilitada de forma predeterminada.

Al habilitar la copia de seguridad automatizada de SQL, puede configurar las siguientes opciones:

* El período de retención (días) de las copias de seguridad
* La cuenta de almacenamiento que se utilizará para las copias de seguridad
* La opción de cifrado y la contraseña de las copias de seguridad
* La copia de seguridad de bases de datos del sistema
* La configuración de la programación de copia de seguridad

Para cifrar la copia de seguridad, haga clic en **Habilitar**. Después, especifique un valor en **Contraseña**. Azure crea un certificado para cifrar las copias de seguridad y utiliza la contraseña especificada para proteger ese certificado.

Para obtener más información, vea [Automated Backup para SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).


### <a name="r-services-advanced-analytics"></a>Servicios de R (análisis avanzado)

Tiene la opción para habilitar [SQL Server R Services (análisis avanzado)](/sql/advanced-analytics/r/sql-server-r-services/). Esta opción le permite usar el análisis avanzado con SQL Server 2017. Haga clic en **Habilitar** en la ventana **Configuración de SQL Server**.


## <a name="4-review--create"></a>4. Revisar y crear

En la pestaña **Revisar y crear**, revise el resumen y seleccione **Crear** para crear la instancia de SQL Server, el grupo de recursos y los recursos especificados para esta máquina virtual.

Puede supervisar la implementación desde Azure Portal. En el botón **Notificaciones** de la parte superior de la pantalla, se muestra el estado básico de la implementación.

> [!NOTE]
> Para darle una idea de los tiempos de implementación, se implementó una máquina virtual de SQL en la región este de Estados Unidos con la configuración predeterminada. Esta implementación de prueba tardó aproximadamente 12 minutos en completarse. Pero podría experimentar un tiempo de implementación más rápido o más lento en función de su región y de la configuración seleccionada.

## <a id="remotedesktop"></a>Apertura de la máquina virtual con Escritorio remoto

Use los pasos siguientes para conectarse a la máquina virtual de SQL Server con Escritorio remoto:

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Después de conectarse a la máquina virtual de SQL Server, puede iniciar SQL Server Management Studio y conectarse con la autenticación de Windows mediante sus credenciales de administrador local. Si ha habilitado la autenticación de SQL Server, también puede conectarse con la autenticación de SQL mediante el inicio de sesión de SQL y la contraseña configurada durante el aprovisionamiento.

El acceso a la máquina le permite cambiar directamente la máquina y la configuración de SQL Server según sus necesidades. Por ejemplo, podría configurar el firewall o cambiar la configuración de SQL Server.

## <a id="connect"></a> Conexión remota a SQL Server

En este tutorial, seleccionó el acceso **Público** para la máquina virtual y **Autenticación de SQL Server**. Estos valores configuran automáticamente la máquina virtual para permitir conexiones de SQL Server desde cualquier cliente a través de Internet (suponiendo que tengan el inicio de sesión SQL correcto).

> [!NOTE]
> Si no ha seleccionado la opción Pública durante el aprovisionamiento, puede cambiar la configuración de la conectividad SQL a través del portal después de aprovisionar. Para más información, consulte [Cambio de la configuración de conectividad SQL](virtual-machines-windows-sql-connect.md#change).

En las secciones siguientes se muestra cómo conectarse a la instancia de VM con SQL Server a través de Internet.

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

  > [!NOTE]
  > Este ejemplo usa el puerto 1433 comunes. Sin embargo, este valor deberá modificarse si se especificó un puerto diferente (por ejemplo, 1401) durante la implementación de la máquina virtual de SQL Server. 


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el uso de SQL Server en Azure, consulte Ia página sobre [SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) y las [Preguntas más frecuentes](virtual-machines-windows-sql-server-iaas-faq.md).