---
title: Comprender cómo voluntario herramienta de migración de las alertas en Azure Monitor funciona
description: Comprender el funcionamiento de la herramienta de migración de alerta y solución de problemas si surgen problemas.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: a45a0cff606bc854924d5da0841b26e1cb9031bb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60347607"
---
# <a name="understand-how-the-migration-tool-works"></a>Comprender el funcionamiento de la herramienta de migración

Como [anunciaron](monitoring-classic-retirement.md), las alertas clásicas en Azure Monitor se retirará en julio de 2019. La herramienta de migración para desencadenar voluntariamente la migración está disponible en Azure portal y está implementando en los clientes que usan reglas de alerta clásicas.

Este artículo le guiará a través de cómo funciona la herramienta de migración voluntaria. También se describe la corrección de algunos problemas comunes.

## <a name="which-classic-alert-rules-can-be-migrated"></a>¿Se pueden migrar las reglas de alerta clásicas?

Aunque casi todas las reglas de alerta clásicas se pueden migrar mediante la herramienta, hay algunas excepciones. No se migrarán las siguientes reglas de alerta mediante la herramienta (o durante la migración automática de 2019 julio)

- Reglas de alertas clásicas en las métricas de invitado de máquina virtual (Windows y Linux). [Consulte instrucciones sobre cómo volver a crear estas reglas de alerta en nuevas alertas de métrica](#guest-metrics-on-virtual-machines)
- Reglas de alertas clásicas en las métricas de almacenamiento clásico. [Consulte instrucciones sobre la supervisión de las cuentas de almacenamiento clásico](https://azure.microsoft.com/blog/modernize-alerting-using-arm-storage-accounts/)
- Reglas de alertas clásicas en algunas métricas de la cuenta de almacenamiento. [Detalles a continuación](#storage-account-metrics)

Si su suscripción tiene ninguna regla de clásica, se migrará el resto de las reglas, pero deben migrarse manualmente estas reglas. Como no podemos proporcionar una migración automática, cualquier tales existentes alertas de métricas clásicas seguirá funcionando a junio de 2020 proporcionar tiempo para migrar a nuevas alertas. Sin embargo, no hay nuevas alertas clásicas pueden crearse post junio de 2019.

### <a name="guest-metrics-on-virtual-machines"></a>Métricas de invitado en máquinas virtuales

Para poder crear nuevas alertas de métricas en las métricas de invitado, las métricas de invitado deben enviarse al almacén de Azure Monitor las métricas personalizadas. Siga las instrucciones siguientes para habilitar el receptor de Azure Monitor en la configuración de diagnóstico.

- [Habilitar las métricas de invitado para máquinas virtuales de Windows](collect-custom-metrics-guestos-resource-manager-vm.md)
- [Habilitar las métricas de invitado para máquinas virtuales Linux](https://docs.microsoft.com/azure/azure-monitor/platform/collect-custom-metrics-linux-telegraf)

Una vez que se realizan los pasos anteriores, se podrán crear nuevas alertas de métrica las métricas de invitado. Una vez que se ha vuelto a crear nuevas alertas de métrica, se pueden eliminar las alertas clásicas.

### <a name="storage-account-metrics"></a>Métricas de la cuenta de almacenamiento

Todas las alertas clásicas en cuentas de almacenamiento se pueden migrar, excepto las alertas en las siguientes métricas:

- PercentAuthorizationError
- PercentClientOtherError
- PercentNetworkError
- PercentServerOtherError
- PercentSuccess
- PercentThrottlingError
- PercentTimeoutError
- AnonymousThrottlingError
- SASThrottlingError

Las reglas de alertas clásicas en métricas de porcentaje debe migrarse según [la asignación entre las métricas de almacenamiento antiguos y nuevos](https://docs.microsoft.com/azure/storage/common/storage-metrics-migration#metrics-mapping-between-old-metrics-and-new-metrics). Los umbrales deberá modificarse apropiadamente como la nueva métrica disponible es absoluta.

Las reglas de alerta clásicas error AnonymousThrottlingError y error SASThrottlingError deberá dividirse en dos nuevas alertas que haya no es ninguna métrica combinada que proporciona la misma funcionalidad. Los umbrales deberá adaptarse adecuadamente.

## <a name="roll-out-phases"></a>Fases de puesta en servicio

La herramienta de migración se está implementando en fases a los clientes que usan reglas de alerta clásicas. **Los propietarios de suscripciones** recibirá un correo electrónico cuando esté lista para la migración mediante la herramienta de la suscripción.

> [!NOTE]
> Como la herramienta se está implantando en fases, en las primeras fases, es posible que vea que la mayoría de las suscripciones no está preparada para la migración.

Actualmente un **subconjunto** de suscripciones, lo que **sólo** tiene reglas de alerta clásicas en el siguiente recurso de tipos se marcan como listos para la migración. En las próximas fases se agregará compatibilidad con más tipos de recursos.

- Microsoft.apimanagement/service
- Microsoft.batch/batchaccounts
- Microsoft.cache/redis
- Microsoft.datafactory/datafactories
- Microsoft.dbformysql/servers
- Microsoft.dbforpostgresql/servers
- Microsoft.eventhub/namespaces
- Microsoft.logic/workflows
- Microsoft.network/applicationgateways
- Microsoft.network/dnszones
- Microsoft.network/expressroutecircuits
- Microsoft.network/loadbalancers
- Microsoft.network/networkwatchers/connectionmonitors
- Microsoft.network/publicipaddresses
- Microsoft.network/trafficmanagerprofiles
- Microsoft.network/virtualnetworkgateways
- Microsoft.search/searchservices
- Microsoft.servicebus/namespaces
- Microsoft.streamanalytics/streamingjobs
- Microsoft.timeseriesinsights/environments
- Microsoft.web/hostingenvironments/workerpools
- Microsoft.web/serverfarms
- Microsoft.web/sites

## <a name="who-can-trigger-the-migration"></a>¿Quién puede desencadenar la migración?

Cualquier usuario que tenga el rol integrado de **colaborador de supervisión** en la suscripción de nivel podrá desencadenar la migración. Los usuarios con un rol personalizado con los permisos siguientes también pueden desencadenar la migración:

- */read
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*

## <a name="common-issues-and-remediations"></a>Problemas comunes y correcciones

Una vez que [desencadenar la migración](alerts-using-migration-tool.md), usaremos las direcciones de correo electrónico proporcionada para recibir una notificación de finalización de la migración o si hay una acción necesaria. La siguiente sección describe algunos problemas comunes y cómo solucionarlos

### <a name="validation-failed"></a>No se pudo realizar la validación

Debido a algunos cambios recientes en las reglas de alertas clásicas en su suscripción, no se puede migrar la suscripción. Se trata de un problema temporal. Puede reiniciar la migración una vez que el estado de migración, se mueve hacia atrás **preparados para la migración** en unos días.

### <a name="policyscope-lock-preventing-us-from-migrating-your-rules"></a>Impide la migración de las reglas de bloqueo de directiva y ámbito

Como parte de la migración, se crearán nuevas alertas de métricas y los nuevos grupos de acciones y reglas de alerta clásicas se eliminará (una vez que se crean nuevas reglas). Sin embargo, hay una directiva o el ámbito de bloqueo impide la creación de recursos. Según el bloqueo de la directiva o ámbito, no se podrían migrar algunas o todas las reglas. Puede resolver este problema quitando temporalmente la directiva/bloqueo de ámbito y desencadenar la migración de nuevo.

## <a name="next-steps"></a>Pasos siguientes

- [Cómo usar la herramienta de migración](alerts-using-migration-tool.md)
- [Preparación para la migración](alerts-prepare-migration.md)
