---
title: Cómo corregir las reglas predeterminadas modificadas - Azure AD Connect | Microsoft Docs
description: Obtenga información sobre cómo corregir las reglas modificadas predeterminadas que vienen con Azure AD Connect.
services: active-directory
author: billmath
manager: daveba
editor: curtand
ms.reviewer: darora10
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/21/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 761f3e6e72319a2e63d6b66f2893130ec5a82ebf
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59698171"
---
# <a name="fix-modified-default-rules-in-azure-ad-connect"></a>Corregir las reglas predeterminadas modificadas en Azure AD Connect

Azure AD Connect se incluye con las reglas predeterminadas para la sincronización.  Lamentablemente, estas reglas no se aplican universalmente a todas las organizaciones y puede haber ocasiones, según sus requisitos, cuando deba modificarlas.

 Si ha modificado las reglas predeterminadas o planea modificarlos, dedique un momento a leer este documento.

Este documento presenta 2 ejemplos de las personalizaciones más comunes realizadas por los usuarios y explica la manera correcta para lograr estas personalizaciones.

>[!NOTE] 
> Modificar las reglas existentes de forma predeterminada para lograr una personalización necesaria no se admite - haciendo esto que evitará actualizar estas reglas a la versión más reciente en futuras versiones. Esto impedirá que reciba las correcciones necesarias y las nuevas características.  Este documento explica cómo lograr el mismo resultado sin modificar las reglas predeterminadas existentes. 

## <a name="how-to-identify-modified-default-rules"></a>¿Cómo identificar las reglas predeterminadas modificado?
A partir de la versión 1.3.7.0 de **Azure AD Connect**, ahora es fácil de identificar la regla predeterminada modificada. Puede ir a las aplicaciones de escritorio y haga clic en **Synchronization Rules Editor**.

![Editor de](media/how-to-connect-fix-default-rules/default1.png)

En el Editor, se mostrarán las reglas predeterminadas modificadas con un icono junto al nombre tal como se muestra a continuación:

![icon](media/how-to-connect-fix-default-rules/default2.png)

 También verá una regla deshabilitada con el mismo nombre junto a él, que es la regla predeterminada estándar:

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default2a.png)

## <a name="common-customizations"></a>Personalizaciones comunes
Estas son las personalizaciones comunes a las reglas predeterminadas:

