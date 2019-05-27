---
title: Comprender el funcionamiento de la herramienta de migración voluntaria para las alertas de Azure Monitor
description: Comprender el funcionamiento de la herramienta de migración de las alertas y solución de problemas.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: b5a13254fc9dfd58db83a1bc8b9dd071cfbbdab2
ms.sourcegitcommit: db3fe303b251c92e94072b160e546cec15361c2c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66015585"
---
# <a name="understand-how-the-migration-tool-works"></a>Comprender el funcionamiento de la herramienta de migración

Como [anunciaron](monitoring-classic-retirement.md), las alertas clásicas en Azure Monitor se retirará en septiembre de 2019 (era originalmente de 2019 de julio). Una herramienta de migración está disponible en el portal de Azure a los clientes que utilizan las reglas de alerta clásicas y que desean desencadenar la migración.

En este artículo se explica cómo funciona la herramienta de migración voluntaria. También se describen soluciones para algunos problemas comunes.

> [!NOTE]
> Debido a un retraso en la puesta en servicio de herramienta de migración, ha sido la fecha de retirada para la migración de las alertas clásicas [extiende hasta el 31 de agosto de 2019](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/) desde la fecha anunciada originalmente del 30 de junio de 2019.

## <a name="which-classic-alert-rules-can-be-migrated"></a>¿Se pueden migrar las reglas de alerta clásicas?

Aunque la herramienta pueda migrar casi todas clásicas reglas de alerta, hay algunas excepciones. No se migrarán las siguientes reglas de alerta mediante la herramienta (o durante la migración automática en septiembre de 2019):

- Reglas de alertas clásicas en las métricas de invitado de máquina virtual (Windows y Linux). Consulte la [instrucciones para volver a crear estas reglas de alerta en alertas de métricas nuevo](#guest-metrics-on-virtual-machines) más adelante en este artículo.
- Reglas de alertas clásicas en las métricas de almacenamiento clásico. Consulte la [orientación para la supervisión de las cuentas de almacenamiento clásico](https://azure.microsoft.com/blog/modernize-alerting-using-arm-storage-accounts/).
- Reglas de alertas clásicas en algunas métricas de la cuenta de almacenamiento. Consulte [detalles](#storage-account-metrics) más adelante en este artículo.

Si su suscripción tiene ninguna regla de clásica, deberá migrarlos manualmente. Porque no podemos proporcionar una migración automática, las alertas de métricas clásicas existentes de estos tipos seguirá funcionando hasta junio de 2020. Esta extensión ofrece le dará tiempo para migrar a nuevas alertas. Sin embargo, no hay nuevas alertas clásicas pueden crearse después de agosto de 2019.

### <a name="guest-metrics-on-virtual-machines"></a>Métricas de invitado en máquinas virtuales

Para poder crear nuevas alertas de métricas en las métricas de invitado, se deben enviar las métricas de invitado en el almacén de métricas personalizadas de Azure Monitor. Siga estas instrucciones para habilitar el receptor de Azure Monitor en la configuración de diagnóstico:

- [Habilitar las métricas de invitado para máquinas virtuales de Windows](collect-custom-metrics-guestos-resource-manager-vm.md)
- [Habilitar las métricas de invitado para máquinas virtuales Linux](collect-custom-metrics-linux-telegraf.md)

Una vez realizados estos pasos, puede crear nuevas alertas de métricas en las métricas de invitado. Y, después de crear nuevas alertas de métricas, puede eliminar las alertas clásicas.

### <a name="storage-account-metrics"></a>Métricas de la cuenta de almacenamiento

Todas las alertas clásicas en cuentas de almacenamiento se pueden migrar, excepto las alertas de estas métricas:

- PercentAuthorizationError
- PercentClientOtherError
- PercentNetworkError
- PercentServerOtherError
- PercentSuccess
- PercentThrottlingError
- PercentTimeoutError
- AnonymousThrottlingError
- SASThrottlingError

Alerta clásica se deben migrar las reglas de métricas de porcentaje en función de [la asignación entre las métricas de almacenamiento antiguos y nuevos](https://docs.microsoft.com/azure/storage/common/storage-metrics-migration#metrics-mapping-between-old-metrics-and-new-metrics). Los umbrales deberá modificarse correctamente porque la nueva métrica disponible es absoluta.

Las reglas de alertas clásicas en error AnonymousThrottlingError y error SASThrottlingError deben dividirse en dos nuevas alertas porque no hay ninguna métrica combinada que proporciona la misma funcionalidad. Los umbrales deberá adaptarse adecuadamente.

## <a name="rollout-phases"></a>Fases de implementación

La herramienta de migración se está implementando en fases a los clientes que usan reglas de alerta clásicas. Los propietarios de suscripción recibirá un correo electrónico cuando esté lista para la migración mediante el uso de la herramienta de la suscripción.

> [!NOTE]
> Dado que la herramienta se está implantando en fases, es posible que vea que la mayoría de las suscripciones no está preparada para la migración durante las fases iniciales.

Actualmente, un subconjunto de las suscripciones se marca como listo para la migración. El subconjunto incluye aquellas suscripciones que tienen reglas de alertas clásicas solo en los siguientes tipos de recursos. En las próximas fases se agregará compatibilidad con más tipos de recursos.

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

Cualquier usuario que tenga el rol integrado de colaborador de supervisión a nivel de suscripción puede desencadenar la migración. Los usuarios que tienen un rol personalizado con los permisos siguientes también pueden desencadenar la migración:

- */read
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*

## <a name="common-problems-and-remedies"></a>Problemas comunes y soluciones

Después de [desencadenar la migración](alerts-using-migration-tool.md), recibirá un correo electrónico en las direcciones que se proporciona para avisarle de que la migración está completa o si se requiere ninguna acción del usuario. Esta sección describen algunos problemas comunes y cómo resolverlos.

### <a name="validation-failed"></a>No se pudo realizar la validación

Debido a algunos cambios recientes en las reglas de alertas clásicas en su suscripción, no se puede migrar la suscripción. Este problema es temporal. Puede reiniciar la migración una vez que el estado de migración, se mueve hacia atrás **preparados para la migración** en unos días.

### <a name="policy-or-scope-lock-preventing-us-from-migrating-your-rules"></a>Ámbito o la directiva de bloqueo impide la migración de las reglas

Como parte de la migración, se crearán nuevas alertas de métricas y los nuevos grupos de acciones y, a continuación, se eliminarán las reglas de alerta clásicas. Sin embargo, hay una directiva o el ámbito de bloqueo impide la creación de recursos. Según el bloqueo de la directiva o ámbito, no se podrían migrar algunas o todas las reglas. Puede resolver este problema, quite el ámbito de bloqueo o la directiva temporalmente y desencadenar la migración de nuevo.

## <a name="next-steps"></a>Pasos siguientes

- [Cómo usar la herramienta de migración](alerts-using-migration-tool.md)
- [Preparar la migración](alerts-prepare-migration.md)
