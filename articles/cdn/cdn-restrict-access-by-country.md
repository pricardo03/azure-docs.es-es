---
title: Restricción del contenido de la red CDN de Azure por país | Microsoft Docs
description: Aprenda cómo restringir el acceso por país a su contenido de Azure CDN mediante la característica de filtrado geográfico.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2018
ms.author: v-deasim
ms.openlocfilehash: 661356aeb2369bc1bbddd6caee57b256dd9e1212
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285022"
---
# <a name="restrict-azure-cdn-content-by-country"></a>Restricción del contenido de la red CDN de Azure por país

## <a name="overview"></a>Información general
Cuando un usuario solicita su contenido, de forma predeterminada, el contenido se proporciona sin tener en cuenta la ubicación desde la que el usuario realiza esta solicitud. Sin embargo, puede que en algunos casos desee restringir el acceso al contenido por país. Con la característica de *filtrado geográfico*, puede crear reglas sobre rutas de acceso específicas en su punto de conexión de CDN para permitir o bloquear contenido en países seleccionados.

> [!IMPORTANT]
> Los perfiles **Azure CDN Estándar de Microsoft** no admiten el filtrado geográfico basado en rutas.
> 

## <a name="standard-profiles"></a>Perfiles estándar
Los procedimientos de esta sección se aplican solo a perfiles de **Azure CDN Estándar de Akamai** y **Azure CDN Estándar de Verizon**. 

Con los perfiles de **Azure CDN Premium de Verizon**, debe usar el **Portal de administración** para activar el filtrado geográfico. Para más información, consulte [Perfiles de Azure CDN Premium de Verizon](#azure-cdn-premium-from-verizon-profiles).

### <a name="define-the-directory-path"></a>Definición de la ruta de acceso a directorio
Para acceder a la característica de filtrado geográfico, seleccione el punto de conexión de CDN en el portal y, luego, seleccione **Filtrado geográfico** en Configuración en el menú izquierdo. 

![Estándar de filtrado geográfico](./media/cdn-filtering/cdn-geo-filtering-standard.png)

En el cuadro **Ruta de acceso**, especifique la ruta de acceso relativa a la ubicación a la que se les permitirá o denegará el acceso a los usuarios. 

Puede aplicar el filtrado geográfico a todos los archivos mediante una barra diagonal (/), o seleccionar carpetas concretas mediante la especificación de rutas de acceso a directorio (por ejemplo, */pictures/*). También puede aplicar el filtrado geográfico a un único archivo (por ejemplo, */pictures/city.png*). Se permiten varias reglas; después de especificar una regla, aparece una fila en blanco para que escriba la siguiente regla.

Por ejemplo, todos los filtros siguientes de ruta de acceso a directorio son válidos:   
*/*                                 
*/Photos/*     
*/Photos/Strasbourg/*     
*/Photos/Strasbourg/city.png*

### <a name="define-the-type-of-action"></a>Definición del tipo de acción

En la lista **Acción**, seleccione **Permitir** o **Bloquear**: 

- **Permitir**: solo los usuarios de los países especificados pueden acceder a los recursos solicitados desde la ruta recursiva.

- **Bloquear**: a los usuarios de los países especificados se les deniega el acceso a los recursos solicitados desde esa ruta recursiva. Si no se han configurado otras opciones de filtrado de país para esa ubicación, a continuación, se permitirá acceso a todos los demás usuarios.

Por ejemplo, una regla de filtrado geográfico para bloquear la ruta de acceso */Photos/Strasbourg/* filtra los siguientes archivos:     
*http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg*
*http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg*

### <a name="define-the-countries"></a>Definición de los países
En la lista **Códigos de país**, seleccione los países para los que quiere bloquear o permitir la ruta de acceso. 

Cuando haya terminado de seleccionar los países, seleccione **Guardar** para activar la nueva regla de filtrado geográfico. 

![Reglas de filtrado geográfico](./media/cdn-filtering/cdn-geo-filtering-rules.png)

### <a name="clean-up-resources"></a>Limpieza de recursos
Para eliminar una regla, selecciónela de la lista en la página **Filtrado geográfico** y luego elija **Eliminar**.

## <a name="azure-cdn-premium-from-verizon-profiles"></a>Perfiles de Azure CDN Premium de Verizon
En los perfiles de **Azure CDN Premium de Verizon**, la interfaz de usuario para crear una regla de filtrado geográfico es diferente:

1. En el menú superior del perfil de Azure CDN, seleccione **Administrar**.

2. En el portal de Verizon, seleccione **HTTP Large** (HTTP grande) y, luego, **Country Filtering** (Filtrado de países).

    ![Estándar de filtrado geográfico](./media/cdn-filtering/cdn-geo-filtering-premium.png)

3. Seleccione **Add Country Filter** (Agregar filtro de país).

    Aparece la página **Step One:** (Paso 1:).

4. Escriba la ruta de acceso al directorio, seleccione **Block** (Bloquear) o **Add** (Agregar) y, luego, seleccione **Next** (Siguiente).

    Aparece la página **Step Two:** (Paso 2:). 

5. Seleccione uno o varios países de la lista y, luego, seleccione **Finish** (Finalizar) para activar la regla. 
    
    La nueva regla aparece en la tabla en la página **Country Filtering** (Filtrado de países).

    ![Reglas de filtrado geográfico](./media/cdn-filtering/cdn-geo-filtering-premium-rules.png)

### <a name="clean-up-resources"></a>Limpieza de recursos
En la tabla de reglas de filtrado de países, seleccione el icono de eliminación situado junto a una regla para eliminarla o el icono de edición para modificarla.

## <a name="considerations"></a>Consideraciones
* Los cambios en la configuración de filtrado geográfico no surten efecto inmediatamente:
   * En los perfiles de **Azure CDN Estándar de Microsoft**, la propagación se completa normalmente en 10 minutos. 
   * En los perfiles de **Azure CDN estándar**, la propagación normalmente se completa en un minuto. 
   * En los perfiles **Azure CDN Estándar de Verizon** y **Azure CDN Premium de Verizon**, la propagación se completa normalmente en 10 minutos. 
 
* Esta característica no admite caracteres comodín (por ejemplo, ‘*’).

* La configuración de filtrado geográfico asociada a la ruta de acceso relativa se aplica de forma recursiva a esa ruta.

* Se puede aplicar solo una regla a la misma ruta de acceso relativa. Es decir, no se pueden crear varios filtros de países que apunten a la misma ruta de acceso relativa. Sin embargo, dado que los filtros de país son recursivos, una carpeta puede tener varios filtros de país. En otras palabras, se puede asignar un filtro de país diferente a una subcarpeta de una carpeta configurada previamente.

* La característica de filtrado geográfico usa códigos de país para definir los países desde los que se permite o bloquea una solicitud en un directorio protegido. Aunque los perfiles de Akamai y Verizon admiten la mayoría de los mismos códigos de país, hay algunas diferencias. Para más información, consulte [Azure CDN Country Codes](https://msdn.microsoft.com/library/mt761717.aspx) (Códigos de país de Azure CDN). 

