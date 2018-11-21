---
title: Migración del área de trabajo y los proyectos de Microsoft Translator Hub Custom Translator
titleSuffix: Azure Cognitive Services
description: Migre el área de trabajo y los proyectos de Microsoft Translator Hub a Custom Translator.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 378baad0735238dc0921e5e78e2a27b3ae907e19
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51627289"
---
# <a name="migrate-hub-workspace-and-projects-to-custom-translator"></a>Migración del área de trabajo y los proyectos de Microsoft Translator Hub a Custom Translator

Puede migrar el área de trabajo y los proyectos de [Microsoft Translator Hub](https://hub.microsofttranslator.com/) a Custom Translator. La migración comienza en Microsoft Translator Hub.


Durante el proceso se migran estos elementos:

1.  Las definiciones de proyectos.

2.  La definición del aprendizaje se utilizará para crear una nueva definición del modelo en Custom Translator.

3.  Los archivos paralelos y monolingües que se emplearon en el aprendizaje se migrarán todos como nuevos documentos en Custom Translator.

4.  La prueba del sistema generada automáticamente y los datos de optimización se exportarán y crearán como nuevos documentos en Custom Translator.

Para todos los aprendizajes implementados, Custom Translator entrenará el modelo sin costo alguno. Tiene la opción de implementarlos de forma manual.

>[!Note]
>Para que un entrenamiento se realice correctamente, Custom Translator necesita un mínimo de 10 000 frases extraídas. Si dispone de un número de frases extraídas inferior al [mínimo sugerido](sentence-alignment.md#suggested-minimum-number-of-extracted-and-aligned-sentences), Custom Translator no podrá realizar un aprendizaje.

Todos los aprendizajes correctos, que no están implementados, se migrarán como borrador en Custom Translator.

## <a name="find-custom-translator-workspace-id"></a>Búsqueda del identificador de área de trabajo de Custom Translator

Para migrar un área de trabajo de [Microsoft Translator Hub](https://hub.microsofttranslator.com/) necesita el identificador del área de trabajo de destino en Custom Translator. El área de trabajo de destino en Custom Translator es el lugar al que se migrarán todas las áreas de trabajo y proyectos de Microsoft Translator Hub.

Puede encontrar el identificador del área de trabajo de destino en la página de configuración de Custom Translator: 

1.  Vaya a la página "Configuración" en el portal de Custom Translator.

2.  Encontrará el identificador del área de trabajo en la sección de información básica.

    ![Búsqueda del identificador del área de trabajo de destino](media/how-to/how-to-find-destination-ws-id.png)

3. Conserve el identificador del área de trabajo de destino para consultarlo durante el proceso de migración.

## <a name="migrate-workspace"></a>Migración del área de trabajo

Cuando migra el área de trabajo completa de Microsoft Translator Hub a Custom Translator, se migran los proyectos, documentos y aprendizajes. Antes de la migración, debe decidir si desea migrar solo los aprendizajes implementados o si desea migrar todos los aprendizajes correctos.

Para migrar un área de trabajo:

1.  Inicie sesión en Microsoft Translator Hub.

2.  Vaya a la página Configuración.

3.  En la página "Configuración", haga clic en "Migrate Workspace data to Custom Translator" (Migración de datos de área de trabajo a Custom Translator).

    ![Migración desde Microsoft Translator Hub](media/how-to/how-to-migrate-workspace-from-hub.png)

4.  En la siguiente página, seleccione cualquiera de estas dos opciones:

     a.  Solo los aprendizajes implementados: si selecciona esta opción se migrarán solo los sistemas implementados y los documentos relacionados.

    b.  Todos los aprendizajes correctos: si selecciona esta opción se migrarán todos los aprendizajes correctos y los documentos relacionados.

    c.  Escriba el identificador del área de trabajo de destino en Custom Translator.

    ![Migración desde Microsoft Translator Hub](media/how-to/how-to-migrate-from-hub-screen.png)

5.  Haga clic en Enviar solicitud.

## <a name="migrate-project"></a>Migración de un proyecto

Si desea migrar los proyectos de forma selectiva, Microsoft Translator Hub le ofrece esa posibilidad.

Para migrar un proyecto:

1.  Inicie sesión en Microsoft Translator Hub.

2.  Vaya a la página "Proyectos".

3.  Haga clic en el vínculo "Migrar" del proyecto adecuado.

    ![Migración desde Microsoft Translator Hub](media/how-to/how-to-migrate-from-hub.png)

4.  En la siguiente página, seleccione cualquiera de estas dos opciones:

     a.  Solo los aprendizajes implementados: si selecciona esta opción se migrarán solo los sistemas implementados y los documentos relacionados. 

    b.  Todos los aprendizajes correctos: si selecciona esta opción se migrarán todos los aprendizajes correctos y los documentos relacionados.

    c.  Escriba el identificador del área de trabajo de destino en Custom Translator.

    ![Migración desde Microsoft Translator Hub](media/how-to/how-to-migrate-from-hub-screen.png)

5.  Haga clic en "Enviar solicitud".

## <a name="migration-history"></a>Historial de migraciones

Si solicita la migración del área de trabajo o de los proyectos de Microsoft Translator Hub, puede ver el historial de migraciones en la página Configuración de Custom Translator. 

Para ver el historial de migraciones realice estos pasos:

1.  Vaya a la página "Configuración" en el portal de Custom Translator.

2.  En la sección del historial de migraciones de la página Configuración, haga clic en Historial de migraciones.

    ![Historial de migraciones](media/how-to/how-to-migration-history.png)

La página Historial de migraciones muestra la siguiente información como resumen para cualquier migración que solicite.

1.  Migrada por: nombre y correo electrónico del usuario que envió esta solicitud de migración

2.  Fecha de migración: marca de fecha y hora de la migración

3.  Proyectos: número de proyectos para los que se solicitó la migración en comparación con el número de proyectos que se migraron correctamente.

4.  Aprendizajes: número de aprendizajes para los que se solicitó la migración en comparación con el número de aprendizajes que se migraron correctamente.

5.  Documentos: número de documentos para los que se solicitó la migración en comparación con el número de documentos que se migraron correctamente.

    ![Detalles del historial de migraciones](media/how-to/how-to-migration-history-details.png)

Si desea obtener un informe de migración más detallado sobre los proyectos, aprendizajes y documentos, puede exportar los detalles en un archivo .csv.

>[!Note]
>Solo se admite la migración para los pares de idiomas que existen para NMT. Compruebe la lista de los [idiomas de NMT admitidos actualmente](https://www.microsoft.com/translator/business/languages/). Para los pares de idiomas en los que hay un idioma que no existe para NMT, los datos se moverán desde Microsoft Translator Hub a Custom Translator, pero los aprendizajes no se realizarán en esos pares de idiomas.

## <a name="next-steps"></a>Pasos siguientes

- [Entrenamiento de un modelo](how-to-train-model.md).
- Comience a utilizar el modelo de traducción personalizado implementado mediante [Microsoft Translator Text API V3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).