- [Cambiar el flujo de atributos](#changing-attribute-flow)
- [Cambiar filtro de ámbito](#changing-scoping-filter)
- [Cambiar la condición de combinación](#changing-join-condition)

## <a name="before-changing-any-rules"></a>Antes de cambiar las reglas
- Deshabilitar al programador de sincronización.  El programador se ejecuta cada 30 minutos de forma predeterminada. Asegúrese de que no se inicia mientras realiza cambios y soluciona problemas en las nuevas reglas. Para deshabilitar temporalmente el programador, inicie PowerShell y ejecute `Set-ADSyncScheduler -SyncCycleEnabled $false`.
 ![reglas predeterminadas](media/how-to-connect-fix-default-rules/default3.png)

- El cambio en el filtro de ámbito puede provocar la eliminación de objetos en el directorio de destino. Tenga cuidado antes de realizar cambios en el ámbito de los objetos. Se recomienda realizar cambios en un servidor de ensayo antes de realizar cambios en el servidor activo.
- Ejecute una vista previa en un objeto único como se mencionó en [validar regla de sincronización](#validate-sync-rule) sección después de agregar cualquier nueva regla.
- Ejecute una sincronización completa después de agregar nuevas reglas o modificar cualquier regla de sincronización personalizadas. Esta sincronización aplicará nuevas reglas para todos los objetos.

## <a name="changing-attribute-flow"></a>Cambiar el flujo de atributos
Hay 3 escenarios diferentes para el flujo de atributos, echemos un vistazo a cómo lograrlo sin modificar las reglas predeterminadas estándar.
- Agregar nuevo atributo
- Reemplazar el valor del atributo existente
- No se sincronizan atributo existente

### <a name="adding-new-attribute"></a>Agregar nuevo atributo:
Si encuentra que un atributo no fluyen desde el directorio de origen en el directorio de destino, a continuación, puede usar el [Azure AD Connect sync: Extensiones de directorio](how-to-connect-sync-feature-directory-extensions.md) fluya los nuevos atributos.

Tenga en cuenta que debe ser la primera opción usar [Azure AD Connect sync: Extensiones de directorio](how-to-connect-sync-feature-directory-extensions.md), fuera de la característica de cuadro proporcionada por Azure AD Connect. Sin embargo, si no funciona a continuación, vaya a través de los pasos siguientes para pasar un atributo sin modificar regla de sincronización predeterminada estándar existente, puede hacerlo mediante la adición de dos nuevas reglas de sincronización.


#### <a name="add-an-inbound-sync-rule"></a>Agregar una regla de sincronización de entrada:
Regla de sincronización entrante significa que el origen para el atributo es un espacio conector y el destino es el metaverso. Por ejemplo, para enviar un nuevo atributo de Active Directory local a Azure Active Directory, cree una nueva regla de sincronización de entrada iniciando la **Editor de reglas de sincronización**, a continuación, seleccione la dirección como **entrante** y haga clic en **agregar nueva regla**. 

 ![reglas predeterminadas](media/how-to-connect-fix-default-rules/default3a.png)

Siga su propia convención de nomenclatura para el nombre de la regla, aquí usamos **personalizado In from AD - usuario**, esto significa que la regla sea una regla personalizada y una regla de entrada desde el espacio del conector de AD en el metaverso.   

 ![reglas predeterminadas](media/how-to-connect-fix-default-rules/default3b.png)

Asigne a su propia descripción de la regla para que sea fácil, como cuál es el objetivo de esta regla y por qué se necesita el mantenimiento futuro de la regla.
Seleccione un sistema conectado (el bosque): el origen de atributo. A continuación, seleccione el tipo de objeto de sistema conectado y el tipo de objeto de metaverso.

Especifique el valor de prioridad entre 0 y 99 (reducir el número, mayor precedencia). Mantenga los demás campos como 'Etiqueta', "Habilitar sincronización de contraseñas" y "Disabled" como valor predeterminado.

Mantener 'filtro de ámbito' vacío, esto significa que la regla se aplicará a todos los objetos que se unen entre el sistema conectado de AD y el metaverso.

Mantener el vacío 'Reglas de unión', lo que significa que esta regla se se incluyen en la condición de combinación definida en la regla predeterminada estándar. Este es otro motivo para no deshabilitar/eliminar la regla predeterminada estándar porque si no hay ninguna condición de combinación se incluyen a continuación, el atributo no fluirá. 

Agregar transformación adecuado para el atributo, puede asignar constante para pasar un valor constante para el atributo de destino, o dirigir la asignación entre el atributo de origen o destino, o una expresión para el atributo. Estos son distintos [funciones de expresión](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-sync-functions-reference) puede usar.

#### <a name="add-an-outbound-sync-rule"></a>Agregar una regla de sincronización de salida:
Hasta ahora agregando simplemente una regla de sincronización de entrada que hemos realizado la mitad del trabajo, porque el atributo todavía no está vinculado al directorio de destino. Para vincular el atributo con el director de destino, deberá crear una regla de salida, lo que significa el origen es el metaverso y el destino es el sistema conectado. Para crear una regla de salida, inicie **Editor de reglas de sincronización**, cambie el **dirección** a **saliente** y haga clic en **agregar nueva regla**. 

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default3c.png)

Como se muestra en la regla de entrada, puede usar su propia convención de nomenclatura para **nombre** la regla. Seleccione el **sistema conectado** como inquilino de Azure AD, seleccione el objeto de sistema conectado a la que desea establecer el valor del atributo. Establecer la prioridad entre 0 - 99. 

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default3d.png)

Mantener **filtro de ámbito** keep vacío, **reglas de unión** vacío, rellene la transformación como constante, directa o expresión. 

En este ejemplo hemos mostrado cómo pasar un atributo nuevo para un objeto de usuario de Active Directory a Azure Active Directory. Puede utilizar estos pasos para asignar cualquier atributo de cualquier objeto de origen y destino.  Para obtener más información, consulte [crear reglas de sincronización personalizadas](how-to-connect-create-custom-sync-rule.md) y [preparar para aprovisionar usuarios](https://docs.microsoft.com/office365/enterprise/prepare-for-directory-synchronization).

### <a name="overriding-value-of-existing-attribute"></a>Reemplazar el valor del atributo existente
Es posible que desea reemplazar el valor del atributo ya están asignada, por ejemplo siempre desee establecer el valor Null a un atributo en Azure AD, para ello, basta con crear solo una regla de entrada como se mencionó en el paso anterior y el flujo  **AuthoritativeNull** valor constante para el atributo de destino. Tenga en cuenta que hemos usado AuthoritativeNulll en lugar de Null en este caso. Esto es porque el valor null no reemplazará el valor Null incluso si tiene una prioridad más baja (valor numérico más alto en la regla). Sin embargo, el AuthoritativeNull se tratará como Null y no se reemplazará con el valor no nulo por otras reglas. 

### <a name="dont-sync-existing-attribute"></a>No se sincronizan atributo existente
Si desea excluir un atributo de la sincronización, a continuación, puede usar el atributo filtrado característica proporcionada en Azure AD Connect. Iniciar **Azure AD Connect** desde el icono del escritorio y, a continuación, seleccione **personalizar las opciones de sincronización**.

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default4.png)

 Asegúrese de que **aplicación de Azure AD y el filtrado de atributos** está seleccionada y haga clic en **siguiente**.

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default5.png)

Desactive los atributos que desea excluir de la sincronización.

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default6a.png)

