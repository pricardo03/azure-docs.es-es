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
ms.openlocfilehash: d2f0956b44d6df64fb73e5eee7844574237d8755
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067634"
---
# <a name="fix-modified-default-rules-in-azure-ad-connect"></a>Corregir las reglas predeterminadas modificadas en Azure AD Connect

Azure Active Directory (Azure AD) Connect utiliza las reglas predeterminadas para la sincronización.  Desafortunadamente, estas reglas no se aplican universalmente a todas las organizaciones. Según sus requisitos, es posible que deba modificarlas. En este artículo se describe dos ejemplos de las personalizaciones más comunes y se explica la manera correcta para lograr estas personalizaciones.

>[!NOTE] 
> No se admite la modificación de las reglas predeterminadas existentes para lograr una personalización necesaria. Si lo hace, por lo que impide actualizar estas reglas a la versión más reciente en futuras versiones. No obtendrá las correcciones de errores que necesita, o nuevas características. Este documento explica cómo lograr el mismo resultado sin modificar las reglas predeterminadas existentes. 

## <a name="how-to-identify-modified-default-rules"></a>Cómo identificar las reglas predeterminadas modificadas
A partir de la versión 1.3.7.0 de Azure AD Connect, es fácil de identificar la regla predeterminada modificada. Vaya a **las aplicaciones de escritorio**y seleccione **Synchronization Rules Editor**.

![Azure AD Connect, con resaltado de Editor de reglas de sincronización](media/how-to-connect-fix-default-rules/default1.png)

En el Editor, se muestran las reglas predeterminadas modificadas con un icono de advertencia delante del nombre.

![Icono Advertencia](media/how-to-connect-fix-default-rules/default2.png)

 Deshabilitado regla con el mismo nombre junto a también aparece (esta es la regla predeterminada estándar).

![Editor de reglas de sincronización, que muestra la regla predeterminada estándar y la regla predeterminada modificada](media/how-to-connect-fix-default-rules/default2a.png)

## <a name="common-customizations"></a>Personalizaciones comunes
Estas son las personalizaciones comunes a las reglas predeterminadas:

- Cambiar el flujo de atributos
- Cambiar filtro de ámbito
- Condición de combinación de cambio

Antes de cambiar las reglas:

- Deshabilitar al programador de sincronización. El programador se ejecuta cada 30 minutos de forma predeterminada. Asegúrese de que no se inicia mientras realizar cambios y solución de problemas de las nuevas reglas. Para deshabilitar temporalmente el programador, inicie PowerShell y ejecute `Set-ADSyncScheduler -SyncCycleEnabled $false`.
 ![Comandos de PowerShell para deshabilitar al programador de sincronización](media/how-to-connect-fix-default-rules/default3.png)

- El cambio en el filtro de ámbito puede provocar la eliminación de objetos en el directorio de destino. Tenga cuidado antes de realizar cambios en el ámbito de los objetos. Se recomienda que realizar cambios en un servidor de ensayo antes de realizar cambios en el servidor activo.
- Ejecute una vista previa en un único objeto, como se mencionó en el [validar regla de sincronización](#validate-sync-rule) sección después de agregar cualquier nueva regla.
- Ejecute una sincronización completa después de agregar nuevas reglas o modificar cualquier regla de sincronización personalizadas. Esta sincronización aplica a las nuevas reglas a todos los objetos.

## <a name="change-attribute-flow"></a>Cambiar el flujo de atributos
Hay tres escenarios diferentes para cambiar el flujo de atributos:
- Agregar un nuevo atributo.
- Invalidar el valor de un atributo existente.
- Si decide no sincronizar un atributo existente.

Puede hacer estas sin modificar las reglas predeterminadas estándar.

### <a name="add-a-new-attribute"></a>Agregar un nuevo atributo
Si encuentra que un atributo no fluyen desde el directorio de origen en el directorio de destino, utilice el [Azure AD Connect sync: Extensiones de directorio](how-to-connect-sync-feature-directory-extensions.md) para solucionar este problema.

Si las extensiones no funcionan, pruebe a agregar dos nuevas reglas de sincronización, que se describe en las secciones siguientes.


#### <a name="add-an-inbound-sync-rule"></a>Agregar una regla de sincronización de entrada
Una regla de sincronización de entrada, significa que el origen para el atributo es un espacio conector y el destino es el metaverso. Por ejemplo tener un flujo de un nuevo atributo en Active Directory local a Azure Active Directory, cree una nueva regla de sincronización de entrada. Iniciar el **Synchronization Rules Editor**, seleccione **entrante** como la dirección y seleccione **agregar nueva regla**. 

 ! Sincronización reglas Editor](media/how-to-connect-fix-default-rules/default3a.png)

