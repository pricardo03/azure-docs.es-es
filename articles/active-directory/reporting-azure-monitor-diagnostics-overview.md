---
title: Registros de actividad de Azure Active Directory en Azure Monitor (versión preliminar) | Microsoft Docs
description: Introducción a los registros de actividad de Azure Active Directory en Azure Monitor (versión preliminar)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 9b594360bc760fa9eec8ab9900c3aadcb10e9db6
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240115"
---
# <a name="azure-active-directory-activity-logs-in-azure-monitor-preview"></a>Registros de actividad de Azure Active Directory en Azure Monitor (versión preliminar)

Ahora puede enrutar los registros de actividad de Azure AD mediante Azure Monitor a su propia cuenta de almacenamiento o centro de eventos. Con la versión preliminar pública de los registros de Azure Active Directory en Azure Monitor, puede:

* Archivar los registros de auditoría para una cuenta de almacenamiento de Azure, lo que le permite conservar los datos durante mucho tiempo
* Transmitir sus registros de auditoría a un centro de eventos de Azure para su análisis mediante herramientas SIEM populares como Splunk y QRadar
* Integrar los registros de auditoría con sus propias soluciones de registro personalizadas mediante streaming a un centro de eventos

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

## <a name="supported-reports"></a>Informes admitidos

Puede enrutar registros de actividad de auditoría y registros de actividad de inicio de sesión a la cuenta de almacenamiento de Azure, un centro de eventos o una solución personalizada mediante esta característica. 

* **Registros de auditoría**: el [informe de actividad de registros de auditoría](active-directory-reporting-activity-audit-logs.md) le proporciona acceso al historial de todas las tareas llevadas a cabo en el inquilino.
* **Inicios de sesión**: con el [informe de actividad de inicios de sesión](active-directory-reporting-activity-sign-ins.md), puede determinar quién ha realizado las tareas notificadas en el informe de registros de auditoría.

> [!NOTE]
> En este momento no se admiten los registros de actividad de auditoría e inicio de sesión relacionados con B2C.
>

## <a name="prerequisites"></a>Requisitos previos

Para usar esta característica, necesita:

