---
title: Pruebas de comprobación de características interactiva en Azure Stack con validación como servicio | Microsoft Docs
description: Aprenda a crear pruebas de comprobación de características interactiva para Azure Stack con validación como servicio.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/07/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: a483a8e5e4c37c51c4efce13ad69b4abc83a402d
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54105379"
---
# <a name="interactive-feature-verification-testing"></a>Pruebas de comprobación de características interactivas  

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Puede usar el marco de pruebas de comprobación de características interactiva para solicitar la realización de en su sistema. Cuando se solicita una prueba, Microsoft utiliza el marco para preparar las pruebas que requieren pasos manuales interactivos. Microsoft puede usar el marco de trabajo para encadenar varias pruebas automatizadas independientes.

En este artículo se describe un escenario manual simple. La prueba comprueba la sustitución de un disco en Azure Stack. El marco recopila registros de diagnóstico en cada paso. Los problemas se pueden depurar a medida que se detectan. El marco también permite el uso compartido de registros que generan otras herramientas o procesos, así como proporcionar comentarios acerca del escenario.

## <a name="overview-of-a-test-pass"></a>Introducción a las pruebas

La realización de una prueba antes de sustituir un disco es un escenario común. En este ejemplo, la prueba tiene siete pasos:

1.  Cree un flujo de trabajo de **Test Pass** (Prueba superada).
2.  Seleccione la **prueba de identificación de disco**.
3.  Inicie la prueba.
4.  Elija las acciones en el escenario de comprobación interactiva.
5.  Compruebe el resultado del escenario.
6.  Envíe el resultado de la prueba a Microsoft.
7.  Compruebe el estado en el portal de VaaS.

## <a name="create-a-new-test-pass"></a>Creación de una prueba

1.  Vaya al [portal de validación de Azure Stack](https://www.azurestackvalidation.com) e inicie sesión.

2.  Cree una solución nueva o elija una existente.

3.  Seleccione **Iniciar** en el icono **Prueba superada**.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image1.png)

4.  Escriba el nombre del flujo de trabajo de **Test Pass** (Prueba superada).

5.  Escriba los parámetros de prueba comunes y del entorno, para lo que debe seguir las instrucciones del artículo [Parámetros comunes del flujo de trabajo en la validación como servicio de Azure Stack](azure-stack-vaas-parameters.md).

6.  La cadena de conexión de diagnóstico debe seguir el formato especificado en la sección [Parámetros de prueba](azure-stack-vaas-parameters.md#test-parameters) del artículo acerca de los [parámetros comunes del flujo de trabajo](azure-stack-vaas-parameters.md).

7.  Escriba los parámetros necesarios para la prueba.

8.  Seleccione el agente que va a ejecutar la serie de pruebas interactivas.

> [!Note]  
> Para la prueba de comprobación de características interactiva de la identificación del disco se deben especificar la contraseña y el usuario administrador de dominio.

![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image2.png)

## <a name="select-the-test"></a>Seleccione la prueba

1.  Seleccione **Disk Identification Test (Prueba de identificación de disco) \<versión>**.

    > [!Note]  
    > La versión de la prueba aumentará a medida que se realicen mejoras en la documentación y el material adjunto. Siempre se debe usar la versión más alta, salvo que Microsoft indique lo contrario.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image4.png)

2.  Para especificar la contraseña y el usuario administrador de dominio, seleccione **Edit** (Editar).

3.  Seleccione el agente de ejecución de pruebas o DVM apropiados en el que ejecutar la prueba.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image5.png)

4.  Seleccione **Submit** (Enviar) para iniciar la prueba.

![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image6.png)

## <a name="start-the-test"></a>Inicio de la prueba

Los mensajes de la prueba de identificación de disco se muestran en el equipo que ejecuta al agente de VaaS. Normalmente es la DVM o Jumpbox de la instancia de Azure Stack.

![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image8.png)

## <a name="choose-the-actions"></a>Elección de las acciones

1.  En los vínculos **Documentation**(Documentación) y **Validation** (Validación) encontrará instrucciones de Microsoft para actuar en este escenario.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image9.png)

2.  Seleccione **Next** (Siguiente).

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image10.png)

3.  Siga las instrucciones para ejecutar el script de comprobaciones previas.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image11.png)

4.  Una vez que dicho script se haya completado correctamente, ejecute el escenario manual (sustitución del disco) tal como se indica en los vínculos **Documentation** (Documentación) y **Validation** (Validación) de la pestaña **Information** (Información).

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image12.png)

5.  No cierre el cuadro de diálogo mientras actúa en el escenario manual.

6.  Cuando haya terminado, siga las instrucciones para ejecutar el script de comprobaciones posteriores.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image13.png)

7.  Cuando el escenario manual se haya completado correctamente (sustitución del disco), seleccione **Next** (Siguiente).

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image14.png)

> [!Important]  
> Si cierra la ventana, la prueba se detendrá antes de haber acabado.

## <a name="check-the-status"></a>Comprobación del estado

1.  Cuando se complete la prueba, se le pedirá que realice los comentarios que desee.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image15.png)

2.  Estas preguntas ayudarán a Microsoft a evaluar la tasa de éxito y la calidad de la versión del escenario.

## <a name="send-the-test-result"></a>Envío del resultado de la prueba

1.  Adjunte los archivos de registro que desee enviar a Microsoft.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image16.png)

2.  Acepte el CLUF del envío de comentarios.

3.  Seleccione **Submit** (Enviar) para enviar los resultados a Microsoft.

## <a name="next-steps"></a>Pasos siguientes

- [Supervisión y administración de pruebas en el portal de VaaS](azure-stack-vaas-monitor-test.md)
