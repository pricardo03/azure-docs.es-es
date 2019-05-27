---
title: Migrar existente espacios de nombres estándar de Azure Service Bus para el nivel premium | Microsoft Docs
description: Guía para permitir la migración de Azure Service Bus estándar espacios de nombres existentes a premium
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
ms.date: 05/18/2019
ms.author: aschhab
ms.openlocfilehash: 65c207b4d03e7d156c8c871a3642601fd0489ead
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991416"
---
# <a name="migrate-existing-azure-service-bus-standard-namespaces-to-the-premium-tier"></a>Migrar existente espacios de nombres estándar de Azure Service Bus para el nivel premium
Anteriormente, Azure Service Bus ofrece los espacios de nombres solo en el nivel estándar. Espacios de nombres son configuraciones de varios inquilinos que se optimizan para entornos de desarrollo y un rendimiento bajo. El nivel premium ofrece recursos dedicados por espacio de nombres para una latencia predecible y aumento del rendimiento a un precio fijo. El nivel premium está optimizado para entornos de producción que requieren características empresariales adicionales y de alto rendimiento.

En este artículo se describe cómo migrar espacios de nombres de nivel estándar existente al nivel premium.  

>[!WARNING]
> Migración está diseñada para que espacios de nombres estándar de Service Bus para actualizarse al nivel premium. La herramienta de migración no admite la degradación.

Algunos de los puntos a tener en cuenta: 
- Esta migración está pensada para que se producen en su lugar, lo que significa existentes aplicaciones del remitente y receptor **no requiere ningún cambio en código o configuración**. La cadena de conexión existente apuntan automáticamente al nuevo espacio de nombres premium.
- El **premium** debe tener el espacio de nombres **ninguna entidad** en él para la migración se realice correctamente. 
- Todos los **entidades** en el espacio de nombres estándar son **copian** al espacio de nombres premium durante el proceso de migración. 
- Admite la migración **1.000 entidades por unidad de mensajería** en el nivel premium. Para identificar el número de unidades de mensajería que necesita, comience con el número de entidades que existen en el espacio de nombres estándar actual. 
- No se puede migrar directamente desde **básico** a **nivel premier**, pero puede hacerlo indirectamente mediante la migración de básico a estándar primero y, a continuación, en el estándar a premium en el paso siguiente.

## <a name="migration-steps"></a>Pasos de migración
Algunas condiciones asociadas con el proceso de migración. Familiarícese con los pasos siguientes para reducir la posibilidad de errores. Estos pasos describen el proceso de migración y se muestran los detalles de paso a paso en las secciones siguientes.

1. Crear un nuevo espacio de nombres premium.
1. Empareje los espacios de nombres estándar y premium entre sí.
1. Entidades de sincronización (por encima de copia) del estándar para el espacio de nombres premium.
1. Confirmar la migración.
1. Purgar las entidades en el espacio de nombres estándar con el nombre después de la migración del espacio de nombres.
1. Eliminar el espacio de nombres estándar.

>[!IMPORTANT]
> Después de la migración se ha confirmado, tener acceso el antiguo espacio de nombres estándar y purgar las colas y suscripciones. Después de que los mensajes que se hayan descargado, se enviará al nuevo espacio de nombres premium para ser procesados por las aplicaciones de receptor. Después de las colas y suscripciones que se hayan descargado, se recomienda que elimine el antiguo espacio de nombres estándar.

### <a name="migrate-by-using-the-azure-cli-or-powershell"></a>Migrar mediante la CLI de Azure o PowerShell

Para migrar el espacio de nombres de Service Bus estándar a premium mediante la CLI de Azure o la herramienta de PowerShell, siga estos pasos.

1. Crear un nuevo espacio de nombres premium de Service Bus. Puede hacer referencia a la [plantillas Azure Resource Manager](service-bus-resource-manager-namespace.md) o [usar Azure portal](service-bus-create-namespace-portal.md). No olvide seleccionar **premium** para el **serviceBusSku** parámetro.

1. Establezca las siguientes variables de entorno para simplificar los comandos de migración.
   ```azurecli
   resourceGroup = <resource group for the standard namespace>
   standardNamespace = <standard namespace to migrate>
   premiumNamespaceArmId = <Azure Resource Manager ID of the premium namespace to migrate to>
   postMigrationDnsName = <post migration DNS name entry to access the standard namespace>
   ```

    >[!IMPORTANT]
    > Después de la migración alias/nombre (post_migration_dns_name) se usará para tener acceso a la migración de entrada de espacio de nombres estándar antiguo. Use esta opción para purgar las colas y las suscripciones y, a continuación, elimine el espacio de nombres.