Siga su propia convención de nomenclatura para el nombre de la regla. En este caso, usamos **personalizado In from AD - usuario**. Esto significa que la regla es una regla personalizada y es una regla de entrada desde el espacio del conector de Active Directory al metaverso.   

 ![Crear regla de sincronización de entrada](media/how-to-connect-fix-default-rules/default3b.png)

Proporcionar su propia descripción de la regla, por lo que es fácil mantenimiento futuro de la regla. Por ejemplo, la descripción puede basarse en ¿qué es el objetivo de la regla, y por qué se necesita.

Realice las selecciones la **sistema conectado**, **tipo de objeto de sistema conectado**, y **tipo de objeto de metaverso** campos.

Especifique el valor de prioridad entre 0 y 99 (menor es el número, mayor será la prioridad). Para el **etiqueta**, **Habilitar sincronización de contraseñas**, y **deshabilitado** campos, use las selecciones predeterminadas.

Mantener **filtro de ámbito** vacía. Esto significa que la regla se aplica a todos los objetos se une entre el sistema conectado de Active Directory y el metaverso.

Mantener **reglas de unión** vacía. Esto significa que esta regla usa la condición de combinación definida en la regla predeterminada estándar. Este es otro motivo para no deshabilitar o eliminar la regla predeterminada estándar. Si no hay ninguna condición de combinación, no fluirá el atributo. 

Agregar transformaciones adecuadas para el atributo. Puede asignar una constante, para realizar una constante de valor de flujo para el atributo de destino. Puede usar la asignación directa entre el atributo de origen o destino. O bien, puede usar una expresión para el atributo. Estos son distintos [funciones de expresión](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-sync-functions-reference) puede usar.

#### <a name="add-an-outbound-sync-rule"></a>Agregar una regla de sincronización de salida
Para vincular el atributo en el directorio de destino, deberá crear una regla de salida. Esto significa que el origen es el metaverso y el destino es el sistema conectado. Para crear una regla de salida, inicie el **Synchronization Rules Editor**, cambie el **dirección** a **saliente**y seleccione **agregar nueva regla**. 

![Editor de reglas de sincronización](media/how-to-connect-fix-default-rules/default3c.png)

Como con la regla de entrada, puede usar su propia convención de nomenclatura para el nombre de la regla. Seleccione el **sistema conectado** como el inquilino de Azure AD y seleccione el sistema conectado el objeto al que desea establece el valor del atributo. Establecer la prioridad entre 0 y 99. 

![Crear regla de sincronización de salida](media/how-to-connect-fix-default-rules/default3d.png)

Mantener **filtro de ámbito** y **reglas de unión** vacía. Rellene la transformación como constante, directa o expresión. 

