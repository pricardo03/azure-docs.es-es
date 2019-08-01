---
title: Preguntas frecuentes acerca de Instancia administrada de Azure SQL Database | Microsoft Docs
description: Preguntas frecuentes acerca de Instancia administrada de Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
ms.date: 07/16/2019
ms.openlocfilehash: 32da92fb3c5573d24eb18a01a4ede8fe6a0bf36a
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567460"
---
# <a name="sql-database-managed-instance-frequently-asked-questions-faq"></a>Preguntas frecuentes acerca de Instancia administrada de Azure SQL Database

En este artículo encontrará muchas de las preguntas más frecuentes acerca de [Instancia administrada de SQL Database](sql-database-managed-instance.md).

## <a name="where-can-i-find-a-list-of-features-supported-on-managed-instance"></a>¿Dónde puedo encontrar una lista de las características que se admiten en una instancia administrada?

Para obtener una lista de las características admitidas en una instancia administrada, consulte [Azure SQL Database frente a SQL Server](sql-database-features.md).

Para conocer las diferencias de sintaxis y comportamiento entre Instancia administrada de Azure SQL Database y SQL Server local, consulte [Diferencias entre T-SQL y SQL Server](sql-database-managed-instance-transact-sql-information.md).


## <a name="where-can-i-find-technical-characteristics-and-resource-limits-for-managed-instance"></a>¿Dónde puedo encontrar las características técnicas y los límites de recursos de una instancia administrada?

