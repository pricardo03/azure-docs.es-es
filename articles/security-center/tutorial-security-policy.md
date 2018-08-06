---
title: 'Tutorial de Azure Security Center: Definición y evaluación de las directivas de seguridad | Microsoft Docs'
description: 'Tutorial de Azure Security Center: Definición y evaluación de las directivas de seguridad'
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/30/2018
ms.author: terrylan
ms.openlocfilehash: 15c69bce87ede96eb3a7bc0bada4e4f6a6669abb
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "39358615"
---
# <a name="tutorial-define-and-assess-security-policies"></a>Tutorial: Definición y evaluación de las directivas de seguridad
Security Center ayuda a garantizar el cumplimiento de los requisitos de seguridad normativos o de la empresa mediante el uso de directivas de seguridad para definir la configuración deseada de las cargas de trabajo. Después de definir las directivas de las suscripciones de Azure y adaptarlas al tipo de carga de trabajo o al nivel de confidencialidad de los datos, Security Center puede proporcionar recomendaciones de seguridad para sus recursos de proceso, aplicaciones, redes, datos y almacenamiento e identidad y recursos de acceso. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configuración de la directiva de seguridad
> * Evaluar la seguridad de los recursos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos
Para recorrer todas las características que se tratan en este tutorial, es preciso tener el plan de tarifa Estándar de Security Center. Dicho plan se puede probar de forma gratuita los primeros 60 días. En [Guía de inicio rápido de Azure Security Center](security-center-get-started.md) le explicamos cómo realizar la actualización al plan de tarifa Estándar.

## <a name="configure-security-policy"></a>Configuración de la directiva de seguridad
Security Center crea automáticamente una directiva de seguridad predeterminada para cada una de las suscripciones de Azure. Las directivas de seguridad se componen de recomendaciones que puede activar o desactivar según los requisitos de seguridad de esa suscripción. Para hacer cambios en la directiva de seguridad predeterminada debe ser propietario, colaborador o administrador de seguridad de la suscripción.

1. En el menú principal de Security Center, seleccione **Directiva de seguridad**.
2. Seleccione la suscripción que quiere usar.

  ![Directiva de seguridad](./media/tutorial-security-policy/tutorial-security-policy-fig1.png)  

3. En **COMPONENTES DE LA DIRECTIVA**, seleccione **Directiva de seguridad**
4. Para cada configuración de seguridad que desee supervisar, seleccione **Activado**. Security Center evaluará continuamente la configuración del entorno y cuando exista una vulnerabilidad generará una recomendación de seguridad. Seleccione **Desactivado** si la configuración de seguridad no es recomendable o no es pertinente. Por ejemplo, en un entorno de desarrollo y pruebas seguramente no necesite el mismo nivel de seguridad que en un entorno de producción. Después de seleccionar las directivas que son aplicables al entorno, haga clic en **Guardar**.

  ![Configuración de seguridad](./media/tutorial-security-policy/tutorial-security-policy-fig6.png)  

Espere a que Security Center procese estas directivas y genere las recomendaciones. Algunas configuraciones, como las actualizaciones del sistema y las configuraciones del sistema operativo pueden tardar hasta 12 horas, mientras que las configuraciones de los grupos de seguridad de red y de cifrado se evalúan casi al instante. Una vez que ve las recomendaciones en el panel de Security Center, puede continuar con el paso siguiente.

