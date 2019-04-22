---
title: Migrar Azure Service Bus estándar espacios de nombres existentes al nivel Premium | Microsoft Docs
description: Guía para permitir la migración de Azure Service Bus estándar espacios de nombres existentes a Premium
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: darosa
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/18/2019
ms.author: aschhab
ms.openlocfilehash: 7b153c36e10f1d4e2be2a0cf42f998c31cb6473a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58896587"
---
# <a name="migrate-existing-azure-service-bus-standard-namespaces-to-premium-tier"></a>Migrar Azure Service Bus estándar espacios de nombres existentes al nivel Premium

Anteriormente, Azure Service Bus ofrece los espacios de nombres solo en el nivel estándar. Se trataba de configuraciones de varios inquilinos que se han optimizado para entornos de desarrollo y un rendimiento bajo.

En el pasado reciente, Azure Service Bus ha ampliado para ofrecer el nivel Premium que ofrece recursos dedicados por espacio de nombres para una latencia predecible y aumento del rendimiento a un precio fijo que está optimizado para entornos de producción y de alto rendimiento requerir que las características empresariales adicionales.

El siguiente herramientas permite existente de espacios de nombres estándar para migrar al nivel Premium.

>[!WARNING]
> Migración está prevista para el espacio de nombres de Service Bus estándar sea ***actualizado*** al nivel Premium.
>
> Las herramientas de migración ***no*** admite la degradación.
>[!NOTE]
> Esta migración está pensada para ocurrir ***en su lugar***.
>
> Esto implica que aplicaciones de receptor y remitente existente no requieren ningún cambio de código o configuración.
>
> La cadena de conexión existente apuntan automáticamente al nuevo espacio de nombres premium.
>
> Además, todas las entidades en el espacio de nombres estándar son **copian** en el espacio de nombres Premium durante el proceso de migración.
>
>
> Admitimos ***1000 entidades por unidad de mensajería*** en versión preliminar, por lo que para identificar el número de unidades de mensajería necesita, empieza con el número de entidades que existen en el espacio de nombres estándar actual.

## <a name="migration-steps"></a>Pasos de migración

>[!IMPORTANT]
> Existen algunas advertencias asociadas con el proceso de migración. Le rogamos que se familiarice completamente con los pasos necesarios para reducir las posibilidades de errores.

El proceso de migración paso a paso concreto se detalla en las guías siguientes.

Los pasos lógicos necesarios son:

1. Crear un nuevo espacio de nombres Premium.
2. Emparejar el espacio de nombres estándar y Premium entre sí.
3. Entidades de sincronización (por encima de copia) del estándar al espacio de nombres Premium
4. Confirmar la migración
5. Purgar las entidades en el espacio de nombres estándar con el nombre después de la migración del espacio de nombres
6. Eliminar el espacio de nombres estándar

>[!NOTE]
> Una vez que se ha confirmado la migración, es muy importante tener acceso el antiguo espacio de nombres estándar y purgar las colas y suscripciones.
>
> Una vez que los mensajes que se hayan descargado se enviará al nuevo espacio de nombres premium para ser procesados por las aplicaciones de receptor.
>
> Una vez que las colas y suscripciones que se hayan descargado, se recomienda eliminar el antiguo espacio de nombres estándar. ¡No necesidad lo!

### <a name="migrate-using-azure-cli-or-powershell"></a>Migrar con PowerShell o la CLI de Azure

Para migrar un espacio de nombres de Service Bus estándar a Premium con la herramienta CLI de Azure o PowerShell, consulte la siguiente guía.

1. Crear un nuevo espacio de nombres Premium de Service Bus. Puede hacer referencia a la [plantillas Azure Resource Manager](service-bus-resource-manager-namespace.md) o [usar Azure portal](service-bus-create-namespace-portal.md). No olvide seleccionar "Premium" para el **serviceBusSku** parámetro.

2. Establecer las siguientes variables de entorno para simplificar los comandos de migración.
   ```
   resourceGroup = <resource group for the standard namespace>
   standardNamespace = <standard namespace to migrate>
   premiumNamespaceArmId = <Azure Resource Manager ID of the Premium namespace to migrate to>
   postMigrationDnsName = <post migration DNS name entry to access the Standard namespace>
   ```

    >[!IMPORTANT]
    > El nombre después de la migración (post_migration_dns_name) se usará para tener acceso a la migración de entrada de espacio de nombres estándar antiguo. Se debe usar para purgar las colas y las suscripciones y, a continuación, elimine el espacio de nombres.

3. **Par** los espacios de nombres estándar y Premium y **iniciar sincronización** mediante el siguiente comando:

    ```
    az servicebus migration start --resource-group $resourceGroup --name $standardNamespace --target-namespace $premiumNamespaceArmId --post-migration-name $postMigrationDnsName
    ```


