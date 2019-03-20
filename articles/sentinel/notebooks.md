---
title: Capacidades de búsqueda con blocs de notas de versión preliminar de Azure Sentinel | Microsoft Docs
description: Este artículo describe cómo usar cuadernos con las capacidades de búsqueda Centinela de Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 1721d0da-c91e-4c96-82de-5c7458df566b
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 63ce2be847017ed7e80fe5e573d5553311f6af2f
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58107685"
---
# <a name="use-notebooks-to-hunt-for-anomalies"></a>Uso de cuadernos que para buscar anomalías

> [!IMPORTANT]
> Sentinel Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sentinel Azure aprovecha el potencial de cuadernos de Jupyter interactivos para proporcionar información y acciones para investigar o de búsqueda de anomalías de seguridad dentro de su entorno. Sentinel Azure viene cargado previamente con blocs de notas que se han desarrollado por analistas de seguridad de Microsoft. Cada bloc de notas es verdaderamente creada específicamente con un flujo de trabajo independiente para un caso de uso específico. Las visualizaciones se incluyen en cada bloc de notas para la exploración de datos más rápido y caza de amenaza. Personalizar los cuadernos integrados para satisfacer sus necesidades, crear nuevos cuadernos desde cero o importar blocs de notas de la Azure Sentinel "Comunidad de GitHub. Cuadernos de Jupyter se importan como un proyecto en la página de Azure Notebooks, lo que permite usuario tener acceso a todas sus blocs de notas Centinela de Azure en un solo lugar. Blocs de notas se pueden ejecutar con un clic de un botón o pueden configurarse por el usuario para que coincida con su entorno.

La experiencia completamente integrada permite Notebook para ejecutar en el almacenamiento y proceso en la nube sin sobrecarga de mantenimiento subyacente. La capacidad de aprovechar el ecosistema de cuadernos de Jupyter Notebook existente permite a los modelos de origen público, sin compartir los datos del cliente. 


Cada notebook se hospeda en el Azure Notebooks (actualmente en versión preliminar), un entorno de desarrollo interactivo en la nube de Azure. Blocs de notas siempre son accesibles, siempre está disponible desde cualquier explorador, en cualquier lugar del mundo.  El Azure Sentinel' integrados blocs de notas para la investigación y búsqueda se clonan en un proyecto que le pertenece y que puede modificar y adaptar a su entorno. Algunos de los cuadernos más populares integrados son:

- **Alerta de investigación y búsqueda**: Este cuaderno interactivo permite la evaluación de errores rápida de clases distintas de las alertas recuperando actividad relacionada y enriquecer la alerta con datos desde el que se generó la alerta y la actividad asociada.

- **Host del punto de conexión interactiva caza**: Permite al desglosar las actividades relevantes de seguridad asociadas con un host del punto de conexión de captura en busca de indicios de una infracción de seguridad.  

- **Office sesión interactiva caza**: Permite buscar inicios de sesión sospechosos por ubicaciones geográficas de los registros de sospechoso de visualización, así como mostrar patrones poco habituales en el inicio de sesión derivadas de los datos de Office 365.

## <a name="run-a-notebook"></a>Ejecutar un bloc de notas
En el ejemplo siguiente, se ejecuta el cuaderno integrado para buscar profundo profundizar en las anomalías de ubicación para ver si alguien en una posición inesperada está haciendo algo en la red.

1. En el portal de Azure Sentinel, haga clic en **blocs de notas** y, a continuación, seleccione cualquiera de los cuadernos integrados.
  
   ![Seleccione el Bloc de notas](./media/notebooks/select-notebook.png)

3. Haga clic en **importación** para clonar el repositorio de GitHub en el proyecto de Azure Notebooks.
   ![Cuaderno de importación](./media/notebooks/import1.png)
4. Cada cuaderno le guiará a través de los pasos para realizar una búsqueda o la investigación. Los modelos, bibliotecas y otras dependencias y configuración para la conectividad con Azure Sentinel se importa automáticamente para habilitar la ejecución de un solo clic. Todo el código y bibliotecas necesarias para ejecutar un bloc de notas se cargan previamente. Puede empezar inmediatamente la ejecución de un bloc de notas en el área de trabajo de Log Analytics sin ninguna configuración.

   ![Importación de repositorio](./media/notebooks/import2.png)

5. Explorar, modificar y ejecutar todos los cuadernos de ejemplo proporcionados. Se puede usar como bloques de creación para muchos escenarios diferentes.

   ![Seleccione el Bloc de notas](./media/notebooks/import3.png)



## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido cómo ejecutar una investigación caza mediante cuadernos en Azure Sentinel. Para obtener más información acerca de Centinela de Azure, consulte los artículos siguientes:

- [Captura de forma proactiva en busca de amenazas](hunting.md)
- [Uso de marcadores para guardar información interesante durante la búsqueda](bookmarks.md)