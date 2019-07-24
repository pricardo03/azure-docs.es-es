---
title: Validación de alertas (archivo de prueba EICAR) en Azure Security Center | Microsoft Docs
description: Este documento le ayuda a validar las alertas de seguridad en Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: rkarlin
ms.openlocfilehash: f65b4b74a1a91fa081bd9c0d8146d055cebb0de6
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626301"
---
# <a name="alert-validation-eicar-test-file-in-azure-security-center"></a>Validación de alertas (archivo de prueba EICAR) en Azure Security Center
Este documento le ayuda a comprobar si el sistema está configurado correctamente para las alertas de Azure Security Center.

## <a name="what-are-security-alerts"></a>¿Qué son las alertas de seguridad?
Las alertas son notificaciones que Security Center genera cuando detecta amenazas en los recursos. Asigna prioridades y enumera las alertas, junto con la información necesaria para que pueda investigar rápidamente el problema. Security Center también proporciona recomendaciones sobre el modo en que puede corregir un ataque.
Para más información, lea [Alertas de seguridad en Azure Security Center](security-center-alerts-overview.md) y [Administración y respuesta a las alertas de seguridad en el Centro de seguridad de Azure](security-center-managing-and-responding-alerts.md).

## <a name="alert-validation"></a>Validación de alertas

* [Windows](#validate-windows)
* [Linux](#validate-linux)

## Validación de alertas en máquinas virtuales Windows <a name="validate-windows"></a>

Una vez que el agente de Security Center esté instalado en el equipo, siga estos pasos desde el equipo donde desee que esté el recurso atacado de la alerta:

1. Copie un archivo ejecutable (por ejemplo **calc.exe**) en el escritorio del equipo o en otro directorio que prefiera y cambie el nombre a **ASC_AlertTest_662jfi039N.exe**.
1. Abra el símbolo del sistema y ejecute este archivo con un argumento (un nombre de argumento falso), por ejemplo, ```ASC_AlertTest_662jfi039N.exe -foo```
1. Espere de cinco a diez minutos y abra Alertas de Security Center. Se mostrará una alerta similar a la del [ejemplo](#alert-validate) siguiente:

> [!NOTE]
> Al revisar esta alerta de prueba para Windows, asegúrese de que el campo **Arguments Auditing Enabled** (Auditoría de argumentos habilitada) aparece como **true**. Si es **false**, debe habilitar la auditoría de argumentos de línea de comandos. Para habilitarlo, utilice la línea de comandos siguiente:
>
>```reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"```

## Validación de alertas en máquinas virtuales Linux <a name="validate-linux"></a>

Una vez que el agente de Security Center esté instalado en el equipo, siga estos pasos desde el equipo donde desee que esté el recurso atacado de la alerta:
1. Copie un archivo ejecutable en una ubicación adecuada y cambie el nombre a **./asc_alerttest_662jfi039n**, por ejemplo:

    ```cp /bin/echo ./asc_alerttest_662jfi039n```

1. Abra el símbolo del sistema y ejecute este archivo:

    ```./asc_alerttest_662jfi039n testing eicar pipe```

1. Espere de cinco a diez minutos y abra Alertas de Security Center. Se mostrará una alerta similar a la del [ejemplo](#alert-validate) siguiente:

### Ejemplo de alerta <a name="alert-validate"></a>

![Ejemplo de validación de alertas](./media/security-center-alert-validation/security-center-alert-validation-fig2.png) 

## <a name="see-also"></a>Otras referencias
En este artículo se explicó el proceso de validación de las alertas. Ahora que conoce esta validación, intente los siguientes pasos:

* [Administración y respuesta a las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Aprenda a administrar las alertas y responder a incidentes de seguridad en Security Center.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md). Aprenda a supervisar el estado de los recursos de Azure.
* [Comprensión de las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Obtenga información acerca de los distintos tipos de alertas de seguridad.
* [Guía de solución de problemas de Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Obtenga información acerca de cómo solucionar problemas comunes en Security Center.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md). Preguntas más frecuentes acerca del uso del servicio.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/). Encuentre artículos de blog sobre el cumplimiento y la seguridad de Azure.