* Una suscripción de Azure. Si no tiene una suscripción de Azure, puede [registrarse para obtener una evaluación gratuita](https://azure.microsoft.com/free/).
* Una [licencia](https://azure.microsoft.com/pricing/details/active-directory/) Azure AD Free, Basic, Premium 1 o Premium 2 para acceder a los registros de Azure AD en Azure Portal. 

Dependiendo de hacia dónde desee enrutar los datos de registro de auditoría, necesitará:

* Una cuenta de almacenamiento de Azure, en la que tenga permisos *ListKeys*. Le recomendamos utilizar una cuenta de almacenamiento general y no una cuenta de almacenamiento de blobs. Para más información sobre precios de almacenamiento, consulte la [Calculadora de precios de Azure Storage](https://azure.microsoft.com/pricing/calculator/?service=storage). 
* Espacio de nombres de Azure Event Hubs para la integración con soluciones de terceros.

> [!NOTE]
> Debe ser un *administrador global* o *administrador de seguridad* en el inquilino de Azure AD para acceder a los registros de actividad de Azure AD.
>

## <a name="cost-considerations"></a>Consideraciones sobre el coste

Si ya tiene una licencia de Azure AD, necesita una suscripción a Azure para configurar la cuenta de almacenamiento y el centro de eventos. La suscripción a Azure es gratuita, pero tiene que pagar para utilizar los recursos de Azure, incluida la cuenta de almacenamiento que se usa para archivar y el centro de eventos que se utiliza para el streaming. La cantidad de datos, y por lo tanto el costo incurrido, variará significativamente dependiendo del tamaño del inquilino. 

### <a name="storage-size-for-activity-logs"></a>Tamaño de almacenamiento para registros de actividad

Cada evento del registro de auditoría utiliza aproximadamente 2 KB de almacenamiento de datos. Por lo tanto, para un inquilino con 100 000 usuarios, que incurriría en alrededor de 1,5 millones de eventos por día, necesitaría alrededor de 3 GB de almacenamiento de datos al día. Dado que las escrituras se producen en lotes de aproximadamente 5 minutos, puede anticipar aproximadamente 9000 operaciones de escritura al mes. La siguiente tabla contiene una estimación aproximada del costo, dependiendo del tamaño del inquilino, de una cuenta de almacenamiento de uso general v2 en la región Oeste de EE. UU. durante al menos un año de retención. Use la [Calculadora de precios de almacenamiento de Azure](https://azure.microsoft.com/pricing/details/storage/blobs/) para crear una estimación más precisa del volumen de datos previstos para la aplicación. Cada evento del registro de auditoría utiliza aproximadamente 2 KB de almacenamiento de datos. Por lo tanto, para un inquilino con 100 000 usuarios, que incurriría en alrededor de 1,5 millones de eventos por día, necesitaría alrededor de 3 GB de almacenamiento de datos al día. Dado que las escrituras se producen en lotes de aproximadamente 5 minutos, puede anticipar aproximadamente 9000 operaciones de escritura al mes. La siguiente tabla contiene una estimación aproximada del costo, dependiendo del tamaño del inquilino, de una cuenta de almacenamiento de uso general v2 en la región Oeste de EE. UU. durante al menos un año de retención. Use la [Calculadora de precios de almacenamiento de Azure](https://azure.microsoft.com/pricing/details/storage/blobs/) para crear una estimación más precisa del volumen de datos previstos para la aplicación. 

| Categoría del registro | Número de usuarios | Número de eventos al día | Volumen de datos aproximado al mes | Costo aproximado al mes | Costo aproximado al año |
|--------------|-----------------|----------------------|--------------------------------------|----------------------------|---------------------------|
| Auditoría | 100 000 | 1,5 millones | 90 GB | 1,93 USD | 23,12 USD |
| Auditoría | 1000 | 15000 | 900 MB | 0,02 USD | 0,24 USD |
| Inicios de sesión | 1000 | 34800 | 4 GB | 0,13 USD | 1,56 USD |
| Inicios de sesión | 100 000 | 15 millones | 1,7 TB | 35,41 USD | 424,92 USD | 


### <a name="event-hub-messages-for-activity-logs"></a>Mensajes del centro de eventos para los registros de actividad

Los eventos se agrupan en intervalos de aproximadamente cinco minutos y se envían como un único mensaje que contiene todos los eventos dentro de ese período de tiempo. Un mensaje en el centro de eventos tiene un tamaño máximo de 256 kB y, si el tamaño de todos los mensajes dentro del período de tiempo excede ese volumen, se envían múltiples mensajes. 

Por ejemplo, normalmente hay alrededor de 18 eventos por segundo para un inquilino grande de más de 100 000 usuarios, lo que equivale a 5400 eventos cada 5 minutos. Dado que los registros de auditoría son de unos 2 KB por evento, esto equivale a 10,8 MB de datos, por lo que se enviarán 43 mensajes al centro de eventos en ese intervalo de 5 minutos. La siguiente tabla contiene un costo aproximado para un centro de eventos básico en la región Oeste de Estados Unidos, dependiendo del volumen de datos de eventos. Use la [Calculadora de precios de Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/) para calcular una estimación más precisa del volumen de datos previstos para la aplicación.

| Categoría del registro | Número de usuarios | Número de eventos por segundo | Número de eventos por intervalo de 5 minutos | Volumen por intervalo | Número de mensajes por intervalo | Número de mensajes al mes | Costo aproximado al mes |
|--------------|-----------------|-------------------------|----------------------------------------|---------------------|---------------------------------|------------------------------|----------------------------|
| Auditoría | 100 000 | 18 | 5400 | 10,8 MB | 43 | 371,520 | 10,83 USD |
| Auditoría | 1000 | 0,1 | 52 | 104 KB | 1 | 8640 | 10,8 USD |
| Inicios de sesión | 1000 | 178 | 53400 | 106,8 MB | 418 | 3 611 520 | 11,06 USD |  


## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

En esta sección se incluyen las preguntas más frecuentes y los problemas conocidos con los registros de Azure Active Directory en Azure Monitor.

**P: ¿Por dónde debo empezar?** 

**R:** Comience por la [Introducción](reporting-azure-monitor-diagnostics-overview.md) para hacerse una idea de lo que necesita para implementar esta característica. Cuando esté familiarizado con los requisitos previos, consulte los tutoriales que le ayudarán a configurar y enrutar los registros a Event Hubs.

---

**P: ¿Qué registros se incluyen?**

**A:** Tanto los inicios de sesión como los registros de auditoría se pueden enrutar a través de esta característica, aunque actualmente no se incluyen los eventos de auditoría relacionados con B2C. Lea el [esquema de registro de auditoría](reporting-azure-monitor-diagnostics-audit-log-schema.md) y el [esquema de registro de inicio de sesión](reporting-azure-monitor-diagnostics-sign-in-log-schema.md) para averiguar qué tipos de registros y qué registros basados en características se admiten en estos momentos. 

---

**P: ¿Cuánto tiempo después de una acción aparecerán los registros correspondientes en Event Hubs?**

**R:** Los registros deberían aparecer en Event Hubs entre dos y cinco minutos después de realizar la acción. Para más información sobre Event Hubs, consulte [¿Qué es Azure Event Hubs?](/azure/event-hubs/event-hubs-what-is-event-hubs.md)

---

**P: ¿En cuánto tiempo después de una acción van a aparecer los registros correspondientes en las cuentas de almacenamiento?**

**R:** Para las cuentas de almacenamiento de Azure, la latencia está entre 5 y 15 minutos de realizar la acción.

---

**P: ¿Cuánto costará almacenar mis datos?**

**R:** El costo de almacenamiento depende del tamaño de sus registros, así como del período de retención que elija. Para una estimación aproximada de los costos para los inquilinos en función del volumen de registros generados, consulte la [Introducción](reporting-azure-monitor-diagnostics-overview.md).

---

**P: ¿Cuánto costará transmitir mis datos a Event Hubs?**

**R:** El costo del streaming depende del número de mensajes que reciba por minuto. Lea la [Introducción](reporting-azure-monitor-diagnostics-overview.md) para obtener más información sobre cómo se calcula el costo, así como para encontrar una estimación aproximada basada en el número de mensajes. 

---

**P: ¿Qué herramientas SIEM se admiten actualmente?** 

**R:** Actualmente, Splunk, QRadar y Sumologic admiten Azure Monitor. Sin embargo, Splunk es la única herramienta SIEM que se admite para los registros de Azure Active Directory. Para más información sobre cómo funcionan los conectores, consulte [Flujo de datos de supervisión de Azure a un centro de eventos para que lo consuma una herramienta externa](/azure/monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs).

---

**P: ¿Puedo acceder a los datos desde un centro de eventos sin utilizar una herramienta SIEM externa?** 

**R:** Sí, puede usar la [API de Event Hubs](/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph) para acceder a los registros desde la aplicación personalizada. 

---


## <a name="next-steps"></a>Pasos siguientes

* [Archivado de registros de actividad en una cuenta de almacenamiento](reporting-azure-monitor-diagnostics-azure-storage-account.md)
* [Enrutamiento de registros de actividad a un centro de eventos](reporting-azure-monitor-diagnostics-azure-event-hub.md)
* [Más información sobre los registros de Diagnósticos de Azure](/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)