## <a name="assess-security-of-resources"></a>Evaluación de la seguridad de los recursos
1. Según las directivas de seguridad que se hayan habilitado, Security Center proporcionará un conjunto de recomendaciones de seguridad según sea necesario. Debe empezar por revisar las recomendaciones acerca de las máquinas virtuales y equipos. En el panel de Security Center, haga clic en **Información general** y, a continuación, en **Compute & apps** (Proceso y aplicaciones).

  ![Compute](./media/tutorial-security-policy/tutorial-security-policy-fig2.png)

  Revise cada recomendación dando prioridad a las recomendaciones en rojo (prioridad alta). Algunas de estas recomendaciones incluyen correcciones que se pueden implementar directamente desde Security Center, como los [problemas de protección de puntos de conexión](https://docs.microsoft.com/azure/security-center/security-center-install-endpoint-protection). Otras recomendaciones incluyen solo las directrices para aplicar la corrección como, por ejemplo, la recomendación acerca del cifrado de discos que faltan.

2. Una vez resueltas todas las recomendaciones de proceso pertinentes, debe pasar a la siguiente carga de trabajo: redes. En el panel Security Center, haga clic en **Información general** y, a continuación, en **Redes**.

  ![Redes](./media/tutorial-security-policy/tutorial-security-policy-fig3.png)

  La página de recomendaciones de red incluye una lista con problemas de seguridad de la configuración de red, los puntos de conexión accesibles desde Internet y la topología de red. Al igual que con **Compute & app** (Proceso y aplicaciones), algunas recomendaciones acerca de la red incluirán una corrección integrada y otras no.

3. Una vez resueltas todas las recomendaciones de red relevantes, debe pasar a la siguiente carga de trabajo: almacenamiento y datos. En el panel de Security Center, haga clic en **Información general** y, a continuación, en **Data & storage** (Datos y almacenamiento).

  ![Recursos de datos](./media/tutorial-security-policy/tutorial-security-policy-fig4.png)

  La página **Recursos de datos** contiene recomendaciones acerca de la habilitación de la auditoría de servidores y bases de datos de Azure SQL, el cifrado de bases de datos SQL Database y la habilitación del cifrado de su cuenta de Azure Storage. Si no tiene estas cargas de trabajo, no verá ninguna recomendación. Al igual que con **Compute & apps** (Proceso y aplicaciones), algunas recomendaciones relativas a datos y almacenamiento incluirán una corrección integrada y otras no.

4. Una vez resueltas todas las recomendaciones apropiadas sobre datos y almacenamiento, debe pasar a la siguiente carga de trabajo: identidad y acceso. En el panel Security Center, haga clic en **Información general** y, a continuación, en **Identity & access** (Identidad y acceso).

  ![Identidad y acceso](./media/tutorial-security-policy/tutorial-security-policy-fig5.png)

  La página **Identity & Access** (Identidad y acceso) contiene recomendaciones como:

   - Habilitar MFA para las cuentas con privilegios en la suscripción
   - Quitar las cuentas externas con permisos de escritura de la suscripción
   - Quitar las cuentas externas con privilegios de la suscripción

## <a name="clean-up-resources"></a>Limpieza de recursos
Otras guías de inicio rápido y tutoriales de esta colección se basan en los valores de esta. Si planea continuar trabajando con las guías rápidas y tutoriales posteriores, debe seguir ejecutando el plan de tarifa Estándar y mantener el aprovisionamiento automático habilitado. Si no planea continuar o desea volver al nivel Gratis:

1. Vuelva al menú principal de Security Center y seleccione **Directiva de seguridad**.
2. Seleccione la suscripción o directiva que desea que vuelva al nivel Gratis. Se abre **Directiva de seguridad**.
3. En **COMPONENTES DE LA DIRECTIVAS**, seleccione **Plan de tarifa**.
4. Seleccione **Gratis** para cambiar la suscripción de Estándar a Gratis.
5. Seleccione **Guardar**.

Si desea deshabilitar el aprovisionamiento automático:

1. Vuelva al menú principal de Security Center y seleccione **Directiva de seguridad**.
2. Seleccione la suscripción en la que quiere deshabilitar el aprovisionamiento automático.
3. En **Directiva de seguridad: Colección de datos**, en **Incorporación**, seleccione **Desactivado** para deshabilitar el aprovisionamiento automático.
4. Seleccione **Guardar**.

>[!NOTE]
> La deshabilitación del aprovisionamiento automático no quita Microsoft Monitoring Agent de las máquinas virtuales de Azure en las que se ha aprovisionado el agente. La deshabilitación del aprovisionamiento automático limita la supervisión de seguridad de los recursos.
>

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha aprendido los conceptos básicos acerca de la definición de directivas y evaluación de la seguridad de la carga de trabajo con Security Center. Conceptos como:

> [!div class="checklist"]
> * Configuración de directivas de seguridad para garantizar el cumplimiento con los requisitos normativos de seguridad o los de la empresa
> * Evaluación de la seguridad de los recursos de proceso, redes, SQL y almacenamiento, y aplicaciones

Continúe en el siguiente tutorial para aprender a usar Security Center para proteger los recursos.

> [!div class="nextstepaction"]
> [Protección de los recursos](tutorial-protect-resources.md)