4. Compruebe el estado de la migración mediante el siguiente comando:
    ```
    az servicebus migration show --resource-group $resourceGroup --name $standardNamespace
    ```

    La migración se considera completada cuando
    * MigrationState = "Activo"
    * pendingReplicationsOperationsCount = 0
    * provisioningState = "Succeeded"

    Este comando también muestra la configuración de la migración. Por favor, compruebe lo siguiente para asegurarse de que los valores se establecen como anteriores declara.

    Además, compruebe también el espacio de nombres Premium en el portal para asegurarse de que se han creado todas las colas y temas, y que coinciden con lo que existía en el espacio de nombres estándar.

5. Confirmar la migración ejecutando el siguiente comando completo
   ```
   az servicebus migration complete --resource-group $resourceGroup --name $standardNamespace
   ```

### <a name="migrate-using-azure-portal"></a>Migrar mediante Azure portal

Migración a través del portal de Azure tiene el mismo flujo lógico que la migración mediante los comandos. Consulte la siguiente guía para el proceso paso a paso para migrar mediante el portal.

1. Elegir el **'Migrar a Premium'** la opción de menú en el menú de navegación en el panel izquierdo. Haga clic en el **"Comenzar"** botón para continuar con la página siguiente.
    ![Página de aterrizaje de migración][]

2. Completa **instalación**.
   ![Espacio de nombres del programa de instalación][]
   1. Cree y asigne el espacio de nombres para migrar el espacio de nombres estándar existente a Premium.
        ![Configuración del espacio de nombres: crear el espacio de nombres premium][]
   2. Elegir el **'Nombre después de la migración'** para tener acceso al espacio de nombres estándar, una vez completada la migración.
        ![Configuración del espacio de nombres: elegir el nombre de la migración de post][]
   3. Haga clic en **'Next'** para continuar.
3. **Sincronización** entidades entre el espacio de nombres estándar y Premium.
    ![Configurar el inicio del espacio de nombres - sincronización de entidades:][]

   1. Haga clic en **'Iniciar sincronización'** para empezar a sincronizar las entidades.
   2. Haga clic en **'Sí'** en la ventana emergente de confirmación para iniciar la sincronización.
   3. Espere hasta que el **sincronización** completada. El estado está disponible en la barra de estado.
        ![Espacio de nombres - sincronización de entidades: progreso de la instalación][]
        >[!IMPORTANT]
        > Si necesita **anular** por cualquier motivo, revise el flujo de anulación en la sección de preguntas más frecuentes de este documento.
   4. Una vez completada la sincronización, haga clic en el **'Next'** situado en la parte inferior de la página.

4. Revise los cambios en la página de resumen.
    ![Cambiar el espacio de nombres - menú conmutador][]

5. Haga clic en **'Completar la migración'** para cambiar los espacios de nombres y completar la migración.
    ![Espacio de nombres de conmutador: correcto][]

## <a name="faqs"></a>Preguntas más frecuentes

### <a name="what-happens-when-the-migration-is-committed"></a>¿Qué ocurre cuando se confirma la migración?

Una vez confirmada la migración, la cadena de conexión que señala al espacio de nombres estándar apuntará al espacio de nombres Premium.

Las aplicaciones de remitente y receptor se desconecte el Namespace estándar y volver a conectar al espacio de nombres Premium.

### <a name="what-do-i-do-after-the-standard-to-premium-migration-is-complete"></a>¿Qué hacer una vez completada la estándar para la migración Premium?

El estándar para la migración Premium garantiza que los metadatos de entidad (temas, suscripciones, los filtros, etc.) se copian a través del estándar al espacio de nombres Premium. No se copian los datos del mensaje que se confirmó en el espacio de nombres estándar a través del estándar al espacio de nombres Premium.

Debido a esto, el espacio de nombres estándar puede tener algunos mensajes que se han enviado y confirmado mientras la migración estaba en curso. Estos mensajes deben agotar del Namespace estándar manualmente y se envían a través de la Namespace Premium manualmente.

Para ello, le ***debe*** usar una aplicación de consola o script que purga las entidades del espacio de nombres estándar mediante el **nombre DNS de la migración de Post** que especificado en los comandos de migración y, a continuación, enviar estos mensajes en El Namespace Premium, por lo que se puede procesar los receptores.

Una vez que los mensajes que se hayan descargado, vaya a eliminar el espacio de nombres estándar.

>[!IMPORTANT]
> Tenga en cuenta que una vez que los mensajes del espacio de nombres estándar que se hayan descargado, se **debe** eliminar el espacio de nombres estándar.
>
> Esto es importante porque la cadena de conexión que inicialmente hacer referencia al espacio de nombres estándar ahora hace referencia al espacio de nombres Premium. No ser la necesidad de este estándar Namespace ya.
>
> Eliminando el espacio de nombres estándar que se han migrado ayuda a reduce la confusión en una fecha posterior. 

### <a name="how-much-downtime-do-i-expect"></a>¿Cuánto tiempo de inactividad se debe esperar?
El proceso de migración se ha descrito anteriormente está pensado para reducir el tiempo de inactividad previsto para las aplicaciones. Esto se hace mediante el uso de la cadena de conexión que usan las aplicaciones del remitente y receptor para que apunte al nuevo espacio de nombres Premium.

