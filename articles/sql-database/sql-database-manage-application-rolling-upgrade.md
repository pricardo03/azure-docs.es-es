---
title: 'Actualizaciones graduales de aplicaciones: Azure SQL Database | Microsoft Docs'
description: Aprenda a usar la replicación geográfica en SQL Database para admitir las actualizaciones en línea de la aplicación en la nube.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 01/29/2019
ms.openlocfilehash: 50f6f114a4d90f48218f751e1649e8694e664491
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55295753"
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>Administración de actualizaciones graduales de aplicaciones en la nube mediante la replicación geográfica activa de SQL Database

Aprenda a usar la [replicación geográfica activa](sql-database-auto-failover-group.md) en SQL Database para habilitar las actualizaciones graduales de su aplicación en la nube. Como la actualización es una operación problemática, pues debe ser parte del diseño y planeamiento de continuidad. En este artículo veremos dos métodos diferentes de organizar el proceso de actualización y comentaremos las ventajas y desventajas de cada opción. Para este artículo, usaremos una aplicación que consta de un sitio web conectado a una base de datos única como su capa de datos. Nuestro objetivo consiste en actualizar la versión 1 de la aplicación a la versión 2 sin que se observe ningún impacto importante en la experiencia del usuario final.

Al evaluar las opciones de actualización, se deben considerar los factores siguientes:

* El impacto sobre la disponibilidad de las aplicaciones durante las actualizaciones. Durante cuánto tiempo la función de aplicación debe estar limitada o degradada.
* Capacidad de revertir si se produce un error de actualización.
* La vulnerabilidad de la aplicación si se produce un error grave no relacionado durante la actualización.
* El costo total en dólares.  Incluye redundancia adicional y costos incrementales de los componentes temporales usados en el proceso de actualización.

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>La actualización de las aplicaciones que dependen de las copias de seguridad de base de datos para recuperación ante desastres.

Si la aplicación se basa en copias de seguridad automáticas y utiliza la restauración geográfica para la recuperación ante desastres, se implementa en una única región de Azure. Para minimizar las interrupciones del usuario final, creará un entorno de ensayo en dicha región con todos los componentes de la aplicación implicados en la actualización. En el siguiente diagrama se ilustra el entorno operativo antes del proceso de actualización. El punto de conexión `contoso.azurewebsites.net` representa un espacio de producción de la aplicación web. Para habilitar la capacidad de revertir la actualización, necesita crear un espacio de ensayo con una copia totalmente sincronizada de la base de datos. Los siguientes pasos crearán un entorno de ensayo para la actualización:

1. Cree una base de datos secundaria en la misma región de Azure. Supervise la base de datos secundaria para ver si se completa el proceso de propagación (1).
2. Cree una nueva ranura de implementación para la aplicación web denominada "Ensayo". Se registrará en DNS con la dirección URL `contoso-staging.azurewebsites.net` (2).

> [!NOTE]
> Tenga en cuenta que los pasos preparatorios no afectarán al espacio de producción y puede funcionar en modo de acceso completo.
>  

![Configuración de la replicación geográfica de la SQL Database. Recuperación ante desastres en la nube.](media/sql-database-manage-application-rolling-upgrade/option1-1.png)

Una vez completados los pasos preparatorios, la aplicación está preparada para la actualización real. En el siguiente diagrama se ilustran los pasos implicados en el proceso de actualización.

1. Establecimiento de la base de datos principal en modo de solo lectura (3). Este modo garantizará que el espacio de producción de la aplicación web (V1) seguirá siendo de solo lectura durante la actualización, con lo que se impide la divergencia de datos entre las instancias de base de datos de V1 y V2.  
2. Desconecte la base de datos secundaria con el modo de finalización planeada (4). Se creará una copia independiente totalmente sincronizada de la base de datos principal. Esta base de datos se actualizará.
3. Cambie la base de datos principal al modo de lectura y escritura y ejecute el script de actualización (5).

![Configuración de replicación geográfica de SQL Database. Recuperación ante desastres en la nube.](media/sql-database-manage-application-rolling-upgrade/option1-2.png)

Si la actualización se completó correctamente, ya está listo para cambiar los usuarios finales a la copia actualizada de la aplicación. Ahora se convertirá en un espacio de producción.  Ese cambio implica unos pocos pasos más, tal como se muestra en el diagrama siguiente.

1. Active una operación de intercambio entre los espacios de ensayo y producción de la aplicación web (6). Así se cambiarán las direcciones URL de los dos espacios. Ahora `contoso.azurewebsites.net` apuntará a la versión V2 del sitio web y la base de datos (entorno de producción).  
2. Si ya no necesita la versión V1, que se convirtió en una copia de ensayo después del intercambio, puede retirar el entorno de ensayo (7).

