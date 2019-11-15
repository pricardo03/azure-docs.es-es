---
title: Aplicación de HTTPS mediante el motor de reglas estándar de Azure CDN | Microsoft Docs
description: El motor de reglas estándar permite personalizar cómo controla Azure CDN de Microsoft las solicitudes HTTP, como el bloqueo de la entrega de determinados tipos de contenido, la definición de una directiva de almacenamiento en caché y la modificación de encabezados HTTP.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: magattus
ms.openlocfilehash: b24c4a04e0c02258a918ee075066d90c22ea0c75
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615831"
---
# <a name="azure-cdn-standard-rules-engine"></a>Motor de reglas estándar de Azure CDN

> [!NOTE]
> El motor de reglas estándar solo está disponible para Azure CDN de Microsoft. 

El motor de reglas estándar de Azure CDN permite personalizar cómo se controlan las solicitudes HTTP. Por ejemplo, exigir la entrega de contenido a través de protocolos concretos, definir una directiva de almacenamiento en caché o modificar un encabezado HTTP. En este tutorial se muestra cómo crear una regla que redirigirá automáticamente a los usuarios a HTTPS. 


## <a name="tutorial"></a>Tutorial

1. En la página **Perfil de CDN** en Azure CDN de perfiles de Microsoft, seleccione el punto de conexión para el que quiere configurar las reglas.
  
2. Seleccione la pestaña **Motor de reglas** situada a la izquierda.
   
    Se muestra la hoja Motor de reglas, con la regla global. 
   
    [![Página de nuevas reglas de CDN](./media/cdn-standard-rules-engine/cdn-new-rule.png)](./media/cdn-standard-rules-engine/cdn-new-rule.png#lightbox)
   
   > [!IMPORTANT]
   > El orden en que se muestran varias reglas afecta a la manera en que se controlan. Una regla posterior puede invalidar las acciones especificadas por una regla anterior.
   >

3. Haga clic en el botón **Agregar regla** y proporcione un nombre de regla. Los nombres de las reglas deben empezar con una letra y contener solamente números y letras.

4. Identifique el tipo de solicitudes al que se aplica la regla. Use la lista desplegable para seleccionar la condición de coincidencia **Protocolo de solicitud** y use el valor **Es igual a** **HTTP**.
   
   [![Condición de coincidencia de regla de CDN](./media/cdn-standard-rules-engine/cdn-match-condition.png)](./media/cdn-standard-rules-engine/cdn-match-condition.png#lightbox)
   
   > [!NOTE]
   > Existen varias condiciones de coincidencia en la lista desplegable. Para ver una lista detallada de las condiciones de coincidencia, consulte [Condiciones de coincidencia del motor de reglas](cdn-standard-rules-engine-match-conditions.md).
   
5. Elija la acción que se va a aplicar a las solicitudes identificadas. Use la lista desplegable para seleccionar la acción de **redireccionamiento de URL** y use el valor **Encontrado (302)** para el tipo y **HTTPS** para el protocolo. Deje el resto de campos en blanco para usar los valores entrantes.
   
   [![Acción de regla de CDN](./media/cdn-standard-rules-engine/cdn-action.png)](./media/cdn-standard-rules-engine/cdn-action.png#lightbox)
   
   > [!NOTE]
   > Hay varias acciones disponibles en la lista desplegable. Para obtener una lista detallada de acciones, vea [Acciones del motor de reglas](cdn-standard-rules-engine-actions.md).

6. Seleccione **Guardar** para guardar la regla nueva.  La regla nueva se implementará ahora.
   
   > [!IMPORTANT]
   > Los cambios de las reglas pueden tardar hasta 15 minutos en propagarse a través de Azure CDN.
   >
   

## <a name="see-also"></a>Otras referencias

- [Información general de Azure CDN](cdn-overview.md)
- [Referencia del motor de reglas estándar](cdn-standard-rules-engine-reference.md)
- [Condiciones de coincidencia del motor de reglas estándar](cdn-standard-rules-engine-match-conditions.md)
- [Acciones del motor de reglas estándar](cdn-standard-rules-engine-actions.md)
