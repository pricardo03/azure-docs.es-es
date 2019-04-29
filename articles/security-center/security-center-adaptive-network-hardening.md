---
title: Protección de red adaptable en Azure Security Center | Microsoft Docs
description: " Obtenga información sobre cómo habilitar la protección de red adaptable en Azure Security Center. "
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: monhaber
ms.assetid: 09d62d23-ab32-41f0-a5cf-8d80578181dd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/13/2019
ms.author: monhaber
ms.openlocfilehash: cede54f69fbeec5e01c17d84436bdc4c9ee91002
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60706656"
---
# <a name="adaptive-network-hardening-in-azure-security-center"></a>Protección de red adaptable en Azure Security Center
Obtenga información sobre cómo configurar la protección de red adaptable en Azure Security Center.

## <a name="what-is-adaptive-network-hardening"></a>¿Qué es la protección de red adaptable?
Aplicar [grupos de seguridad (NSG) de red](https://docs.microsoft.com/azure/virtual-network/security-overview) filtrar el tráfico hacia y desde los recursos, mejora la postura de seguridad de red. Sin embargo, todavía puede haber algunos casos en que el tráfico real que fluyen a través de NSG es un subconjunto de las reglas NSG definidas. En estos casos, la mejora aún más la postura de seguridad puede lograrse mediante la protección de las reglas NSG, según los patrones de tráfico real.

Protección de red adaptable proporciona recomendaciones para proteger aún más las reglas de NSG. Usa un algoritmo que tenga en cuenta el tráfico real, conocido la configuración de confianza, inteligencia de amenazas y otros indicadores de riesgo, de aprendizaje automático y, a continuación, se proporcionan recomendaciones para permitir el tráfico solo desde las tuplas IP/puerto específicos.

Por ejemplo, supongamos que la regla NSG existente es permitir el tráfico desde 140.20.30.10/24 en el puerto 22. Recomendación de la adaptación red refuerzo, en función del análisis, se restringir el intervalo y permitir el tráfico desde 140.23.30.10/29, que es un intervalo más restringido de IP, y denegar todo el tráfico restante a ese puerto.

![vista de sistema de protección de red](./media/security-center-adaptive-network-hardening/traffic-hardening.png)


> [!NOTE]
> Se admiten las recomendaciones de protección de red adaptables en los siguientes puertos: 22, 3389, 21, 23, 445, 4333, 3306, 1433, 1434, 53, 20, 5985, 5986, 5432, 139, 66, 1128

## <a name="view-adaptive-network-hardening-alerts-and-rules"></a>Ver las alertas de protección de red adaptable y reglas

1. En Security Center, seleccione **redes** -> **protección de red adaptable**. Las máquinas virtuales de red aparecen en tres pestañas independientes:
   * **Los recursos en mal estado**: Máquinas virtuales que tienen actualmente las recomendaciones y alertas que se activaron mediante la ejecución del algoritmo adaptable protección de red. 
   * **Recursos correctos**: Máquinas virtuales sin alertas y recomendaciones.
   * **Sin analizar recursos**: Máquinas virtuales que no se puede ejecutar el algoritmo adaptable protección de red debido a una de las razones siguientes:
      * **Las máquinas virtuales son máquinas virtuales clásicas**:-se admiten solo máquinas virtuales de Azure Resource Manager.
      * **No hay suficientes datos están disponibles**: Para generar recomendaciones de protección de tráfico preciso, Security Center requiere al menos 30 días de datos de tráfico.
      * **Máquina virtual no está protegido por ASC estándar**: Solo las máquinas virtuales que se establecen en el plan de tarifa estándar de Security Center son aptas para esta característica.

     ![recursos en mal estado](./media/security-center-adaptive-network-hardening/unhealthy-resources.png)

2. Desde el **recursos incorrecto** pestaña, seleccione una máquina virtual para ver sus alertas y las reglas de protección recomendado para aplicar.

    ![alertas de protección](./media/security-center-adaptive-network-hardening/hardening-alerts.png)


## <a name="review-and-apply-adaptive-network-hardening-recommended-rules"></a>Revisar y aplicar la protección de red adaptable reglas recomendadas

1. Desde el **recursos incorrecto** pestaña, seleccione una máquina virtual. Se muestran las alertas y reglas de protección recomendado.
   ![alertas de protección](./media/security-center-adaptive-network-hardening/hardening-alerts.png)

   > [!NOTE]
   > El **reglas** ficha enumera las reglas de protección de red adaptable recomienda que agregue. El **alertas** ficha enumera las alertas que se generaron debido al tráfico, fluyen en el recurso, que no está dentro del intervalo IP permitido en las reglas recomendadas.

   ![reglas de protección](./media/security-center-adaptive-network-hardening/hardening-rules.png)

2. Si desea cambiar algunos de los parámetros de una regla, se pueden modificarlo, como se explica en [modificar una regla](#modify-rule).
   > [!NOTE]
   > También puede [eliminar](#delete-rule) o [agregar](#add-rule) una regla.

3. Seleccione las reglas que desea aplicar en el NSG y haga clic en **exigir**. 

### Modificar una regla  <a name ="modify-rule"> </a>

Es posible que desee modificar los parámetros de una regla que hayan sido recomendados. Por ejemplo, desea cambiar los intervalos de IP recomendados.

Algunas instrucciones importantes para modificar una regla de protección de red adaptable:

* Puede modificar los parámetros de reglas de "permitir" únicamente. 
* No se puede cambiar las reglas para convertirse en "Denegar" reglas "permitir". 

  > [!NOTE]
  > Creación y modificación de las reglas de "Denegar" se realiza directamente en el NSG para obtener más información, consulte [crear, cambiar o eliminar un grupo de seguridad de red](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group).

* Un **denegar todo el tráfico** regla es el único tipo de regla de "Denegar" que aquí se muestran y no puede modificarse. Puede, sin embargo, puede eliminarla (consulte [eliminar una regla](#delete-rule)).
  > [!NOTE]
  > Un **denegar todo el tráfico** regla se recomienda cuando, como resultado de ejecutar el algoritmo, Security Center no identifica el tráfico que se debe permitir, según la configuración de NSG existente. Por lo tanto, la regla recomendada es Denegar todo el tráfico al puerto especificado. El nombre de este tipo de regla se muestra como "generada por el sistema". Después de aplicar esta regla, su nombre real en el NSG será una cadena compuesta por el protocolo, dirección del tráfico, "Denegar" y un número aleatorio.

*Para modificar una regla de protección de red adaptable:*

1. Para modificar algunos de los parámetros de una regla, en el **reglas** pestaña, haga clic en los puntos suspensivos (...) al final de la fila de la regla y haga clic en **Editar regla**.

   ![Editar regla](./media/security-center-adaptive-network-hardening/edit-hard-rule.png)

1. En el **Editar regla** ventana, actualice los detalles que desea cambiar y haga clic en **guardar**.

   > [!NOTE]
   > Después de hacer clic **guardar**, ha cambiado correctamente la regla. *Sin embargo, no ha aplicado al NSG.* Para aplicarlo, debe seleccionar la regla en la lista y haga clic en **exigir** (como se explica en el paso siguiente).

3. Para aplicar la regla actualizada, en la lista, seleccione la regla actualizada y haga clic en **exigir**.

### Agregar una nueva regla <a name ="add-rule"> </a>


Puede agregar una regla "permitir" que no se recomienda por Security Center.

> [!NOTE]
> Las reglas de "Permitir" solo se pueden agregar aquí. Si desea agregar las reglas de "Denegar", puede hacer directamente en el NSG. Para obtener más información, consulte [crear, cambiar o eliminar un grupo de seguridad de red](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group).

*Para agregar una regla de protección de red adaptable:*

1. Haga clic en **Agregar regla** (ubicado en la esquina superior izquierda).

   ![Agregar regla](./media/security-center-adaptive-network-hardening/add-hard-rule.png)

1. En el **Editar regla** ventana, escriba los detalles y haga clic en **guardar**.

   > [!NOTE]
   > Después de hacer clic **guardar**, ha agregado correctamente la regla y se muestra con las demás reglas recomendadas. Sin embargo, no ha aplicado en el NSG. Para activarlo, debe seleccionar la regla en la lista y haga clic en **exigir** (como se explica en el paso siguiente).

3. Para aplicar la nueva regla, en la lista, seleccione la nueva regla y haga clic en **exigir**.



### Eliminar una regla <a name ="delete-rule"> </a>

Cuando sea necesario, puede eliminar una regla recomendada. Por ejemplo, puede determinar que aplicar una regla sugerida podría bloquear tráfico legítimo.

*Para eliminar una regla de protección de red adaptable:*

1. En el **reglas** pestaña, haga clic en los puntos suspensivos (...) al final de la fila de la regla y haga clic en **regla de eliminación**.

   ![Eliminar regla](./media/security-center-adaptive-network-hardening/delete-hard-rule.png)







 