![Configuración de replicación geográfica de SQL Database. Recuperación ante desastres en la nube.](media/sql-database-manage-application-rolling-upgrade/option1-3.png)

Si el proceso de actualización es incorrecto, por ejemplo debido a un error en el script de actualización, la ranura de almacenamiento provisional debe considerarse en peligro. Para revertir la aplicación al estado previo a la actualización, revierta la aplicación en la ranura de producción al acceso completo. En el diagrama siguiente se muestran los pasos implicados.

1. Establezca la copia de la base de datos en modo de lectura y escritura (8). Esto restaurará la funcionalidad de V1 completa de la copia de producción.
2. Realice el análisis de la causa principal y retire el entorno de ensayo (9).

En este punto, la aplicación es totalmente funcional y se pueden repetir los pasos de actualización.

> [!NOTE]
> La reversión no requiere cambios de DNS, ya que aún no ha realizado una operación de intercambio.

![Configuración de replicación geográfica de SQL Database. Recuperación ante desastres en la nube.](media/sql-database-manage-application-rolling-upgrade/option1-4.png)

La principal **ventaja** de esta opción es que se puede actualizar una aplicación en una sola región mediante un conjunto de pasos sencillos. El costo de la actualización es relativamente bajo. La principal **contrapartida** es que si se produce un error grave durante la actualización, la recuperación al estado previo a la actualización implicará que haya que volver a implementar la aplicación en una región distinta y restaurar la base de datos desde la copia de seguridad mediante la restauración geográfica. Este proceso dará como resultado un tiempo de inactividad considerable.

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Actualización de aplicaciones que dependen de la replicación geográfica para la recuperación ante desastres

Si la aplicación aprovecha la replicación geográfica activa o los grupos de conmutación por error para la continuidad empresarial, se implementa en al menos dos regiones diferentes con una base de datos principal activa en la región primaria y una base de datos secundaria de solo lectura en la región de copia de seguridad. Además de los factores mencionados anteriormente, el proceso de actualización debe garantizar que:

* La aplicación permanece protegida frente a errores catastróficos en todo momento durante el proceso de actualización.
* Los componentes con redundancia geográfica de la aplicación se actualizan en paralelo con los componentes activos.

Para lograr estos objetivos, además de usar las ranuras de implementación de la aplicación web, usará Azure Traffic Manager (ATM) con un perfil de conmutación por error con un punto de conexión activo y otro de copia de seguridad.  En el siguiente diagrama se ilustra el entorno operativo antes del proceso de actualización. Los sitios web `contoso-1.azurewebsites.net` y `contoso-dr.azurewebsites.net` representan un entorno de producción de la aplicación con redundancia geográfica completa. El entorno de producción incluye los siguientes componentes:

1. Ranura de producción de la aplicación web `contoso-1.azurewebsites.net` en la región principal (1)
2. Base de datos principal en la región principal (2) 
3. Una instancia en espera de la aplicación web en la región de copia de seguridad (3)
4. Base de datos secundaria de replicación geográfica en la región de copia de seguridad (4)
5. Perfil de rendimiento de Azure Traffic Manager con un punto de conexión en línea `contoso-1.azurewebsites.net` y punto de conexión sin conexión `contoso-dr.azurewebsites.net`

Para habilitar la capacidad de revertir la actualización, necesita crear un entorno de ensayo con una copia totalmente sincronizada de la aplicación. Dado que debe asegurarse de que la aplicación puede recuperarse rápidamente en caso de que se produzca un error grave durante el proceso de actualización, el entorno de ensayo tiene que tener también redundancia geográfica. Estos son los pasos necesarios para crear un entorno de ensayo para la actualización:

1. Implemente un espacio de ensayo de la aplicación web en la región principal (6)
2. Cree una base de datos secundaria en la región principal de Azure (7). Configure el espacio de ensayo de la aplicación web para conectarse a él. 
3. Cree otra base de datos secundaria con redundancia geográfica en la región de copia de seguridad mediante la replicación de la base de datos secundaria en la región principal (esto se denomina "replicación geográfica encadenada") (8).
3. Implemente un espacio de ensayo de la instancia de la aplicación web en la región de copia de seguridad (9) y configúrelo para conectarse a la replicación geográfica secundaria creada en el paso (9).


> [!NOTE]
> Tenga en cuenta que los pasos preparatorios no afectarán a la aplicación del espacio de producción y permanecerán completamente funcionales en el modo de lectura-escritura.

![Configuración de replicación geográfica de SQL Database. Recuperación ante desastres en la nube.](media/sql-database-manage-application-rolling-upgrade/option2-1.png)

Una vez completados los pasos preparatorios, el entorno de ensayo estará preparado para la actualización. En el siguiente diagrama se ilustran los pasos de actualización:

