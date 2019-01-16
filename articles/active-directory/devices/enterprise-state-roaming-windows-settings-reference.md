---
title: Referencia de la configuración de itinerancia de Windows 10 | Microsoft Docs
description: Una lista completa de todas las opciones que se movilizan o de las que se realiza una copia de seguridad en Windows 10.
services: active-directory
keywords: enterprise state roaming, nube de windows
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: curtand
ms.component: devices
ms.assetid: 17cffc3e-2928-4235-91f7-a685bd6bdcbf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: markvi
ms.openlocfilehash: 45bf554e457e48704684fe8ac3b32c29f3f646b2
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54189291"
---
# <a name="windows-10-roaming-settings-reference"></a>Referencia de la configuración de movilidad de Windows 10
Lo siguiente es una lista completa de todas las opciones que se movilizan o de las que se realiza una copia de seguridad en Windows 10. 

## <a name="devices-and-endpoints"></a>Interfaces y puntos de conexión
Consulte la tabla siguiente para ver un resumen de los dispositivos y tipos de cuenta compatibles con el marco de sincronización, copia de seguridad y restauración en Windows 10.

| Tipo de cuenta y operación | Escritorio | Móvil |
| --- | --- | --- |
| Azure Active Directory: sincronización |SÍ |Sin  |
| Azure Active Directory: copia de seguridad/restauración |Sin  |Sin  |
| Cuenta de Microsoft: sincronización |SÍ |SÍ |
| Cuenta de Microsoft: copia de seguridad/restauración |Sin  |SÍ |

## <a name="what-is-backup"></a>¿Qué es una copia de seguridad?
La configuración de Windows se sincroniza normalmente de forma predeterminada pero de algunas configuraciones solo se realiza una copia de seguridad, como la lista de aplicaciones instaladas en un dispositivo. La característica Backup está destinada solo para dispositivos móviles y no está disponible actualmente para los usuarios de Enterprise State Roaming. Backup utiliza una cuenta Microsoft y almacena la configuración y los datos de aplicación en OneDrive. Si un usuario deshabilita la sincronización en el dispositivo mediante la aplicación Configuración, los datos de la aplicación que se suelen sincronizar se convierten en datos de copia de seguridad solamente. Solo se puede tener acceso a los datos de copia de seguridad mediante la operación de restauración durante la primera experiencia de ejecución de un nuevo dispositivo. Las copias de seguridad se pueden deshabilitar mediante la configuración del dispositivo, y se pueden administrar y eliminar mediante la cuenta OneDrive del usuario.

## <a name="windows-settings-overview"></a>Introducción a la configuración de Windows
Los grupos de configuración siguientes están disponibles para que los usuarios finales puedan habilitar o deshabilitar en ellos la sincronización de configuración en dispositivos de Windows 10.

* Tema: fondo de escritorio, icono de usuario, posición de la barra de tareas, etc. 
* Configuración de Internet Explorer: historial de exploración, direcciones URL escritas, favoritos, etc. 
* Contraseñas: administrador de credenciales de Windows, incluidos los perfiles Wi-Fi 
* Preferencias de idioma: diccionario de ortografía, configuración de idioma del sistema 
* Facilidad de acceso: narrador, teclado en pantalla, lupa 
* Otras configuraciones de Windows: consultar Detalles de configuración de Windows
* Configuración del explorador Microsoft Edge: favoritos de Microsoft Edge, lista de lectura y otras opciones

![](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-syncyoursettings.png)

Es posible habilitar o deshabilitar la sincronización de un grupo de configuración del explorador Microsoft Edge a través de la opción de menú Configuración del explorador Microsoft Edge.

![](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-edge.png)

Para Windows 10 versión 1803 o posterior, la sincronización del grupo de configuración de Internet Explorer (favoritos, direcciones URL escritas) puede ser habilitada o deshabilitada por los usuarios finales mediante la opción del menú de configuración de Internet Explorer. 

![](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-ie.png)

## <a name="windows-settings-details"></a>Detalles de configuración de Windows
En la tabla siguiente, las entradas Otros en la columna Grupo de la configuración se refieren a configuraciones que se pueden deshabilitar en Configuración > Cuentas > Sincronizar la configuración > Otras configuraciones de Windows. 