## <a name="changing-scoping-filter"></a>Cambiar filtro de ámbito
Sincronización de Azure AD se encarga de la mayoría de los objetos, puede reducir el ámbito de los objetos y reducirlo menos objetos se pueda exportar de manera compatible sin cambiar las reglas de sincronización predeterminado estándar. En caso de que desea aumentar el ámbito de los objetos se pueden **editar** la regla existente, clonarlo y deshabilite la regla original. Microsoft recomienda que no aumente el ámbito configurado por Azure AD Connect. Aumento en el ámbito de objetos hará que sea difícil para el equipo de soporte técnico a entender las personalizaciones y soporte técnico del producto.

Aquí es cómo puede reducir el ámbito de los objetos que se sincronizan con Azure AD. Tenga en cuenta que si reduce el ámbito de la **usuarios** sincronizadas, a continuación, también se detendrá la sincronización de hash de contraseña para los usuarios de filtrado horizontal. Si, a continuación, los objetos ya se sincronización después de reducir el ámbito, los objetos de filtrado de salida desde el directorio de destino se eliminará, trabaje en el ámbito con mucho cuidado.
Estas son las formas compatibles para reducir el alcance de los objetos que se están sincronizando.

- [atributo cloudFiltered](#cloudfiltered-attribute)
- [Filtrado por unidad organizativa](#ou-filtering)

### <a name="cloudfiltered-attribute"></a>atributo cloudFiltered
Tenga en cuenta que esto no es un atributo que se puede establecer en Active Directory. Debe establecer el valor de este atributo mediante la adición de una nueva regla de entrada como se mencionó en **reemplazar el valor del atributo existente** sección. A continuación, puede usar el **transformación** y usar **expresión** para establecer este atributo en el metaverso. Este es un ejemplo que no desea sincronizar todo del usuario cuyo nombre de departamento empieza con mayúsculas y minúsculas **HRD**:

`cloudFiltered <= IIF(Left(LCase([department]), 3) = "hrd", True, NULL)`

En primer lugar hemos convertido a minúsculas del departamento de origen (Active Directory). A continuación, usar la función Left, se tardó solo primero 3 caracteres y lo compara con hrd. Si encuentran coincidencias, a continuación, establezca el valor en NULL en caso contrario, True. Tenga en cuenta que estamos estableciendo el valor NULL, por lo que otra regla con prioridad más baja (valor numérico más alto) puede escribir en él con una condición diferente. Vuelva a ejecutar en un objeto para validar la regla de sincronización como se mencionó en versión preliminar [validar regla de sincronización](#validate-sync-rule) sección.

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default7a.png)



### <a name="ou-filtering"></a>Filtrado por unidad organizativa
Puede crear una o varias unidades organizativas y mover los objetos que no desea sincronizar a las unidades organizativas. A continuación, configure la unidad organizativa filtrado en Azure AD Connect iniciando **Azure AD Connect** en el icono del escritorio y seleccione las opciones tal como se muestra a continuación. También puede configurar la unidad organizativa de filtrado en el momento de la instalación de Azure AD Connect. 

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default8.png)

Siga los pasos del asistente y, a continuación, anule la selección de las unidades organizativas que no desee sincronizar.

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default9.png)