1. Establezca la base de datos principal en el espacio de producción en modo de solo lectura (10). Este modo garantizará que la base de datos de producción (V1) no cambiará durante la actualización, con lo que se impide la divergencia de datos entre las instancias de base de datos de V1 y V2.  
2. Desconecte la base de datos secundaria de la misma región con el modo de finalización planeada (11). Se creará una copia independiente aunque totalmente sincronizada de la base de datos de producción. Esta base de datos se actualizará.
3. Ejecute el script de actualización en `contoso-1-staging.azurewebsites.net`, `contoso-dr-staging.azurewebsites.net` y la base de datos principal de ensayo (12). Los cambios de la base de datos se replicarán automáticamente en la base de datos secundaria de ensayo. 

![Configuración de replicación geográfica de SQL Database. Recuperación ante desastres en la nube.](media/sql-database-manage-application-rolling-upgrade/option2-2.png)

Si la actualización se completó correctamente, ya está listo para cambiar los usuarios finales a la versión V2 de la aplicación. En el siguiente diagrama se ilustran los pasos implicados.

1. Active una operación de intercambio entre los espacios de ensayo y producción de la aplicación web en la región principal (13) y en la región de copia de seguridad (14). La versión V2 de la aplicación se convertirá ahora en un espacio de producción con una copia redundante en la región de copia de seguridad.
2. Puede retirar el entorno de ensayo si ya no necesita la aplicación V1 (15 y 16).  

![Configuración de replicación geográfica de SQL Database. Recuperación ante desastres en la nube.](media/sql-database-manage-application-rolling-upgrade/option2-3.png)

Si el proceso de actualización es incorrecto, por ejemplo debido a un error en el script de actualización, el entorno de ensayo debe considerarse en estado incoherente. Para revertir la aplicación al estado previo a la actualización, revierta al uso de la versión V1 de la aplicación en el entorno de producción. En el diagrama siguiente se muestran los pasos necesarios.

1. Establezca la copia de la base de datos principal en el espacio de producción en modo de lectura y escritura (17). Esto restaurará la funcionalidad de V1 completa en el espacio de producción.
2. Realice el análisis de la causa principal y repare o quite el entorno de ensayo (18 y 19).

En este punto, la aplicación es totalmente funcional y se pueden repetir los pasos de actualización.

> [!NOTE]
> La reversión no requiere cambios de DNS, ya que no ha realizado una operación de intercambio.

![Configuración de replicación geográfica de SQL Database. Recuperación ante desastres en la nube.](media/sql-database-manage-application-rolling-upgrade/option2-4.png)

La principal **ventaja** de esta opción es que puede actualizar tanto la aplicación como su copia con redundancia geográfica en paralelo, sin poner en peligro la continuidad del negocio durante la actualización. La principal **contrapartida** es que requiere redundancia doble de cada componente de aplicación y, por tanto, implica un mayor costo. También implica un flujo de trabajo más complicado.

## <a name="summary"></a>Resumen

Los dos métodos de actualización que se describen en el artículo difieren en complejidad y en costo, pero ambos se centran en minimizar el tiempo en que el usuario final está limitado a las operaciones de solo lectura. La duración del script de actualización define directamente ese tiempo. No depende del tamaño de la base de datos, del nivel de servicio que haya seleccionado, de la configuración del sitio web y de otros factores que no se pueden controlar fácilmente. Todos los pasos preparatorios se desacoplan de los pasos de actualización y no afectan a la aplicación de producción. La eficacia del script de actualización es un factor clave que determina la experiencia del usuario final durante las actualizaciones. Por tanto, la mejor manera de que pueda mejorarlo es centrar sus esfuerzos en hacer que el script de actualización sea lo más eficiente posible.  

## <a name="next-steps"></a>Pasos siguientes

* Para obtener una descripción general y los escenarios de la continuidad empresarial, consulte [Información general sobre la continuidad empresarial](sql-database-business-continuity.md).
* Para obtener información acerca de la replicación geográfica activa de Azure SQL Database, consulte [Creación de bases de datos secundarias legibles mediante la replicación geográfica activa](sql-database-active-geo-replication.md).
* Para obtener información sobre los grupos de conmutación por error de Azure SQL Database, consulte [Uso de grupos de conmutación por error automática para permitir la conmutación por error de varias bases de datos de manera transparente y coordinada](sql-database-auto-failover-group.md).
* Para obtener información acerca de las ranuras de implementación y el entorno de ensayo en Azure App Service, consulte [Configuración de entornos de ensayo en Azure App Service](../app-service/deploy-staging-slots.md).  
* Para obtener información sobre perfiles de Azure Traffic Manager, consulte [Administrar un perfil de Azure Traffic Manager](../traffic-manager/traffic-manager-manage-profiles.md).  