Las entradas Interno de la columna Grupo de la configuración hacen referencia a la configuración y las aplicaciones que solo se pueden deshabilitar de la sincronización dentro de la propia aplicación o al deshabilitar la sincronización para todo el dispositivo mediante la administración de dispositivos móviles (MDM) o la configuración de directivas de grupo.
Las configuraciones que no se movilizan o sincronizan no pertenecerán a ningún grupo.

| Configuración | Escritorio | Móvil | Grupo |
| --- | --- | --- | --- |
| **Cuentas**: imagen de la cuenta |sync |X |Tema |
| **Cuentas**: otras configuraciones de la cuenta |X |X | |
| **Ancho de banda móvil avanzado**: conexión a Internet que comparte el nombre de red (permite la detección automática de zonas con cobertura inalámbrica móvil a través de Bluetooth) |X |X |Contraseñas |
| **Datos de la aplicación**: las aplicaciones individuales pueden sincronizar datos |sincronización copia de seguridad |sincronización copia de seguridad |interno |
| **Lista de aplicaciones**: lista de aplicaciones instaladas |X |backup |Otros |
| **Bluetooth**: toda la configuración de Bluetooth |X |X | |
| **Símbolo del sistema**: configuración de los "valores predeterminados" del símbolo del sistema |sync |X |interno |
| **Credenciales**: Caja de seguridad de credenciales |sync |sync |contraseña |
| **Fecha, hora y región**: hora automática (sincronización de hora de Internet) |sync |sync |language |
| **Fecha, hora y región**: formato de 24 horas |sync |X |language |
| **Fecha, hora y región**: fecha y hora |sync |X |language |
| **Fecha, hora y región**: zona horaria | |X |language |
| **Fecha, hora y región**: horario de verano |sync |X |language |
| **Fecha, hora y región**: país o región |sync |X |language |
| **Fecha, hora y región**: primer día de la semana |sync |X |language |
| **Fecha, hora y región**: formato de región (configuración regional) |sync |X |language |
| **Fecha, hora y región**: fecha corta |sync |X |language |
| **Fecha, hora y región**: fecha larga |sync |X |language |
| **Fecha, hora y región**: hora corta |sync |X |language |
| **Fecha, hora y región**: hora larga |sync |X |language |
| **Personalización del escritorio**: tema de escritorio (fondo, color del sistema, sonidos del sistema predeterminados, protector de pantalla) |sync |X |Tema |
| **Personalización del escritorio**: papel tapiz de presentación |sync |X |Tema |
| **Personalización del escritorio**: configuración de la barra de tareas (posición, ocultar automáticamente, etc.) |sync |X |Tema |
| **Personalización del escritorio**: diseño de pantalla de inicio |X |backup | |
| **Dispositivos**: impresoras compartidas a las que se conecta |X |X |Otros |
| **Explorador Microsoft Edge**: lista de lectura |sync |sync |interno |
| **Explorador Microsoft Edge**: favoritos |sync |sync |interno |
| **Explorador Microsoft Edge**: sitios principales <sup>[[1]](#footnote-1)</sup> |sync |sync |interno |
| **Explorador Microsoft Edge**: direcciones URL escritas <sup>[[1]](#footnote-1)</sup> |sync |sync |interno |
| **Explorador Microsoft Edge**: configuración de la barra de favoritos <sup>[[1]](#footnote-1)</sup> |sync |sync |interno |
| **Explorador Microsoft Edge**: mostrar el botón inicio <sup>[[1]](#footnote-1)</sup> |sync |sync |interno |
| **Explorador Microsoft Edge**: bloquear elementos emergentes <sup>[[1]](#footnote-1)</sup> |sync |sync |interno |
| **Explorador Microsoft Edge**: preguntarme qué hacer en cada descarga <sup>[[1]](#footnote-1)</sup> |sync |sync |interno |
| **Explorador Microsoft Edge**: ofrecer guardar contraseñas <sup>[[1]](#footnote-1)</sup> |sync |sync |interno |
| **Explorador Microsoft Edge**: enviar solicitudes de no seguimiento <sup>[[1]](#footnote-1)</sup> |sync |sync |interno |
| **Explorador Microsoft Edge**: guardar entradas de formularios <sup>[[1]](#footnote-1)</sup> |sync |sync |interno |
| **Explorador Microsoft Edge**: mostrar sugerencias de búsqueda y de sitios web al escribir <sup>[[1]](#footnote-1)</sup> |sync |sync |interno |
| **Explorador Microsoft Edge**: preferencias de cookies <sup>[[1]](#footnote-1)</sup> |sync |sync |interno |
| **Explorador Microsoft Edge**: permitir que los sitios guarden licencias multimedia protegidas en mi dispositivo <sup>[[1]](#footnote-1)</sup> |sync |sync |interno |
| **Explorador Microsoft Edge**: configuración del lector de pantalla <sup>[[1]](#footnote-1)</sup> |sync |sync |interno |
| **Contraste alto**: On (Activado) u Off (Desactivado) |sync |X |Facilidad de acceso |
| **Contraste alto**: Configuración de tema |sync |X |Facilidad de acceso |
| **Internet Explorer**: abrir pestañas (dirección URL y título) |sync |sync |Internet Explorer |
| **Internet Explorer**: lista de lectura |sync |sync |Internet Explorer |
| **Internet Explorer**: direcciones URL escritas |sync |sync |Internet Explorer |
| **Internet Explorer**: historial de exploración |sync |sync |Internet Explorer |
| **Internet Explorer**: favoritos |sync |sync |Internet Explorer |
| **Internet Explorer**: direcciones URL excluidas |sync |sync |Internet Explorer |
| **Internet Explorer**: páginas de inicio |sync |sync |Internet Explorer |
| **Internet Explorer**: sugerencias de dominio |sync |sync |Internet Explorer |
| **Teclado**: los usuarios pueden activar o desactivar el teclado en pantalla |sync |X |Facilidad de acceso |
| **Teclado**: activar sí temporal (desactivado de forma predeterminada) |sync |X |Facilidad de acceso |
| **Teclado**: activar teclas de filtro (desactivado de forma predeterminada) |sync |X |Facilidad de acceso |
| **Teclado**: activar teclas de alternancia (desactivado de forma predeterminada) |sync |X |Facilidad de acceso |
| **Internet Explorer**: idioma de dominio: QWERTY chino simplificado - habilitar autoaprendizaje |sync |X |Idioma |
| **Idioma**: QWERTY chino simplificado - habilitar clasificación dinámica del candidato |sync |X |Idioma |
| **Idioma**: QWERTY chino simplificado - juego de caracteres de chino simplificado |sync |X |Idioma |
| **Idioma**: QWERTY chino simplificado - juego de caracteres de chino tradicional |sync |X |Idioma |
| **Idioma**: QWERTY chino simplificado - pinyin aproximado |sync |backup |Idioma |
| **Idioma**: QWERTY chino simplificado - pares aproximados |sync |backup |Idioma |
| **Idioma**: QWERTY chino simplificado - pinyin completo |sync |X |Idioma |
| **Idioma**: QWERTY chino simplificado - pinyin doble |sync |X |Idioma |
| **Idioma**: QWERTY chino simplificado - corrección automática de lectura |sync |X |Idioma |
| **Idioma**: QWERTY chino simplificado - tecla de modificador C/E, MAYÚS |sync |X |Idioma |
| **Idioma**: QWERTY chino simplificado - tecla de modificador C/E, Ctrl |sync |X |Idioma |
| **Idioma**: WUBI chino simplificado - modo de entrada de carácter único |sync |X |Idioma |
| **Idioma**: WUBI chino simplificado - mostrar la codificación restante del candidato |sync |X |Idioma |
| **Idioma**: WUBI chino simplificado - pitido cuando la codificación 4 no sea válida |sync |X |Idioma |
| **Idioma**: Bopomofo chino simplificado, incluir CJK Ext-A |sync |X |Idioma |
| **Idioma**: IME japonés - escritura predictiva y palabras personalizadas |sync |sync |Idioma |
| **Idioma**: IME coreano |X |X |Idioma |
| **Idioma**: reconocimiento de escritura a mano |X |X |Idioma |
| **Idioma**: perfil de lenguaje |sync |backup |Idioma |
| **Idioma**: corrector ortográfico - autocorrección y resaltar errores ortográficos |sync |backup |Idioma |
| **Idioma**: lista de teclados |sync |backup |Idioma |
| **Pantalla de bloqueo**: toda la configuración de pantalla de bloqueo |X |X | |
| **Lupa**: activar o desactivar (alternancia de maestro) |X |X |Facilidad de acceso |
| **Lupa**: activar o desactivar inversión del color (desactivado de forma predeterminada) |sync |X |Facilidad de acceso |
| **Lupa**: seguimiento - seguimiento del foco del teclado |sync |X |Facilidad de acceso |
| **Lupa**: seguimiento - seguimiento del cursor del mouse |sync |X |Facilidad de acceso |
| **Lupa**: iniciar cuando los usuarios inicien sesión (desactivado de forma predeterminada) |sync |X |Facilidad de acceso |
| **Mouse**: cambiar el tamaño del cursor del mouse |sync |X |Otros |
| **Mouse**: cambiar el color del cursor del mouse |sync |X |Otros |
| **Mouse**: todas las demás configuraciones |X |X | |
| **Narrador**: inicio rápido |sync |X |Facilidad de acceso |
| **Narrador**: los usuarios pueden cambiar el tono de voz del narrador |sync |X |Facilidad de acceso |
| **Narrador**: los usuarios pueden activar o desactivar las sugerencias de lectura del narrador para los elementos comunes (activado de forma predeterminada) |sync |X |Facilidad de acceso |
| **Narrador**: los usuarios pueden activar o desactivar si pueden oír los caracteres escritos (activado de forma predeterminada) |sync |X |Facilidad de acceso |
| **Narrador**: los usuarios pueden activar o desactivar si pueden oír las palabras escritas (activado de forma predeterminada) |sync |X |Facilidad de acceso |
| **Narrador**: insertar cursor a continuación del narrador (activado de forma predeterminada) |sync |X |Facilidad de acceso |
| **Narrador**: habilitar resaltado visual del cursor del narrador (activado de forma predeterminada) |sync |X |Facilidad de acceso |
| **Narrador**: reproducir indicaciones de sonido (activado de forma predeterminada) |sync |X |Facilidad de acceso |
| **Narrador**: activar teclas del teclado táctil al levantar el dedo (desactivado de forma predeterminada) |sync |X |Facilidad de acceso |
| **Facilidad de acceso**: establecer el grosor del cursor intermitente |sync |X |Facilidad de acceso |
| **Facilidad de acceso**: quitar imágenes de fondo (desactivado de forma predeterminada) |sync |X |Facilidad de acceso |
| **Encendido y suspensión**: todas las configuraciones |X |X | |
| **Personalización de la pantalla de inicio**: color de acento (solo teléfono). |X |sync |Tema |
| **Escritura**: diccionario ortográfico |sync |backup |Idioma |
| **Escritura**: autocorrección de palabra escrita incorrectamente |sync |backup |Idioma |
| **Escritura**: resaltar palabras incorrectas |sync |backup |Idioma |
| **Escritura**: mostrar sugerencias de texto al escribir |sync |backup |Idioma |
| **Escritura**: agregar un espacio después de seleccionar una sugerencia de texto |sync |backup |Idioma |
| **Escritura**: agregar un punto al pulsar dos veces la barra espaciadora |sync |backup |Idioma |
| **Escritura**: en mayúsculas la primera letra de cada frase |sync |backup |Idioma |
| **Escritura**: escribir todo en mayúsculas al pulsar dos veces la tecla MAYÚS |sync |backup |Idioma |
| **Escritura**: reproducir sonidos de teclas al escribir |sync |backup |Idioma |
| **Escritura**: datos de personalización para teclado táctil |sync |backup |Idioma |
| **Wi-Fi**: perfiles Wi-Fi (solo WPA) |sync |sync |Contraseñas |

###### <a name="footnote-1"></a>Nota al pie 1
Versión mínima admitida de Windows Creators Update (Build 15063). 

## <a name="next-steps"></a>Pasos siguientes

Para obtener información general, consulte [Información general sobre Enterprise State Roaming](enterprise-state-roaming-overview.md).