1. Empareje los espacios de nombres estándar y premium y comience la sincronización con el comando siguiente:

    ```azurecli
    az servicebus migration start --resource-group $resourceGroup --name $standardNamespace --target-namespace $premiumNamespaceArmId --post-migration-name $postMigrationDnsName
    ```


1. Comprobar el estado de la migración mediante el comando siguiente:
    ```azurecli
    az servicebus migration show --resource-group $resourceGroup --name $standardNamespace
    ```

    La migración se considera completada cuando vea los siguientes valores:
    * MigrationState = "Activo"
    * pendingReplicationsOperationsCount = 0
    * provisioningState = "Succeeded"

    Este comando también muestra la configuración de la migración. Compruebe que los valores se establecen correctamente. Compruebe también el espacio de nombres premium en el portal para asegurarse de todas las colas y temas se han creado y que coinciden con lo que ya existían en el espacio de nombres estándar.

1. Confirmar la migración ejecutando el siguiente comando completo:
   ```azurecli
   az servicebus migration complete --resource-group $resourceGroup --name $standardNamespace
   ```

### <a name="migrate-by-using-the-azure-portal"></a>Migrar mediante el portal de Azure

Migración mediante el portal de Azure tiene el mismo flujo lógico que la migración mediante el uso de los comandos. Siga estos pasos para migrar mediante el portal de Azure.

1. En el **navegación** menú en el panel izquierdo, seleccione **migrar a premium**. Haga clic en el **comenzar** botón para continuar a la página siguiente.
    ![Página de aterrizaje de migración][]

1. Completa **instalación**.
   ![Espacio de nombres del programa de instalación][]
   1. Cree y asigne el espacio de nombres para migrar el espacio de nombres estándar existente a premium.
        ![Configuración del espacio de nombres: crear el espacio de nombres premium][]
   1. Elija un **nombre después de la migración**. Usará este nombre para tener acceso al espacio de nombres estándar, una vez completada la migración.
        ![Configuración del espacio de nombres: elegir el nombre de la migración de post][]
   1. Seleccione **'Next'** para continuar.
1. Entidades de sincronización entre los espacios de nombres estándar y premium.
    ![Configurar el inicio del espacio de nombres - sincronización de entidades:][]

   1. Seleccione **iniciar sincronización** para empezar a sincronizar las entidades.
   1. Seleccione **Sí** en el cuadro de diálogo para confirmar e iniciar la sincronización.
   1. Espere hasta que finalice la sincronización. El estado está disponible en la barra de estado.
        ![Espacio de nombres - sincronización de entidades: progreso de la instalación][]
        >[!IMPORTANT]
        > Si necesita anular la migración por cualquier motivo, revise el flujo de anulación en la sección de preguntas más frecuentes de este documento.
   1. Una vez completada la sincronización, seleccione **siguiente** en la parte inferior de la página.

1. Revise los cambios en la página de resumen. Seleccione **completar migración** para cambiar los espacios de nombres y para completar la migración.
    ![Cambiar el espacio de nombres - menú conmutador][] aparece la página de confirmación una vez completada la migración.
    ![Espacio de nombres de conmutador: correcto][]

## <a name="faqs"></a>Preguntas más frecuentes

### <a name="what-happens-when-the-migration-is-committed"></a>¿Qué ocurre cuando se confirma la migración?

Una vez confirmada la migración, la cadena de conexión que señala al espacio de nombres estándar apuntará al espacio de nombres premium.

Las aplicaciones de remitente y receptor se desconecte el Namespace estándar y volver a conectar al espacio de nombres premium.

### <a name="what-do-i-do-after-the-standard-to-premium-migration-is-complete"></a>¿Qué hacer una vez completada la estándar para la migración premium?

El estándar para la migración premium garantiza que los metadatos de entidad, como temas, suscripciones y los filtros se copian desde el espacio de nombres estándar en el espacio de nombres premium. Los datos del mensaje que se confirmó en el espacio de nombres estándar no se copian desde el espacio de nombres estándar en el espacio de nombres premium.

El espacio de nombres estándar puede tener algunos mensajes que se han enviado y confirmado mientras la migración estaba en curso. Purgar manualmente estos mensajes desde el Namespace estándar y los envían manualmente a la premium Namespace. Para purgar manualmente los mensajes, utilice una aplicación de consola o un script que purga las entidades del espacio de nombres estándar con el nombre DNS de la migración de entrada que especificó en los comandos de migración. Enviar estos mensajes para el espacio de nombres premium para que se pueden procesar los receptores.