## <a name="changing-join-condition"></a>Cambiar la condición de combinación
Microsoft recomienda que use el valor predeterminado unir condiciones configurados por Azure AD Connect. Cambiar las condiciones de combinación predeterminado resultará difícil para el equipo de soporte técnico a entender las personalizaciones y soporte técnico del producto.

## <a name="validate-sync-rule"></a>Validar la regla de sincronización
Puede validar la regla de sincronización recién agregado mediante la característica de vista previa sin ejecutar el ciclo de sincronización completo. Iniciar **servicio de sincronización** la interfaz de usuario.

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default10.png)

Haga clic en el **búsqueda de metaverso**, seleccione el objeto de ámbito como **persona**, **Agregar cláusula** y mencionar los criterios de búsqueda. Haga clic en **búsqueda** botón y haga doble clic en el objeto en el **los resultados de búsqueda** tenga en cuenta que se ejecuta la importación y sincronización en el bosque antes de ejecutar este paso, esto es para asegurarse de que los datos en Azure AD Connect está actualizada para ese objeto.

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default11.png)



Seleccione **conectores**, seleccione el objeto en connector(forest) correspondiente, haga clic en **propiedades...** .

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default12.png)

Haga clic en el **vista previa...**

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default13a.png)

Haga clic en **generar vista previa** y **Importar flujo de atributo** en el panel izquierdo.

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default14.png)
 
Aquí observará que la regla recién agregada se ejecuta en el objeto y ha establecido el atributo cloudFiltered en True.

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default15a.png)
 
¿Cómo se comparan de reglas modificado con la regla predeterminada?
Puede exportar tanto las reglas por separado como archivos de texto. Estas reglas se exportarán como archivo de script de powershell. Puede compararlas mediante cualquier herramienta de comparación de archivos para ver qué tipo de cambios se realizan. Aquí en este ejemplo usé windiff para comparar dos archivos.
 
Puede observar que en el usuario puede modificar regla, se cambia el atributo msExchMailboxGuid a **expresión** en lugar del tipo **directo** con el valor como **NULL** y  **ExecuteOnce** opción. Puede omitir el identificador y la prioridad de las diferencias. 

![reglas predeterminadas](media/how-to-connect-fix-default-rules/default17.png)
 
¿Cómo corregir una regla predeterminada modificado?
Para corregir las reglas a la configuración predeterminada, puede eliminar la regla modificada y habilitar la regla predeterminada como se muestra a continuación y, a continuación, ejecute un **sincronización completa**. Antes de hacer que tome medidas correctivas, como se mencionó anteriormente, para que no pierda la personalización está intentando lograr ## los pasos siguientes

## <a name="next-steps"></a>Pasos siguientes
- [Hardware y requisitos previos](how-to-connect-install-prerequisites.md) 
- [Configuración rápida](how-to-connect-install-express.md)
- [Configuración personalizada](how-to-connect-install-custom.md)