Para conocer las características disponibles para la generación de hardware, consulte las [diferencias técnicas en las generaciones de hardware](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics).
Para ver los niveles de servicio disponibles y sus características, consulte las [diferencias técnicas entre los niveles de servicio](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

## <a name="where-can-i-find-known-issues-and-bugs"></a>¿Dónde puedo encontrar soluciones a errores y problemas conocidos?

Para conocer cuáles son los errores y problemas conocidos, consulte los apartados [Cambios de comportamiento](sql-database-managed-instance-transact-sql-information.md#Changes) y [Problemas conocidos](sql-database-managed-instance-transact-sql-information.md#Issues).


## <a name="can-a-managed-instance-have-the-same-name-as-on-premises-sql-server"></a>¿Se pueden llamar igual una instancia administrada y SQL Server en un entorno local?

El nombre de la instancia administrada debe terminar en *database.windows.net*. Para usar otra zona DNS en lugar de la predeterminada, por ejemplo, **mi otro nombre**.contoso.com: 
- Use CliConfig para definir un alias. La herramienta no es más que un contenedor de la configuración del registro, por lo que también se puede hacer mediante una directiva de grupo o un script.
- Use *CNAME* con la opción *TrustServerCertificate = true*.


## <a name="how-can-i-move-database-from-managed-instance-back-to-sql-server-or-azure-sql-database"></a>¿Cómo puedo devolver una base de datos de una instancia administrada a SQL Server o Azure SQL Database?

Puede [exportar la base de datos a BACPAC](sql-database-export.md) y, a continuación, [importar el archivo BACPAC]( sql-database-import.md). Este enfoque es aconsejable si la base de datos tiene menos de 100 GB.

La replicación transaccional se puede utilizar si todas las tablas de la base de datos tienen claves principales.

Las copias de seguridad nativas `COPY_ONLY` realizadas de una instancia administrada no se pueden restaurar en SQL Server, porque la instancia administrada tiene una versión superior de la base de datos en comparación con SQL Server.

## <a name="how-can-i-migrate-my-instance-database-to-a-single-azure-sql-database"></a>¿Cómo puedo migrar mi base de datos de instancia a una instancia individual de Azure SQL Database?

Una opción es [exportar la base de datos a un archivo BACPAC](sql-database-export.md) y, a continuación, [importar dicho archivo]( sql-database-import.md). 

Este enfoque es aconsejable si la base de datos tiene menos de 100 GB. La replicación transaccional se puede utilizar si todas las tablas de la base de datos tienen claves principales.

## <a name="how-do-i-choose-between-gen-4-and-gen-5-hardware-generation-for-managed-instance"></a>¿Cómo se elige entre las generaciones de hardware Gen 4 y Gen 5 para una instancia administrada?

Depende de la carga de trabajo, ya que una generación de hardware es mejor para ciertos tipos de cargas de trabajo que la otra. Aunque el asunto del rendimiento es bastante complejo para simplificarlo, las siguientes diferencias entre las generaciones de hardware afectan al rendimiento de las cargas de trabajo:
- Gen 4 proporciona mejor compatibilidad de proceso, ya que se basa en procesadores físicos, frente a Gen 5, que se basa en los procesadores de núcleo virtual. Esta generación puede que sea más útil para cargas de trabajo en las que hay muchos procesos.
- Gen 5 admite una red acelerada, lo que mejora el ancho de banda de E/S en el almacenamiento remoto. Esto puede suponer una ventaja para cargas de trabajo con mucha E/S de los niveles de servicio de uso general. Gen 5 utiliza discos locales SSD más rápidos que Gen 4. Esto puede suponer una ventaja en las cargas de trabajo con mucha E/S de los niveles de servicio críticos de la empresa.

Se recomienda encarecidamente que pruebe el rendimiento de las cargas de trabajo reales destinadas a producción antes de activarlas para determinar qué generación de hardware funcionará mejor en su caso.

## <a name="can-i-switch-my-managed-instance-hardware-generation-between-gen-4-and-gen-5-online"></a>¿Puedo cambiar la generación de hardware de mi instancia administrada entre Gen 4 y Gen 5 en línea? 

La conmutación en línea automatizada entre dos generaciones de hardware es posible si ambas están disponibles en la misma región en la que está aprovisionada la instancia administrada. En este caso, tiene una opción en la sección del plan de tarifa de Azure Portal para conmutar entre las generaciones de hardware.

Se trata de una operación de larga duración, ya que la nueva instancia administrada se aprovisionará en segundo plano y en las bases de datos transferidas automáticamente entre la instancia antigua y la nueva, con una conmutación por error rápida al final del proceso. 

Si las dos generaciones de hardware no se admiten en la misma región, es posible cambiar una de ellas, pero se debe hacer manualmente. Esto requiere que se aprovisione una nueva instancia en la región en que esté disponible la generación de hardware que se quiera llevar a cabo, y que se realice una copia de seguridad manual y una restauración de los datos entre las instancias antigua y nueva.


## <a name="how-do-i-tune-performance-of-my-managed-instance"></a>¿Cómo se optimiza el rendimiento de una instancia administrada? 

Una instancia administrada de uso general usa el almacenamiento remoto debido a que el tamaño de los archivos de registro y datos es importante para el rendimiento. Para optimizar el rendimiento del nivel de servicio de uso general, siga las instrucciones de esta entrada de blog.

En el caso de cargas de trabajo con un uso intensivo de E/S, considere la posibilidad de uso de hardware de Gen 5, mientras que se recomienda usar Gen 4 en las que haya un uso elevado de procesos. Para más información, consulte en la sección de preguntas frecuentes cómo elegir la generación de hardware.

Si la carga de trabajo consta de muchas transacciones pequeñas, considere la posibilidad de cambiar el tipo de conexión de proxy a modo de redirección.

## <a name="what-is-the-maximum-storage-size-for-managed-instance"></a>¿Qué es el tamaño de almacenamiento máximo de una instancia administrada? 

El tamaño de almacenamiento de una instancia administrada depende del nivel de servicio seleccionado (De uso general o Crítico para la empresa). Para conocer las limitaciones de almacenamiento de estos niveles de servicio, consulte [Características de los niveles de servicio](sql-database-service-tiers-general-purpose-business-critical.md).

## <a name="is-the-backup-storage-deducted-from-my-managed-instance-storage"></a>¿Se deduce el almacenamiento de copia de seguridad del almacenamiento de mi instancia administrada? 

No, el almacenamiento de copia de seguridad no se deduce del espacio de almacenamiento de su instancia administrada. El almacenamiento de copia de seguridad es independiente del espacio de almacenamiento de la instancia y su tamaño no está limitado. El almacenamiento de copia de seguridad está limitado por el período de tiempo durante el que se conserva la copia de seguridad de las bases de datos de la instancia; se puede configurar entre 7 y 35 días. Para más información, consulte [Copias de seguridad automatizadas](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
  
## <a name="how-can-i-set-inbound-nsg-rules-on-management-ports"></a>¿Cómo se pueden establecer reglas de grupos de seguridad de red de entrada en los puertos de administración?

La característica de firewall integrado configura firewall de Windows en todas las máquinas virtuales del clúster para permitir conexiones entrantes de los rangos de IP asociados solo a máquinas de administración e implementación de Microsoft y estaciones de trabajo de administración segura para evitar que se puedan producir intrusiones a través de la capa de red.

Esto es para lo que se usan los puertos:

La infraestructura de Service Fabric usa los puertos 9000 y 9003. El rol principal de Service Fabric consiste en mantener siempre el clúster virtual en un estado correcto y en mantener el estado del objetivo en cuanto al número de réplicas de componente.

Node Agent utiliza los puertos 1438, 1440 y 1452. Node Agent es una aplicación que se ejecuta dentro del clúster y que el plano de control usa para ejecutar comandos de administración.

Además del firewall integrado de la capa de red, la comunicación también se protege con certificados.
  
Para más información acerca del firewall integrado y acerca de cómo verificarlo, consulte [Firewall integrado en Instancia administrada de Azure SQL Database](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).


## <a name="how-can-i-mitigate-networking-risks"></a>¿Cómo puedo mitigar los riesgos de la red? 

Para mitigar los riesgos de la red, es aconsejable que los clientes apliquen un conjunto de valores y controles de seguridad:

- Activar el [cifrado de datos transparente (TDE)](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql) en todas las bases de datos.
- Desactivar Common Language Runtime (CLR). Se recomienda hacerlo también en el entorno local.
- Usar solo la autenticación de Azure Active Directory (AAD).
- Acceder a la instancia con una cuenta de DBA con pocos privilegios.
- Configurar el acceso del jumpbox de JiT a la cuenta sysadmin.
- Activar la [auditoría de SQL](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) e integrarla en los mecanismos de alerta.
- Activar la [detección de amenazas](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) del conjunto de pruebas de [Advanced Data Security (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security).


## <a name="where-can-i-find-use-cases-and-resulting-cost-savings-with-managed-instance"></a>¿Dónde puedo encontrar casos de uso y los ahorros de costos resultantes con una instancia administrada?

Casos prácticos de instancia administrada:

- [Komatsu](http://customers.microsoft.com/story/komatsu-australia-manufacturing-azure)
- [powerdetails](http://customers.microsoft.com/story/powerdetails-partner-professional-services-azure-sql-database-managed-instance)
- [Allscripts](http://customers.microsoft.com/story/allscripts-partner-professional-services-azure)   
Existe un estudio de Forrester en el que se explican a la perfección los beneficios, costos y riesgos asociados a la implementación de Instancia administrada de Azure SQL Database: [Total Economic Impact of MI](https://azure.microsoft.com/resources/forrester-tei-sql-database-managed-instance) (Impacto económico total de Instancia administrada de Azure SQL Database).


## <a name="can-i-do-dns-refresh"></a>¿Puedo actualizar DNS? 
  
Actualmente, no proporcionamos ninguna característica para actualizar la configuración del servidor DNS para la instancia administrada.

La configuración de DNS se actualiza:

- Cuando expira la concesión de DHCP.
- Al actualizar la plataforma.

Como alternativa, cambie la instancia administrada a la versión 4 del núcleo virtual y vuelva a actualizarla posteriormente. Esto tiene el efecto secundario de actualizar la configuración de DNS.


## <a name="can-a-managed-instance-have-a-static-ip-address"></a>¿Puede una instancia administrada tener una dirección IP estática?

En situaciones poco frecuentes pero necesarias, es posible que tengamos que realizar una migración en línea de una instancia administrada a un nuevo clúster virtual. Si es necesaria, esta migración se debe a los cambios realizados en nuestra pila de tecnología destinados a mejorar la seguridad y confiabilidad del servicio. La migración a un nuevo clúster virtual provoca el cambio de dirección IP que está asignada al nombre de host de la instancia administrada. El servicio de instancia administrada no solicita compatibilidad con direcciones IP estáticas y se reserva el derecho a cambiar la dirección IP sin previo aviso como parte de los ciclos de mantenimiento regular.

Por este motivo se desaconseja confiar en la inmutabilidad de la dirección IP, ya que podría provocar un tiempo de inactividad innecesario.

## <a name="can-i-move-a-managed-instance-or-its-vnet-to-another-resource-group"></a>¿Puedo llevar una instancia administrada o su red virtual a otro grupo de recursos?

No, esto es una limitación actual de la plataforma. Después de crear una instancia administrada, no se admite el traslado de la instancia administrada o la red virtual a otro grupo de recursos o suscripción.

## <a name="can-i-change-the-time-zone-for-an-existing-managed-instance"></a>¿Puedo cambiar la zona horaria de una instancia administrada existente?

La configuración de la zona horaria se puede establecer la primera vez que se aprovisiona una instancia administrada. No se admite el cambio de zona horaria de la instancia administrada existente. Para más información, consulte [limitaciones de la zona horaria](sql-database-managed-instance-timezone.md#limitations).

Las soluciones alternativas incluyen la creación de una nueva instancia administrada con la zona horaria adecuada y la posterior realización de una copia de seguridad y una restauración de forma manual, o lo que recomendamos, realizar una [restauración entre instancias en un momento específico](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/07/cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/).


## <a name="how-do-i-resolve-performance-issues-with-my-managed-instance"></a>Cómo resuelvo los problemas de rendimiento de mi instancia administrada

Para obtener una comparación del rendimiento entre una instancia administrada y SQL Server, un buen punto de partida es leer los [procedimientos recomendados para la comparación de rendimiento entre la Instancia administrada de Azure SQL y SQL Server](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/The-best-practices-for-performance-comparison-between-Azure-SQL/ba-p/683210).

Las cargas de datos suelen ser más lentas en una instancia administrada que en SQL Server debido al modelo de recuperación completa obligatorio y a los [límites](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics) en el rendimiento de la escritura en el registro de transacciones. En ocasiones, esto se puede solucionar mediante la carga de datos transitorios en tempdb, en lugar de en la base de datos de usuario, o bien mediante el uso de un almacén de columnas agrupado o tablas optimizadas para memoria.


## <a name="can-i-restore-my-encrypted-database-to-managed-instance"></a>¿Puedo restaurar mi base de datos cifrada en una instancia administrada?

Sí, no es preciso descifrar la base de datos para poder restaurarla en una instancia administrada. Debe especificar la clave o el certificado utilizados como un protector de clave de cifrado en el sistema de origen a la instancia administrada para poder leer los datos desde el archivo de copia de seguridad cifrado. Existen dos formas posibles de hacerlo:

- *Cargar un protector de certificado en la instancia administrada*. Solo se puede hacer mediante PowerShell. El [script de ejemplo](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-migrate-tde-certificate) describe todo el proceso.
- *Cargar el protector de clave asimétrica a Azure Key Vault (AKV) y apuntar la instancia administrada hacia él*. Este enfoque es similar al caso de uso del cifrado de datos transparente de Bring Your Own Key (BYOK), que también usa la integración de Azure Key Vault para almacenar la clave de cifrado. Si no quiere usar la clave como un protector de clave de cifrado y solo quiere que esté disponible para la instancia administrada y así poder restaurar las bases de datos cifradas, siga las instrucciones para [configurar BYOK TDE](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#manage-transparent-data-encryption-in-the-azure-portal), y no seleccione la casilla *Hacer que la clave seleccionada sea el protector de TDE predeterminado*.

Una vez que el protector de cifrado está disponible para una instancia administrada, puede continuar con el procedimiento de restauración de base de datos estándar.

## <a name="how-can-i-migrate-from-azure-sql-database-single-or-elastic-pool-to-managed-instance"></a>¿Cómo puedo migrar un grupo individual o un grupo de elástico de Azure SQL Database a instancia administrada? 

Una instancia administrada ofrece los mismos niveles de rendimiento por proceso y por tamaño de almacenamiento que otras opciones de implementación de Azure SQL Database. Si desea consolidar los datos en una sola instancia o simplemente necesita una característica admitida exclusivamente en una instancia administrada, puede migrar los datos mediante la funcionalidad de exportación e importación (BACPAC).
