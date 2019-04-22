---
title: Preguntas más frecuentes sobre SQL Server en Windows Virtual Machines de Azure | Microsoft Docs
description: Este artículo brinda respuestas a las preguntas más frecuentes sobre cómo ejecutar SQL Server en Máquinas virtuales de Azure.
services: virtual-machines-windows
documentationcenter: ''
author: v-shysun
manager: felixwu
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/12/2018
ms.author: v-shysun
ms.openlocfilehash: 5299437dea18510fa5f85ee27240c8afc434d125
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680811"
---
# <a name="frequently-asked-questions-for-sql-server-running-on-windows-virtual-machines-in-azure"></a>Preguntas más frecuentes para SQL Server en máquinas virtuales de Windows en Azure

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](../../linux/sql/sql-server-linux-faq.md)

En este artículo se ofrecen respuestas a algunas de las preguntas más comunes sobre la ejecución de [SQL Server en máquinas virtuales de Windows en Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [!NOTE]
> Este artículo se centra en los problemas específicos de SQL Server en máquinas virtuales Windows. Si ejecuta SQL Server en máquinas virtuales Linux, consulte las [preguntas más frecuentes sobre Linux](../../linux/sql/sql-server-linux-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a id="images"></a> Imágenes

1. **¿Qué imágenes de la galería de máquinas virtuales de SQL Server están disponibles?**

   Azure mantiene imágenes de máquinas virtuales para todas las versiones principales de SQL Server admitidas en todas las ediciones para Windows y Linux. Para obtener más información, consulte la lista completa de [imágenes de VM de Windows](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo) e [imágenes de VM de Linux](../../linux/sql/sql-server-linux-virtual-machines-overview.md#create).

1. **¿Están actualizadas las imágenes existentes de la galería de máquinas virtuales de SQL Server?**

   Cada dos meses, las imágenes de SQL Server de la galería de máquinas virtuales se actualizan con las últimas actualizaciones de Windows y Linux. En el caso de las imágenes de Windows, esto incluye cualquier actualización que se marque como importante en Windows Update, incluidas las actualizaciones de seguridad importantes de SQL Server y los Service Pack. En el caso de las imágenes de Linux, esto incluye las últimas actualizaciones del sistema. Las actualizaciones acumulativas de SQL Server se tratan de forma diferente para Linux y Windows. En Linux, las actualizaciones acumulativas de SQL Server también se incluyen en la actualización. Pero en este momento, las máquinas virtuales de Windows no se actualizan con las actualizaciones acumulativas de SQL Server o Windows Server.

1. **¿Se pueden quitar las imágenes de máquinas virtuales de SQL Server de la galería?**

   Sí. Azure solo mantiene una imagen por edición y versión principal. Por ejemplo, cuando se lanza un nuevo Service Pack de SQL Server, Azure agrega una nueva imagen a la galería para dicho Service Pack. La imagen SQL Server para el Service Pack anterior se quita inmediatamente de Azure Portal. Sin embargo, todavía está disponible desde PowerShell para su aprovisionamiento durante los tres meses siguientes. Transcurridos tres meses, la imagen del Service Pack anterior dejará de estar disponible. Esta directiva de eliminación también se aplicaría si una versión de SQL Server no recibe soporte técnico cuando llega al final de su ciclo de vida.


1. **¿Es posible implementar una imagen anterior de SQL Server que no está visible en el portal de Azure?**

   Sí, mediante PowerShell. Para obtener más información sobre cómo implementar VM con SQL Server mediante PowerShell, consulte [Aprovisionamiento de máquinas virtuales de SQL Server con Azure PowerShell](virtual-machines-windows-ps-sql-create.md).

1. **¿Puedo crear una imagen de disco duro virtual a partir de una VM de SQL Server?**

   Sí, pero debe tener en cuenta algunas consideraciones. Si implementa este disco duro virtual en una nueva máquina virtual en Azure, no obtendrá la sección de configuración de SQL Server en el portal. A continuación, debe administrar las opciones de configuración de SQL Server a través de PowerShell. Además, se le cobrará según la tarifa de VM de SQL en la que se basaba originalmente la imagen. Esto ocurre incluso si quita SQL Server del disco duro virtual antes de implementar. 

1. **¿Es posible configurar configuraciones que no aparecen en la galería de máquinas virtuales (por ejemplo, Windows 2008 R2 + SQL Server 2012)?**

    No. En el caso de las imágenes de la galería de máquinas virtuales que incluyen SQL Server, debe seleccionar una de las imágenes que se proporcionan a través de Azure Portal o mediante [PowerShell](virtual-machines-windows-ps-sql-create.md). 


## <a name="creation"></a>Creación

1. **¿Cómo puedo crear una máquina virtual de Azure con SQL Server?**

   La solución más simple es crear una máquina virtual que incluya SQL Server. Para ver un tutorial sobre cómo suscribirse a Azure y crear una máquina virtual de SQL desde el portal, consulte [Aprovisionamiento de una máquina virtual de SQL Server en Azure Portal](virtual-machines-windows-portal-sql-server-provision.md). Puede seleccionar una imagen de máquina virtual que use licencias de SQL Server de pago por segundo o puede utilizar una imagen que le permita traer su propia licencia de SQL Server. También tiene la opción de instalar manualmente SQL Server en una máquina virtual con una edición de licencia libre (Developer o Express) o reutilizando una licencia local. Si trae su propia licencia, debe disponer de [Movilidad de Licencias a través de Software Assurance en Azure](https://azure.microsoft.com/pricing/license-mobility/). Para más información, vea [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Orientación de precios de máquinas virtuales de SQL Server Azure).

1. **¿Cómo puedo migrar mi base de datos local de SQL Server a la nube?**

   En primer lugar, cree una máquina virtual de Azure con una instancia de SQL Server. Luego, migre las bases de datos locales a esa instancia. Para conocer las estrategias de migración de datos, consulte [Migración de una Base de datos SQL Server a SQL Server en una máquina virtual de Azure](virtual-machines-windows-migrate-sql.md).

## <a name="licensing"></a>Licencias

1. **¿Cómo puedo instalar mi copia de SQL Server con licencia en una máquina virtual de Azure?**

   Existen dos formas de hacerlo. Puede aprovisionar una de las [imágenes de máquina virtual que admita licencias](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), lo que también se conoce como traiga su propia licencia (BYOL). También existe la posibilidad de copiar los medios de instalación de SQL Server en la máquina virtual de Windows Server y, luego, instalar SQL Server en la máquina virtual. En cambio, si instala manualmente SQL Server, no habrá ninguna integración del portal y no se admitirá la extensión del agente de IaaS de SQL Server, por lo que las características como Copia de seguridad automática y Aplicación de revisiones no funcionarán en este escenario. Por este motivo, se recomienda usar una de las imágenes de la galería de BYOL. Para usar BYOL o sus propios medios de SQL Server en una máquina virtual de Azure, debe tener [Movilidad de licencias a través de Software Assurance en Azure](https://azure.microsoft.com/pricing/license-mobility/). Para más información, vea [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Orientación de precios de máquinas virtuales de SQL Server Azure).


1. **¿Tengo que pagar para obtener una licencia de SQL Server en una máquina virtual de Azure si solo se usa para conmutación por error o en modo en espera?**

   Si tiene Software Assurance y usa la movilidad de licencias como se describe en [p+f sobre licencias de máquina Virtual](https://azure.microsoft.com/pricing/licensing-faq/), no tendrá que pagar por obtener una licencia SQL Server como réplica secundaria pasiva en una implementación de alta disponibilidad. En caso contrario, deberá pagar para adquirir la licencia.

1. **¿Puedo cambiar una máquina virtual para usar mi propia licencia de SQL Server si se ha creado desde una de las imágenes de la galería de pago por uso?**

   Sí. Si originalmente comenzó con una imagen de la galería de pago por uso, puede moverse fácilmente entre los dos modelos de licencias. Sin embargo, si inicialmente comenzó con una imagen BYOL, no podrá cambiar la licencia a pago por uso. Para más información, consulte [Modificación del modelo de licencia para una máquina virtual de SQL Server en Azure](virtual-machines-windows-sql-ahb.md).

   > [!Note]
   > Actualmente, esta función solo está disponible para clientes de nube pública.

1. **¿Debo usar imágenes de BYOL o RP de VM de SQL para crear la nueva VM de SQL?**

   Las imágenes de traiga su propia licencia (BYOL) solo están disponibles para clientes de EA. Otros clientes que tengan Software Assurance deben usar el proveedor de recursos de VM de SQL para crear una VM de SQL con [Ventaja híbrida de Azure (AHB)](https://azure.microsoft.com/pricing/licensing-faq/). 

1. **¿El cambio de modelos de licencia requerirá un tiempo de inactividad para SQL Server?**

   No. El [cambio del modelo de licencias](virtual-machines-windows-sql-ahb.md) no requiere ningún tiempo de inactividad para SQL Server, ya que el cambio es efectivo de inmediato y no requiere un reinicio de la VM. Sin embargo, para registrar la máquina virtual con SQL Server en el proveedor de recursos de máquina virtual con SQL, la [extensión SQL IaaS](virtual-machines-windows-sql-server-agent-extension.md) es un requisito previo y la instalación de la extensión SQL IaaS reinicia el servicio SQL Server. Por lo tanto, si es preciso instalar la extensión SQL IaaS, la instalación debería realizase durante un periodo de mantenimiento. 

1. **¿Las suscripciones de CSP pueden activar la Ventaja híbrida de Azure?**

   Sí, Ventaja híbrida de Azure está disponible para suscripciones de CSP. En primer lugar, los clientes de CSP deben implementar una imagen de pago por uso y, después, [cambiar el modelo de licencias](virtual-machines-windows-sql-ahb.md) a traiga su propia licencia.  

1. **¿El registro de mi VM con el nuevo proveedor de recursos de VM de SQL implicará costos adicionales?**

   No. El proveedor de recursos de VM de SQL simplemente permite la capacidad de administración adicional para SQL Server en VM de Azure sin cargos adicionales. 

1. **¿El proveedor de recursos de VM de SQL está disponible para todos los clientes?**
 
   Sí. Todos los clientes pueden registrarse con el nuevo proveedor de recursos de VM de SQL. Sin embargo, solo los clientes con el Beneficio de Software Assurance pueden activar la [Ventaja híbrida de Azure (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) (o BYOL) en una VM con SQL Server. 

1. **¿Qué ocurre con el _Microsoft.SqlVirtualMachine_ recurso si se mueve o se quita el recurso de máquina virtual?** 

   Cuando el recurso Microsoft.Compute/VirtualMachine se coloca o se mueve, se envía una notificación al recurso Microsoft.SqlVirtualMachine asociado para replicar de forma asincrónica la operación.

1. **¿Qué ocurre con la máquina virtual si la _Microsoft.SqlVirtualMachine_ se quita el recurso?**

    El recurso Microsoft.Compute/VirtualMachine no se ve afectado cuando se coloca el recurso Microsoft.SqlVirtualMachine. Sin embargo, los cambios en las licencias devolverán el valor predeterminado al origen de la imagen original. 

1. **¿Es posible registrar VM de SQL Server implementadas automáticamente con el proveedor de recursos de VM de SQL?**

    Sí. Si ha implementado SQL Server desde sus propios medios e instalado la extensión IaaS de SQL, puede registrar la VM con SQL Server con el proveedor de recursos para obtener las ventajas de capacidad de administración proporcionadas por la extensión IaaS de SQL. Sin embargo, no es posible convertir una máquina virtual de SQL implementada automáticamente a pago por uso.

## <a name="administration"></a>Administración

1. **¿Puedo instalar una segunda instancia de SQL Server en la misma máquina virtual? ¿Puedo cambiar las características instaladas de la instancia predeterminada?**

   Sí. Los medios de instalación de SQL Server están ubicados en una carpeta en la unidad **C** . Ejecute **Setup.exe** desde esa ubicación para agregar instancias de SQL Server nuevas o para cambiar otras características instaladas de SQL Server en la máquina. Tenga en cuenta que algunas características, como Automated Backup, Automated Patching e integración de Azure Key Vault, solo funcionan con la instancia predeterminada o con nombre de instancia que se ha configurado correctamente (consulte la pregunta 3). 

1. **¿Puedo desinstalar la instancia predeterminada de SQL Server?**

   Sí, pero debe tener en cuenta algunas consideraciones. Como se indicó en la respuesta anterior, hay características que se basan en el [extensión del agente de IaaS de SQL Server](virtual-machines-windows-sql-server-agent-extension.md).  Si desinstala la instancia predeterminada sin quitar también la extensión de IaaS, la extensión continúa buscándola y puede generar errores de registro de eventos. Estos errores proceden de los dos orígenes siguientes: **Administración de credenciales de Microsoft SQL Server** y **Agente de IaaS de Microsoft SQL Server**. Es posible que uno de los errores sea similar al siguiente:

      Se ha producido un error relacionado con la red o específico de la instancia al establecer una conexión en SQL Server. No se encontró el servidor o no era accesible.

   Si decide desinstalar la instancia predeterminada, desinstale también la [Extensión del Agente de IaaS de SQL Server](virtual-machines-windows-sql-server-agent-extension.md).

1. **¿Puedo usar una instancia con nombre de SQL Server con la extensión IaaS**?
   
   Sí, si la instancia con nombre es la única instancia de SQL Server, y si la instancia predeterminada original era [desinstalado correctamente](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md#installation). Si no hay ninguna instancia predeterminada y hay varias instancias con nombre en una sola máquina virtual de SQL Server, no podrá instalar la extensión de IaaS. 

1. **¿Puedo quitar SQL Server por completo de una VM de SQL?**

   Sí, pero se le seguirá cobrando por la VM de SQL como se describe en [Orientación de precios de SQL Server para máquinas virtuales de Azure](virtual-machines-windows-sql-server-pricing-guidance.md). Si ya no necesita SQL Server, puede implementar una nueva máquina virtual y migrar los datos y aplicaciones a la nueva máquina virtual. Después, puede quitar la máquina virtual de SQL Server.
   
## <a name="updating-and-patching"></a>Actualizaciones y revisiones

1. **¿Cómo puedo cambiar a una nueva versión o edición de SQL Server en una máquina virtual de Azure?**

   Los clientes con Software Assurance pueden actualizaciones locales de SQL Server que se ejecuta en una máquina virtual de Azure mediante los medios de instalación en el Portal de licencias por volumen. Actualmente, sin embargo, hay ninguna manera de cambiar la edición de una instancia de SQL Server. Cree una nueva máquina virtual de Azure con la edición de SQL Server deseada y, a continuación, migrar las bases de datos al nuevo servidor mediante el estándar [técnicas de migración de datos](virtual-machines-windows-migrate-sql.md).

1. **¿Cómo se aplican las actualizaciones y los Service Packs en una máquina virtual de SQL Server?**

   Las máquinas virtuales brindan control sobre la máquina virtual, incluido el momento en que se aplican las actualizaciones y la manera de hacerlas. En el caso del sistema operativo, puede aplicar manualmente las actualizaciones de Windows, o bien puede habilitar un servicio de programación llamado [Aplicación de revisiones automatizada](virtual-machines-windows-sql-automated-patching.md). Aplicación de revisión automatizada instala las actualizaciones marcadas como importantes, incluidas las actualizaciones de SQL Server que tienen esa categoría. Otras actualizaciones opcionales a SQL Server se deben instalar manualmente.

## <a name="general"></a>General

1. **¿Son compatibles las instancias del clúster de conmutación por error (FCI) de SQL Server con Azure Virtual Machines?**

   Sí. También puede [crear un clúster de conmutación por error de Windows en Windows Server 2016](virtual-machines-windows-portal-sql-create-failover-cluster.md) y usar Espacios de almacenamiento directo (S2D) para el almacenamiento del clúster. También puede usar soluciones de agrupación en clústeres o almacenamiento de terceros como se describe en [Alta disponibilidad y recuperación ante desastres para SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

   > [!IMPORTANT]
   > En este momento, la [extensión del agente de IaaS de SQL Server](virtual-machines-windows-sql-server-agent-extension.md) no es compatible con FCI de SQL Server en Azure. Se recomienda que desinstale la extensión de las máquinas virtuales que participan en el FCI. Esta extensión admite características, como la copia de seguridad y la aplicación de revisiones automatizadas y algunas características del portal de SQL. Estas características no funcionarán para las máquinas virtuales de SQL después de desinstalar el agente.

1. **¿Cuál es la diferencia entre las máquinas virtuales de SQL y el servicio SQL Database?**

   De manera conceptual, ejecutar SQL Server en una máquina virtual de Azure no es diferente a ejecutar SQL Server en un centro de datos remoto. En cambio, [SQL Database](../../../sql-database/sql-database-technical-overview.md) ofrece base de datos como servicio. Con SQL Database, no se tiene acceso a las máquinas que hospedan las bases de datos. Para una comparación completa, consulte [Choose a cloud SQL Server option: Azure SQL (PaaS) Database or SQL Server on Azure VMs (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md) (Selección de una opción Azure SQL en la nube: Azure SQL (PaaS) Database o SQL Server en máquinas virtuales de Azure (IaaS).

1. **¿Cómo instalo herramientas de datos de SQL en mi máquina virtual de Azure?**

    Descargue e instale las herramientas de datos de SQL desde [Microsoft SQL Server Data Tools - Business Intelligence para Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

1. **¿Son transacciones distribuidas con MSDTC compatible en máquinas virtuales de SQL Server?**
   
    Sí. DTC local es compatible con SQL Server 2016 SP2 y versiones posteriores. Sin embargo, las aplicaciones deben probarse cuando utilizando los grupos de disponibilidad Always On, como las transacciones en curso durante una conmutación por error se producirá un error y debe volver a intentarse. DTC agrupado está disponible a partir de Windows Server 2019. 

## <a name="resources"></a>Recursos

**Máquinas virtuales Windows**:

* [Introducción a SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-overview.md)
* [Aprovisionamiento de una máquina virtual Windows con SQL Server](virtual-machines-windows-portal-sql-server-provision.md)
* [Migración de una base de datos a SQL Server en una máquina virtual de Azure](virtual-machines-windows-migrate-sql.md)
* [Alta disponibilidad y recuperación ante desastres para SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md)
* [Prácticas recomendadas para mejorar el rendimiento para SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-performance.md)
* [Estrategias de desarrollo y patrones de aplicación de SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Máquinas virtuales Linux**:

* [Introducción a SQL Server en máquinas virtuales Linux](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Aprovisionamiento de una máquina virtual Linux con SQL Server](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Preguntas más frecuentes (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Documentación de SQL Server en Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
