---
title: Detección de amenazas en Azure Security Center | Microsoft Docs
description: " Detecte amenazas y usuarios malintencionados integrando Microsoft Cloud App Security con Azure Security Center. "
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: rkarlin
ms.openlocfilehash: af7896ec4afaeefda7261542bf593a89a7bb9ae8
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551800"
---
# <a name="ueba-for-azure-resources-and-users"></a>UEBA para los usuarios y recursos de Azure 

Azure Security Center se asocia con Microsoft Cloud App Security para proporcionarle alertas basadas en análisis de comportamiento de usuarios y entidades (UEBA) para sus recursos y usuarios de Azure (actividad de Azure). Estas alertas detectan anomalías en el comportamiento de los usuarios y se basan en el análisis del comportamiento de usuarios y entidades y en el aprendizaje automático (ML) para que pueda ejecutar inmediatamente la detección avanzada de amenazas en todas las actividades de sus suscripciones. Debido a que se habilitan automáticamente, las nuevas detecciones de anomalías ofrecen resultados inmediatos proporcionando detecciones inmediatas, dirigidas a numerosas anomalías de comportamiento en los usuarios y recursos asociados con su suscripción. Además, estas alertas aprovechan los datos adicionales que ya existen en el motor de detección de Microsoft Cloud App Security para ayudarle a acelerar el proceso de investigación y contener amenazas continuas. 

> [!NOTE]
> Azure Security Center, que puede almacenar una copia de los datos del cliente relacionados con la seguridad, recopilados de un recurso del cliente o asociados con dicho recurso (por ejemplo, una máquina virtual o un inquilino de Azure Active Directory): (a) en la misma geoárea que ese recurso, excepto en esas geoáreas donde Microsoft todavía tiene que implementar Azure Security Center, en cuyo caso se almacenará una copia de dichos datos en Estados Unidos; y (b) cuando Azure Security Center utiliza otro servicio en línea de Microsoft para procesar dichos datos, puede almacenar tales datos de acuerdo con las reglas de geolocalización de ese otro servicio en línea.
>

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="prerequisites"></a>Requisitos previos

- Una [licencia de Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security) válida y activa
- [Nivel Estándar de Security Center](https://azure.microsoft.com/pricing/details/security-center/)
 
## <a name="threat-detection-alerts"></a>Alertas de detección de amenazas

Security Center admite las alertas de detección de anomalías de Cloud App Security, como:

**Viaje imposible**
-  Esta detección identifica dos actividades de usuario (en una o varias sesiones) que se originan desde ubicaciones geográficamente distantes dentro de un período de tiempo menor que el tiempo que habría tardado el usuario para viajar de la primera ubicación a la segunda, lo que indica que otro usuario está usando las mismas credenciales. Esta detección aprovecha un algoritmo de aprendizaje automático que omite "falsos positivos" obvios que contribuyen a la condición de viaje imposible, como las VPN y las ubicaciones que otros usuarios de la organización usan con regularidad. La detección tiene un período de aprendizaje inicial de siete días, durante el cual aprende el patrón de actividad del nuevo usuario.

**Actividad desde un país poco frecuente**
- Esta detección tiene en cuenta las ubicaciones de actividad anteriores para determinar las ubicaciones nuevas e infrecuentes. El motor de detección de anomalías almacena información sobre las ubicaciones anteriores utilizadas por los usuarios de la organización. Se desencadena una alerta cuando se produce una actividad desde una ubicación que ningún usuario de la organización ha visitado recientemente o que nunca ha visitado. 

**Actividad desde direcciones IP anónimas**
- Esta detección identifica que los usuarios estaban activos desde una dirección IP que se ha identificado como una dirección IP de proxy anónima. A menudo, estos servidores proxy los usan los usuarios que desean ocultar la dirección IP del dispositivo y es posible que se usen con fines malintencionados. Esta detección aprovecha un algoritmo de aprendizaje automático que reduce "falsos positivos", como las direcciones IP mal etiquetadas que otros usuarios de la organización utilizan ampliamente.
 
  ![alerta de detección de amenazas](./media/security-center-ueba-mcas/security-center-mcas-alert.png)

## <a name="disabling-threat-detection-alerts"></a>Deshabilitación alertas de detección de amenazas

Estas alertas están habilitadas de forma predeterminada, pero puede deshabilitarlas:

1. En la hoja Security Center, seleccione **Pricing & settings** (Precios y configuración) y seleccione la suscripción correspondiente.
2. Haga clic en **Detección de amenazas**.
3. En **Enable integrations** (Habilitar integraciones), desactive la opción **Allow Microsoft Cloud App Security to access my data** (Permitir a Microsoft Cloud App Security acceder a mis datos) y haga clic en **Guardar**.

   ![alerta de detección de amenazas](./media/security-center-ueba-mcas/security-center-mcas-optout.png)

> [!NOTE]
> Hay un período de aprendizaje inicial de siete días durante el que no todas las alertas de detección de anomalías se generan. Después de ese período, cada sesión se compara con la actividad, cuándo los usuarios estaban activos, las direcciones IP, los dispositivos, etc. que se detectaron durante el mes pasado y la puntuación de riesgo de estas actividades. Estas detecciones son parte del motor de detección de anomalías de aprendizaje automático que genera un perfil del entorno y desencadena alertas con respecto a una línea de base que se aprendió sobre la actividad de la organización. Estas detecciones también aprovechan los algoritmos de aprendizaje automático diseñados para generar perfiles de los usuarios y del patrón de inicio de sesión para reducir los falsos positivos.
>
  
## <a name="next-steps"></a>Pasos siguientes
Este artículo le enseñó a trabajar con la detección de amenazas en Azure Security Center. Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md): encuentre las preguntas más frecuentes sobre el uso del servicio.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): aprenda a supervisar el estado de los recursos de Azure.



<!--Image references-->
[1]: ./media/security-center-confidence-score/confidence-score.png
[2]: ./media/security-center-confidence-score/suspicious-confidence-score.png