Después de que los mensajes que se hayan descargado, elimine el espacio de nombres estándar.

>[!IMPORTANT]
> Después de que los mensajes del espacio de nombres estándar que se hayan descargado, elimine el espacio de nombres estándar. Esto es importante porque la cadena de conexión que inicialmente hacer referencia al espacio de nombres estándar ahora hace referencia al espacio de nombres premium. Ya no necesite el Namespace estándar. Eliminando el espacio de nombres estándar que se migró ayuda a reducir la confusión de versiones posterior.

### <a name="how-much-downtime-do-i-expect"></a>¿Cuánto tiempo de inactividad se debe esperar?
El proceso de migración está pensado para reducir el tiempo de inactividad previsto para las aplicaciones. Se reduce el tiempo de inactividad mediante el uso de la cadena de conexión que usan las aplicaciones del remitente y receptor para que apunte al nuevo espacio de nombres premium.

El tiempo de inactividad que se ha producido por la aplicación se limita a la hora de actualizar la entrada DNS para que apunte al espacio de nombres premium. Tiempo de inactividad es de aproximadamente 5 minutos.

### <a name="do-i-have-to-make-any-configuration-changes-while-doing-the-migration"></a>¿Es necesario realizar cambios de configuración mientras se realiza la migración?
No, no hay ningún cambio de código o la configuración necesaria para realizar la migración. La cadena de conexión que las aplicaciones de remitente y receptor que se usan para tener acceso a la Namespace estándar se asigna automáticamente para actuar como un alias para el espacio de nombres premium.

### <a name="what-happens-when-i-abort-the-migration"></a>¿Qué ocurre cuando anula la migración?
La migración puede anularse mediante el uso de la `Abort` de comandos o mediante el portal de Azure. 

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az servicebus migration abort --resource-group $resourceGroup --name $standardNamespace
```

#### <a name="azure-portal"></a>Azure Portal

![Anular el flujo: anular sincronización][]
![anular el flujo: anular completa][]

Cuando se anula el proceso de migración, se anula el proceso de copiar las entidades (temas, suscripciones y filtros) del estándar en el espacio de nombres premium y el emparejamiento se interrumpe.

La cadena de conexión no se actualiza para que apunte al espacio de nombres premium. Las aplicaciones existentes seguirán funcionando como lo hacían antes de iniciar la migración.

Sin embargo, no elimine las entidades en el espacio de nombres premium o eliminar el espacio de nombres premium. Si decide no continuar con la migración, elimine manualmente las entidades.

>[!IMPORTANT]
> Si decide anular la migración, elimine la prima Namespace que había aprovisionado para la migración para que no se le cobrará por los recursos.

#### <a name="i-dont-want-to-have-to-drain-the-messages-what-do-i-do"></a>No se desea tener que purgar los mensajes. ¿Qué puedo hacer?

Puede que haya mensajes que se envían por las aplicaciones del remitente y confirmados para el almacenamiento en el estándar Namespace mientras lleva a cabo la migración y justo antes de que se confirma la migración.

Durante la migración, el mensaje real/carga de datos no se copiarán del estándar en el espacio de nombres premium. Los mensajes que se purguen manualmente y, a continuación, se envían al espacio de nombres premium.

Sin embargo, si puede migrar durante una ventana de mantenimiento o mantenimiento planeado y no desea purgar manualmente y enviar los mensajes, siga estos pasos:

1. Detenga las aplicaciones del remitente. Las aplicaciones de receptor procesará los mensajes que están actualmente en el espacio de nombres estándar y se vaciará la cola.
1. Después de las colas y suscripciones en el estándar Namespace están vacías, siga el procedimiento descrito anteriormente para ejecutar la migración desde el estándar para el espacio de nombres premium.
1. Una vez completada la migración, puede reiniciar las aplicaciones del remitente.
1. Los remitentes y receptores ahora se conectarán automáticamente con el espacio de nombres premium.

    >[!NOTE]
    > No es necesario detener las aplicaciones de receptor para la migración.
    >
    > Una vez completada la migración, las aplicaciones de receptor se desconecte del espacio de nombres estándar y conectarse automáticamente al espacio de nombres premium.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre la [diferencias entre la mensajería estándar y premium](./service-bus-premium-messaging.md).
* Obtenga información sobre la [aspectos de recuperación ante desastres y alta disponibilidad para el Bus de servicio premium](service-bus-outages-disasters.md#protecting-against-outages-and-disasters---service-bus-premium).

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