El tiempo de inactividad experimentados por la aplicación se limita a la cantidad de tiempo necesario para actualizar la entrada DNS para que apunte al espacio de nombres Premium.

Esto puede suponer que ***aproximadamente 5 minutos***.

### <a name="do-i-have-to-make-any-configuration-changes-while-performing-the-migration"></a>¿Es necesario realizar cambios de configuración mientras se realiza la migración?
No, no hay ningún cambio de código y configuración necesario para realizar esta migración. La cadena de conexión que las aplicaciones de remitente y receptor que se usan para tener acceso a la Namespace estándar se asigna automáticamente para actuar como un **alias** para el Namespace Premium.

### <a name="what-happens-when-i-abort-the-migration"></a>¿Qué ocurre cuando anula la migración?
Puede anular la migración mediante el comando 'Anular' o a través del portal de Azure. 

#### <a name="azure-cli-or-powershell"></a>CLI de Azure o PowerShell

    az servicebus migration abort --resource-group $resourceGroup --name $standardNamespace

#### <a name="azure-portal"></a>Azure Portal

![Anular el flujo: anular sincronización][]
![anular el flujo: anular completa][]

Cuando se anula el proceso de migración, en realidad se anula el proceso de copia a través de las entidades (temas, suscripciones y filtros) del estándar al espacio de nombres Premium y el emparejamiento se interrumpe.

La cadena de conexión **no** actualiza para que apunte al espacio de nombres Premium. Las aplicaciones existentes seguirán funcionando como lo hacían antes de iniciar la migración.

Sin embargo, lo **no** eliminar las entidades en el espacio de nombres Premium o eliminar el espacio de nombres Premium. Esto debe realizarse manualmente si decidió no continuar con la migración después de todo.

>[!IMPORTANT]
> Si decide anular la migración, elimine el Namespace Premium que había aprovisionado para la migración, por lo que no se le cobrará por los recursos.

#### <a name="i-dont-want-to-have-to-drain-the-messages-what-do-i-do"></a>No se desea tener que purgar los mensajes. ¿Qué puedo hacer?

Puede que haya mensajes que se envían por las aplicaciones del remitente y confirmados para el almacenamiento en el estándar Namespace mientras lleva a cabo la migración, y justo antes de la migración se confirma.

Dado que durante la migración, el mensaje real/carga de datos no se copia a través de estándar a Premium, estos tienen que se purguen manualmente y, a continuación, se envían al espacio de nombres Premium.

Sin embargo, si puede migrar durante una ventana de mantenimiento o mantenimiento planeado y no desea purgar manualmente y enviar los mensajes, siga los siguientes pasos:

1. Detenga las aplicaciones del remitente, y permitir que los receptores procesar los mensajes que están actualmente en el espacio de nombres estándar y vaciar la cola.
2. Una vez que las colas y suscripciones en el estándar Namespace están vacías, siga el procedimiento descrito anteriormente para realizar la migración de estándar a espacio de nombres Premium.
3. Una vez completada la migración, puede reiniciar las aplicaciones del remitente.
4. Los remitentes y receptores ahora se conectarán automáticamente con el espacio de nombres Premium.

    >[!NOTE]
    > El receptor no necesita detener la migración.
    >
    > Una vez completada la migración, los receptores se desconecte del espacio de nombres estándar y conectarse automáticamente al espacio de nombres Premium.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre la [diferencias entre estándar y Premium de mensajería](./service-bus-premium-messaging.md)
* Obtenga información sobre los aspectos de recuperación geográfica Diaster y alta disponibilidad para Service Bus Premium [aquí](service-bus-outages-disasters.md#protecting-against-outages-and-disasters---service-bus-premium)

[Página de aterrizaje de migración]: ./media/service-bus-standard-premium-migration/1.png
[Espacio de nombres del programa de instalación]: ./media/service-bus-standard-premium-migration/2.png
[Configuración del espacio de nombres: crear el espacio de nombres premium]: ./media/service-bus-standard-premium-migration/3.png
[Configuración del espacio de nombres: elegir el nombre de la migración de post]: ./media/service-bus-standard-premium-migration/4.png
[Configurar el inicio del espacio de nombres - sincronización de entidades:]: ./media/service-bus-standard-premium-migration/5.png
[Espacio de nombres - sincronización de entidades: progreso de la instalación]: ./media/service-bus-standard-premium-migration/8.png
[Cambiar el espacio de nombres - menú conmutador]: ./media/service-bus-standard-premium-migration/9.png
[Espacio de nombres de conmutador: correcto]: ./media/service-bus-standard-premium-migration/12.png

[Anular el flujo: anular la sincronización]: ./media/service-bus-standard-premium-migration/abort1.png
[Anular el flujo: anular completa]: ./media/service-bus-standard-premium-migration/abort3.png