Ahora que sabe cómo crear un nuevo atributo para un flujo de objeto de usuario desde Active Directory a Azure Active Directory. Puede utilizar estos pasos para asignar cualquier atributo de cualquier objeto de origen y destino. Para obtener más información, consulte [crear reglas de sincronización personalizadas](how-to-connect-create-custom-sync-rule.md) y [preparar para aprovisionar usuarios](https://docs.microsoft.com/office365/enterprise/prepare-for-directory-synchronization).

### <a name="override-the-value-of-an-existing-attribute"></a>Invalidar el valor de un atributo existente
Es posible que desee invalidar el valor de un atributo que ya se ha asignado. Por ejemplo, si siempre desea establecer un valor null a un atributo en Azure AD, basta con crear una regla de entrada. Hacer que el valor constante, `AuthoritativeNull`, flujo para el atributo de destino. 

>[!NOTE] 
> Use `AuthoritativeNull` en lugar de `Null` en este caso. Esto es porque el valor no null reemplaza el valor null, incluso si tiene una prioridad más baja (un número mayor valor en la regla). `AuthoritativeNull`, por otro lado, no se reemplaza con un valor distinto de null por otras reglas. 

### <a name="dont-sync-existing-attribute"></a>No se sincronizan atributo existente
Si desea excluir un atributo de la sincronización, utilice el atributo filtrado característica proporcionada en Azure AD Connect. Iniciar **Azure AD Connect** desde el icono de escritorio y, a continuación, seleccione **personalizar las opciones de sincronización**.

![Opciones de tareas adicionales de Azure AD Connect](media/how-to-connect-fix-default-rules/default4.png)

 Asegúrese de que **aplicación de Azure AD y el filtrado de atributos** está seleccionado y, a continuación, seleccione **siguiente**.

![Características opcionales de Azure AD Connect](media/how-to-connect-fix-default-rules/default5.png)

Borrar los atributos que desea excluir de la sincronización.

![Atributos de Azure AD Connect](media/how-to-connect-fix-default-rules/default6a.png)

## <a name="change-scoping-filter"></a>Cambiar filtro de ámbito
Sincronización de Azure AD se encarga de la mayoría de los objetos. Puede reducir el ámbito de objetos y reducir el número de objetos que se exportarán, sin cambiar las reglas de sincronización predeterminado estándar. 

Use uno de los métodos siguientes para reducir el alcance de los objetos que se va a sincronizar:

- atributo cloudFiltered
- Filtrado de unidad de organización

Si reduce el ámbito de los usuarios que se sincronizan, la sincronización de hash de contraseña deja también para los usuarios de filtrado horizontal. Si ya se están sincronizando los objetos, después de reducir el ámbito, se eliminan los objetos filtrados horizontalmente desde el directorio de destino. Por este motivo, asegúrese de ámbito con mucho cuidado.

>[!IMPORTANT] 
> No se recomienda aumentar el ámbito de los objetos configurado por Azure AD Connect. Si lo hace, dificulta que el equipo de soporte técnico de Microsoft entender las personalizaciones. Si debe aumentar el ámbito de los objetos, edite la regla existente, clonarlo y deshabilite la regla original. 

### <a name="cloudfiltered-attribute"></a>atributo cloudFiltered
No se puede establecer este atributo en Active Directory. Establezca el valor de este atributo mediante la adición de una nueva regla de entrada. A continuación, puede usar **transformación** y **expresión** para establecer este atributo en el metaverso. El ejemplo siguiente se muestra que no desea sincronizar todos los usuarios cuyo nombre de departamento empieza con **HRD** (mayúsculas y minúsculas):

`cloudFiltered <= IIF(Left(LCase([department]), 3) = "hrd", True, NULL)`

En primer lugar convertimos el departamento de origen (Active Directory) a minúsculas. A continuación, usando la `Left` función, se tardó solo los tres primeros caracteres y se compara con `hrd`. Si coincide, el valor se establece en `True`en caso contrario, `NULL`. En el valor null, puede escribir otra regla con prioridad más baja (un valor numérico más alto) en él con una condición diferente. Vista previa de ejecución en un objeto para validar la regla de sincronización, como se mencionó en el [validar regla de sincronización](#validate-sync-rule) sección.

![Crear opciones de regla de sincronización de entrada](media/how-to-connect-fix-default-rules/default7a.png)

### <a name="organizational-unit-filtering"></a>Filtrado de unidad organizativa
Puede crear una o varias unidades organizativas (OU) y mover los objetos que no desea sincronizar a las unidades organizativas. A continuación, configure la unidad organizativa filtrado en Azure AD Connect. Iniciar **Azure AD Connect** desde el icono del escritorio y seleccione las opciones siguientes. También puede configurar la unidad organizativa de filtrado en el momento de la instalación de Azure AD Connect. 

![Tareas adicionales de Azure AD Connect](media/how-to-connect-fix-default-rules/default8.png)

Siga los pasos del asistente y desactive las unidades organizativas que no desee sincronizar.

![Dominio de Active Directory Connect y opciones de filtrado por unidad organizativa de Azure](media/how-to-connect-fix-default-rules/default9.png)

## <a name="change-join-condition"></a>Condición de combinación de cambio
Utilice las condiciones de combinación predeterminado configuradas por Azure AD Connect. Cambiar las condiciones de combinación predeterminada dificulta a soporte técnico de Microsoft entender las personalizaciones y soporte técnico del producto.

## <a name="validate-sync-rule"></a>Validar la regla de sincronización
Puede validar la regla de sincronización recién agregado mediante la característica de vista previa, sin ejecutar el ciclo de sincronización completa. En Azure AD Connect, seleccione **Synchronization Service**.

![Azure AD Connect, con el servicio de sincronización resaltado](media/how-to-connect-fix-default-rules/default10.png)

Seleccione **búsqueda de metaverso**. Seleccione el objeto de ámbito como **persona**, seleccione **Agregar cláusula**y mencionar los criterios de búsqueda. A continuación, seleccione **búsqueda**y haga doble clic en el objeto en los resultados de búsqueda. Asegúrese de que los datos en Azure AD Connect están actualizados para ese objeto, mediante la ejecución de importación y sincronización en el bosque antes de ejecutar este paso.

![Synchronization Service Manager](media/how-to-connect-fix-default-rules/default11.png)

En **propiedades del objeto de metaverso**, seleccione **conectores**, seleccione el objeto en el conector correspondiente (bosque) y seleccione **propiedades...** .

![Metaverse Object Properties](media/how-to-connect-fix-default-rules/default12.png)

Seleccione **vista previa...**

![Propiedades de objeto del espacio del conector](media/how-to-connect-fix-default-rules/default13a.png)

En la ventana de vista previa, seleccione **generar vista previa** y **Importar flujo de atributo** en el panel izquierdo.

![Vista previa](media/how-to-connect-fix-default-rules/default14.png)
 
En este caso, tenga en cuenta que se ejecuta en el objeto de la regla recién agregada y ha establecido la `cloudFiltered` atributo en true.

![Vista previa](media/how-to-connect-fix-default-rules/default15a.png)
 
Para comparar la regla modificada con la regla predeterminada, exporte tanto de las reglas por separado, como archivos de texto. Estas reglas se exportan como un archivo de script de PowerShell. Puede compararlas mediante cualquier herramienta de comparación de archivos (por ejemplo, windiff) para ver los cambios. 
 
Tenga en cuenta que en la regla modificada, el `msExchMailboxGuid` atributo se cambia a la **expresión** tipo, en lugar de **directo**. Además, se cambia el valor a **NULL** y **ExecuteOnce** opción. Puede omitir el identificador y la prioridad de las diferencias. 

![resultados de la herramienta de WinDiff](media/how-to-connect-fix-default-rules/default17.png)
 
Para corregir las reglas para cambiarlas a la configuración predeterminada, elimine la regla modificada y habilitar la regla predeterminada. Asegúrese de que no se pierda la personalización que está intentando lograr. Cuando esté listo, ejecute **sincronización completa**.

## <a name="next-steps"></a>Pasos siguientes
- [Hardware y requisitos previos](how-to-connect-install-prerequisites.md) 
- [Configuración rápida](how-to-connect-install-express.md)
- [Configuración personalizada](how-to-connect-install-custom.md)



