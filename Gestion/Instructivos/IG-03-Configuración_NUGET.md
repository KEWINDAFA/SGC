---
title: IG-03 Configuración NUGET
description: Instructivo gestión de la configuración de NuGets
published: true
date: 2023-04-05T16:13:08.859Z
tags: nuget, gestion de la configuracion, instructivos, ig-03, dependencias, librerias, paquetes
editor: markdown
dateCreated: 2023-04-04T19:44:54.567Z
---

# Descripción General

## 1. OBJETIVOS

Este instructivo tiene como propósito establecer la metodología para gestionar la configuración de los paquetes que son identificados como dependencias en un sistema de información .NET mediante la tecnología NUGET.

## 2. ALCANCE

Este instructivo es aplicable a las dependencias generadas para los proyectos de desarrollo .NET. Las actividades de gestión de configuración de las dependencias comprenden:

- Identificar las dependencias.
- Generar paquetes para las dependencias.
- Versionar los paquetes para las dependencias.
- Controlar el cambio de los paquetes.
- Almacenar los paquetes en el repositorio.
- Servir los paquetes en los IDE de desarrollo (Visual Studio).

## 3.	POLITICAS

Todos los productos de trabajo deben ser puestos bajo control del Sistema de Gestión de Configuración, cumpliendo con sus lineamientos con el fin de velar por su integridad.

## 4.	CONDICIONES GENERALES

- Se considera objeto de configuración para desarrollos WEB *(códigos fuentes de las páginas web, librerías javas, archivos .jar, librerías dll, librerías C#.)*{.caption} 
- Los desarrollos en ambiente web .NET se trabajan por proyectos los cuales son realizados en la herramienta VISUAL STUDIO.
- La gestión de configuración para proyectos WEB se realiza a través de SVN Subversión.
- Todo cambio que afecte o no la funcionalidad de un programa que se encuentre en producción debe generar una nueva versión. Cada vez que se genere un nuevo objeto de los sistemas, se debe definir su nombre según lo indicado en los instructivos ID-05 *`Construcción Software`*.
- Los backups de los objetos de configuración se realizan siguiendo el instructivo IA-04 `*Realización Backups`*.

## 5.	DOCUMENTOS REFERENCIADOS  

- [Instructivo ID-05 *Construcción Software*](/Analisis_Diseño/Instructivos/ID-05-Construcción_de_software.md)
- [Instructivo ID-09 *Construcción Software WEB*](/Analisis_Diseño/Instructivos/ID-09-Construcción_Software_WEB.md)
- [Instructivo IA-04 *Realización Backups*](/)
- [Guía GA-03 *Definiciones SGC*](/)
- [FD-14 *Arquitectura e Interfaces_GENNET*](/)
{.links-list}

# 6.	GESTIÓN DE PAQUETES NUGET

## 6.1.	Servidor de paquetes NUGET

Para la correcta gestión de configuración de los paquetes NUGET de ACTSIS, es necesario contar con un _servidor_ de paquetes NUGET como gestor de dependencias. El servidor es encargado de almacenar, controlar la publicación, listar y servir a los IDE como Visual Studio los paquetes NUGET. El servidor, el IDE y la configuración del paquete, intrínsecamente controlan la correcta instalación, actualización y eliminación de los paquetes con sus respectivas dependencias y versiones en cada proyecto .NET.

La creación y configuración del servidor así como la configuración en el IDE, se encuentra documentada en el repositorio SVN https://svn.actsis.com/svn/NUGET/NugetServer

## 6.2.	Identificar dependencias.

Las librerías, proyectos o contenidos desarrollados por ACTSIS o por un tercero, que son usados en otros proyectos .NET de ACTSIS, son identificados como dependencias.

Las dependencias que son desarrolladas por terceros, deben ser revisadas y aprobadas para uso en los proyectos .NET por parte del proyecto GENERAL. Estas librerías se gestionan por medio de un único paquete NUGET, teniendo en cuenta el ámbito de la librería.

> **3rosCore:** Corresponde al paquete NUGET que agrupa todas las librerías usadas a nivel de Core
{.is-info}

> **3rosWeb:** Corresponde al paquete NUGET que agrupa todas las librerías usadas a nivel de proyecto ASP .NET Web Forms.
{.is-info}

Los proyectos desarrollados por ACTSIS que son identificados como dependencias, son los siguientes:

## tipos {.tabset}

### Core <i class="mdi mdi-cpu-64-bit"></i>

Todos los proyectos identificados como **Core** de un sistema, teniendo en cuenta la definición Core de un proyecto .NET del documento **FD-14** Arquitectura e Interfaces_General .NET.

### GENNET <i class="mdi mdi-alpha-g-box"></i>
El proyecto **GENNET**, es identificado como dependencia base para cualquier proyecto ASP.NET Web Forms de ACTSIS, según el documento **FD-14** Arquitectura e Interfaces_General .NET.

### Otros <i class="mdi mdi-set-none"></i>
  
mdi mdi-set-none

## 6.3. Tipos de paquetes NUGET.

Teniendo en cuenta la identificación de las dependencias que son controladas con paquetes NUGET, se definen 2 tipos de paquetes NUGET para su generación.

## tipos_NuGets {.tabset}

### Paquetes NUGET de librería dll

Corresponden a los paquetes que gestionan las dependencias de proyectos .Net que solo generan librerías dll, el destino de estas librerías en los proyectos que la usan, es la carpeta **Bin**. Teniendo en cuenta que el paquete **3rosCore** entrega todas las librerías dll de terceros que son requeridas a nivel de Core, este es tipificado como paquete de librerías dll.

### Paquetes NUGET de contenido

Corresponden a los paquetes que gestionan cualquier tipo de archivo para ser ubicado dentro de la estructura del proyecto .NET. Estos paquetes pueden entregar código no compilado que debe ser compilado por el proyecto que lo usa. Para este caso se encuentra el paquete NUGET del proyecto GENNET, el cual entrega todo el contenido del proyecto ASP .NET Web Forms correspondiente a las pantallas, controles de usuario, scripts y otras clases C# que debe compilar el proyecto que lo usa. Teniendo en cuenta que el paquete 3rosWeb entrega todos los archivos, contenidos, scripts, dlls, xml y cualquier otro tipo de archivo que sea requerido a nivel de proyecto ASP .NET Web Forms desarrollado por un tercero, es tipificados como proyecto de contenido.

## 6.4.	Generar paquetes NUGET.

Para la generación del paquete NUGET se debe tener en cuenta las siguientes instrucciones.

### 6.4.1.	Identificador del paquete NUGET.

Cada paquete debe tener un identificador único en el lugar que se encuentre ubicado, el identificador no distingue entre mayúsculas y minúsculas. Los identificadores no pueden contener espacios ni caracteres no válidos para una dirección URL, por lo anterior, el identificador debe representar el proyecto del cual es generado cumpliendo los lineamientos anteriores.

Para el caso de los proyectos CORE, los cuales normalmente son identificados con la nomenclatura CORE_NOMBRE, el paquete NUGET seria identificado de igual manera pero sin el guion de piso.

Ejemplo:

> CoreSan, CoreSac, CoreGen, GENNET. 

### 6.4.2.	Versión del paquete NUGET.

Cada paquete debe tener una versión específica, que corresponde a un número en el formato **principal.secundaria.revisión[-sufijo]**, donde cada parte corresponde a lo siguiente:

- **Principal:** Versión aprobada del sistema, teniendo en cuenta el documento IG-01 Administración de la Configuración. 
- **Secundaria:** Versión liberada en un ROADMAP teniendo en cuenta el documento IG-01 Administración de la Configuración.
- **Revisión:** Corresponde al número de revisión en el repositorio de versiones Subversión del proyecto que generó el cambio.
- **-Sufijo:** Un guion seguido de una cadena para indicar que el paquete es una versión preliminar, las versiones preliminares se entienden como todas aquellas que no corresponden a un TAG de repositorio. En ACTSIS se usa entonces la cadena -rc (release candidate), para denotar una versión de paquete preliminar  generada de la rama tronco, por esto se dice que es preliminar. Si el paquete corresponde a un TAG del repositorio del proyecto, es considerado una versión estable y no tiene sufijo.

Ejemplos:

> **Coregen.7.01.222-rc**
Indica la versión aprobada y liberada del sistema 7.01, la revisión 222 indica que en el repositorio la revisión 222 es la copia exacta que genera el paquete y finalmente se indica con el sufijo –rc que el paquete de esta revisión corresponde a una revisión del tronco del proyecto.
{.is-info}


> **CoreGen.7.01.222**
Indica la versión aprobada y liberada del sistema 7.01, la revisión 222 indica que en el repositorio la revisión 222 es la copia exacta que genera el paquete y finalmente como no hay sufijo, se indica que la revisión corresponde a un TAG liberado en Subversión.
{.is-info}

### 6.4.3.	Archivos del paquete NUGET.

Teniendo en cuenta los tipos de paquetes a generar, se debe contar con los correspondientes archivos que el paquete NUGET entregará, de la siguiente manera:

- **Paquetes NUGET de librería dll:** Previamente a la creación del paquete se debe generar archivo dll del proyecto que es identificado como dependencia desde la revisión o TAG especifico a entregar, la generación del archivo dll se debe generar compilando el proyecto en modo release.

- **Paquetes NUGET de contenido:** Posterior a la compilación del proyecto identificado como dependencia y en la revisión o TAG específico, se debe tener en cuenta los archivos y/o carpetas que deben ser entregadas por el paquete. Para modificar la salida o archivos a entregar en el paquete en la generación automatica, la configuración de compilación del proyecto puede especificarse para una configuración personalizada que debe nombrarse Nuget y heredando todas las propiedades de la configuración Debug.

Ejemplo GENNET:

#### Configuración Debug

```csproj
<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|AnyCPU' ">
	<DebugSymbols>true</DebugSymbols>
	<DebugType>full</DebugType>
	<Optimize>false</Optimize>
	<OutputPath>bin\</OutputPath>
	<DefineConstants>DEBUG;TRACE</DefineConstants>
	<ErrorReport>prompt</ErrorReport>
	<WarningLevel>4</WarningLevel>
	<FilesToIncludeForPublish>OnlyFilesToRunTheApp</FilesToIncludeForPublish>
	<DocumentationFile>bin\presentacion.ACTSIS.XML</DocumentationFile>
	<CodeAnalysisRuleSet>presentacion.ACTSIS.ruleset</CodeAnalysisRuleSet>
	<PublishDatabases>false</PublishDatabases>
</PropertyGroup>
```

#### Configuración Nuget personalizada

```csproj
<PropertyGroup Condition="'$(Configuration)|$(Platform)' == 'Nuget|AnyCPU'">
	<DebugSymbols>true</DebugSymbols>
	<OutputPath>bin\</OutputPath>
	<DefineConstants>DEBUG;TRACE</DefineConstants>
	<DocumentationFile>
	</DocumentationFile>
	<DebugType>full</DebugType>
	<PlatformTarget>AnyCPU</PlatformTarget>
	<ErrorReport>prompt</ErrorReport>
	<CodeAnalysisRuleSet>presentacion.ACTSIS.ruleset</CodeAnalysisRuleSet>
	<PublishDatabases>false</PublishDatabases>
</PropertyGroup>
```

Condición en el contenido especifico web.config para la configuración Nuget, indicando que el archivo solo será incluido si se compila con una configuración diferente a Nuget, de esta manera al generar el paquete de manera automatica, el archivo será excluido en el paquete.

```
<Content Include="Web.config" Condition=" '$(Configuration)' != 'Nuget' ">
	<SubType>Designer</SubType>
</Content>
```

**NUGET de terceros:** Solo se debe tener en cuenta las dlls entregadas por el NUGET de tercero que fue identificado y aprobado para uso de ACTSIS.

### 6.4.4.	Creación del paquete NUGET.

Una vez se cuente con los requisitos de los pasos anteriores, se debe generar el paquete NUGET.

Para esta generación tener en cuenta las recomendaciones para usar una u otra forma de acuerdo a lo siguiente:

- **Manualmente:** La generación manual se sugiere solo cuando no sea posible generar el paquete por medio de un script automatizado o con la herramienta NUGET Package Explorer, este tipo de generación debe solicitarse al rol asignado para administrar los paquetes NUGET y cuente con los permisos requeridos para la publicación.

- **NUGET Package Explorer:** La generación con esta herramienta se sugiere solo cuando no sea posible generar el paquete por medio de un script automatizado, para la generación del paquete se debe tomar de referencia un paquete ya construido y la documentación del repositorio https://svn.actsis.com/svn/NUGET/Templates. Este tipo de generación debe solicitarse al rol asignado para administrar los paquetes NUGET y cuente con los permisos requeridos para la publicación.

- **Script automatizado:** En las tareas de integración continua, los proyectos que son identificados como dependencias de otros, tienen una sección final como tarea y es la generación y publicación del paquete NUGET, la cual está documentada en el archivo de automatización y en el repositorio https://svn.actsis.com/svn/NUGET/Templates. Al ser automática como parte del proceso de integración, no se requiere de personal para la creación del paquete, si la creación automática falla, la creación del paquete se realiza de alguna de las dos formas anteriores.

## 6.5.	Almacenar/Listar paquetes NUGET.

Posterior a la creación de un paquete NUGET, se debe publicar el paquete en el servidor de paquetes NUGET, de esta manera queda disponible para su uso. La publicación del paquete en el servidor se puede realizar de tres maneras diferentes de igual manera que la generación de paquetes:

- **Manualmente:** La publicación manual de paquetes NUGET debe solicitarse al rol asignado para administrar los paquetes NUGET y cuente con los permisos requeridos para la publicación. Para esto se requiere tener en el equipo desde donde se realizará la publicación, el entorno de comandos NUGET.exe disponible como descarga en la URL https://www.NUGET.org/downloads, en este momento se encuentra disponible la versión 4.7.1. El comando para realizar la publicación del paquete es la siguiente:

```bash
NUGET.exe push -Source [URL servidor] -NonInteractive -ApiKey [*****] [Paquete].nupkg
```

Teniendo en cuenta que para ejecutar el comando indicado debe estar ubicado en el mismo nivel de carpeta donde se encuentra ubicado el paquete.

- **[URL servidor]:** Corresponde a la URL del servidor de paquetes NUGET.
- **[*****]:** Corresponde a la clave de seguridad para publicar paquetes en el servidor de paquetes NUGET.
- **[Paquete]:** Corresponde al nombre del paquete generado que se va a publicar.

	-	**NUGET Package Explorer:** La publicación del paquete debe solicitarse al rol asignado para administrar los paquetes NUGET y cuente con los permisos requeridos para la publicación. Esta herramienta no requiere el entorno de comando NUGET.exe. La publicación se realiza con el paquete abierto en la herramienta de la siguiente manera:
  
	1.   Ingresar al menú File y hacer clic en la opción Publish, para ingresar al cuadro de dialogo correspondiente.

	![nugetpackageexplroer_1.png](/recursos/imagenes/nugetpackageexplroer_1.png)
  
  En el cuadro de dialogo Publish Package, ingresar en el campo Publish Url, la URL del servidor de paquetes NUGET y en el cuadro de dialogo Publish key or PAT, la clave de seguridad para publicar paquetes en el servidor. Ninguna de las opciones inferiores requiere ser marcadas. Finalmente, haga clic en el botón Publish, para publicar el paquete.
  
  ![nugetpackageexplroer_2.png](/recursos/imagenes/nugetpackageexplroer_2.png)
  
  Posibles errrores en la publicación del paquete:

- Clave errada: la clave no coincide, se debe confirmar la clave con recursos tecnicos.
- Paquete con tamaño mayor al definido como tamaño maximo en el servidor: Se debe configurar en el servidor de paquetes NUGET para aceptar paquetes de mayor tamaño.
- Versión del paquete ya existe: Se debe revisar el paquete creado, ya que el servidor detecta que ya existe un paquete identico con la misma versión.

- **Script automatizado:** En las tareas de integración continua, los proyectos que son identificados como dependencias de otros, tienen una sección final como tarea y es la generación y publicación del paquete NUGET, la cual está documentada en el archivo de automatización y en el repositorio https://svn.actsis.com/svn/NUGET/Templates.

## 6.6.	Instalar paquetes NUGET.

La tarea operativa de instalar un paquete NUGET está sujeta a que previamente se encuentre identificado en el proyecto correspondiente el uso de la dependencia. Por defecto, los proyectos CORE de un sistema específico tienen dependencia de la librería CoreGen, y estas a su vez de la librería 3rosCore, los proyectos web y los servicios web que requieren el uso de la capa CORE de un sistema, tienen dependencia de la librería CORE del sistema.

Ejemplo:

```diagram
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB2ZXJzaW9uPSIxLjEiIHdpZHRoPSI2MTFweCIgaGVpZ2h0PSIyMjFweCIgdmlld0JveD0iLTAuNSAtMC41IDYxMSAyMjEiIGNvbnRlbnQ9IiZsdDtteGZpbGUgaG9zdD0mcXVvdDtlbWJlZC5kaWFncmFtcy5uZXQmcXVvdDsgbW9kaWZpZWQ9JnF1b3Q7MjAyMy0wNC0wNVQxNjoxMjo1NC4zOTBaJnF1b3Q7IGFnZW50PSZxdW90O01vemlsbGEvNS4wIChXaW5kb3dzIE5UIDEwLjA7IFdpbjY0OyB4NjQ7IHJ2OjEwOS4wKSBHZWNrby8yMDEwMDEwMSBGaXJlZm94LzExMS4wJnF1b3Q7IGV0YWc9JnF1b3Q7cmVCOEl1RnVHUXpvYUJ1OUN3OE0mcXVvdDsgdmVyc2lvbj0mcXVvdDsyMS4xLjQmcXVvdDsgdHlwZT0mcXVvdDtlbWJlZCZxdW90OyZndDsmbHQ7ZGlhZ3JhbSBpZD0mcXVvdDtXVlNJU0dUQ1hzUlIxOWsyRUlPVCZxdW90OyBuYW1lPSZxdW90O1DDoWdpbmEtMSZxdW90OyZndDs3VmhOYzVzd0VQMDF2bllBR1d3ZkUwTGNRNXRENlV6U293b2JVQ3V6akJBMjdxK3ZBR0VnU2pMdWwra3c4Y1hzMDY1QTcrMkh6WUw0dTJvcmFKNSt4Qmo0d3JIaWFrRnVGbzVqVzY2bHZtcmsyQ0t1dDJ5QlJMQllPL1ZBeUg1QUY2blJrc1ZRakJ3bElwY3NINE1SWmhsRWNvUlJJZkF3ZG50RVByNXJUaE13Z0RDaTNFVHZXU3pURmwwN3F4NS9EeXhKdXp2YjNxWmQyZEhPV1ora1NHbU1od0ZFZ2dYeEJhSnNyM2FWRDd3bXIrT2xqYnQ5WWZYMFlBSXllVTZBNXIyUXgrNXNFS3VqYWhPRlRESEJqUEtnUjY4Rmxsa005UWFXc25xZkQ0aTVBbTBGZmdNcGoxbzNXa3BVVUNwM1hLOUN4ZVRENFBwTHZkVTdWMXMzbGQ2NU1ZNmRrVWx4ZkJnYWc2amE3TU1hcTRzekNkRWNGVmlLU0ovWjBUbEVSUUxhaTdSUXpjWWdUSk80QmR5QnVvdHlFTUNwWlB0eFlsQ2RYOG5KcjVkQVhXZ1ZubGRFUDh1ZThsSnZTZ1FXUGdvd3BPcUZxSms4cEV4Q21OUG1VQWRWZUdQU0h4bm5QbklVVFN5eHYxSWIxSUd1Q3lud093eFdMTXNMcm03ckNNemtBSDlzUHE5UnVnY2hvWHFWcm02VjZQelhEZUJVRDRlK25Pd09Td2VsNUZsL3puRFhSZWFiOUZFcDlxZThPS3NDaUZrQjdsUVZZTnR6MStkM0pmRW1rOFNadXlSL3BXUldVK2xEaktGUkQ0d3RaRE9hR2M3NnljendMamd6WElOZ1JlNG55RlZXUTNFZnpvam01WEpNczcyKzVHZ21iMzNtYVoveHpENnpucXJQZUdZWkJIZDN3ZWNaNS85RjI0dzkrLzlqdjU3L3EvOG8vMjMzVFo4ejlObE1wYy9xMmQ5QklZMW0zS0RJOG9JTmFtMFFIRjc1OHhvQW5qUGhBTmlZL0lMWXM0aGhvV0RGOVl5Si9wZUpyTXorcFdxek5uZzFUWUtmJmx0Oy9kaWFncmFtJmd0OyZsdDsvbXhmaWxlJmd0OyI+PGRlZnMvPjxnPjxwYXRoIGQ9Ik0gMTIwIDcwIEwgMTQwIDcwIEwgMTQwIDExMCBMIDE0My42MyAxMTAiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDE0OC44OCAxMTAgTCAxNDEuODggMTEzLjUgTCAxNDMuNjMgMTEwIEwgMTQxLjg4IDEwNi41IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjAiIHk9IjQwIiB3aWR0aD0iMTIwIiBoZWlnaHQ9IjYwIiByeD0iOSIgcnk9IjkiIGZpbGw9IiMxYmExZTIiIHN0cm9rZT0iIzAwNmVhZiIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxMThweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiA3MHB4OyBtYXJnaW4tbGVmdDogMXB4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiIGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6ICNmZmZmZmY7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMjU1LCAyNTUsIDI1NSk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPjNyb3NDb3JlPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjYwIiB5PSI3NCIgZmlsbD0iI2ZmZmZmZiIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMnB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj4zcm9zQ29yZTwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSAyNzAgMTEwIFEgMjkwIDExMCAyOTAgNzAgUSAyOTAgMzAgMzAzLjYzIDMwIiBmaWxsPSJub25lIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSAzMDguODggMzAgTCAzMDEuODggMzMuNSBMIDMwMy42MyAzMCBMIDMwMS44OCAyNi41IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxwYXRoIGQ9Ik0gMjcwIDExMCBMIDMwMy42MyAxMTAiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDMwOC44OCAxMTAgTCAzMDEuODggMTEzLjUgTCAzMDMuNjMgMTEwIEwgMzAxLjg4IDEwNi41IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxwYXRoIGQ9Ik0gMjcwIDExMCBRIDI5MCAxMTAgMjkwIDE1MCBRIDI5MCAxOTAgMzAzLjYzIDE5MCIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gMzA4Ljg4IDE5MCBMIDMwMS44OCAxOTMuNSBMIDMwMy42MyAxOTAgTCAzMDEuODggMTg2LjUgWiIgZmlsbD0icmdiKDAsIDAsIDApIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iMTUwIiB5PSI4MCIgd2lkdGg9IjEyMCIgaGVpZ2h0PSI2MCIgcng9IjkiIHJ5PSI5IiBmaWxsPSIjMWJhMWUyIiBzdHJva2U9IiMwMDZlYWYiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTE4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMTEwcHg7IG1hcmdpbi1sZWZ0OiAxNTFweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7IiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiAjZmZmZmZmOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDI1NSwgMjU1LCAyNTUpOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij5Db3JlR2VuPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjIxMCIgeT0iMTE0IiBmaWxsPSIjZmZmZmZmIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjEycHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPkNvcmVHZW48L3RleHQ+PC9zd2l0Y2g+PC9nPjxyZWN0IHg9IjMxMCIgeT0iMCIgd2lkdGg9IjEyMCIgaGVpZ2h0PSI2MCIgcng9IjkiIHJ5PSI5IiBmaWxsPSIjMWJhMWUyIiBzdHJva2U9IiMwMDZlYWYiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTE4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMzBweDsgbWFyZ2luLWxlZnQ6IDMxMXB4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiIGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6ICNmZmZmZmY7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMjU1LCAyNTUsIDI1NSk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPkdlblJlcG9ydGVzV1M8L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iMzcwIiB5PSIzNCIgZmlsbD0iI2ZmZmZmZiIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMnB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj5HZW5SZXBvcnRlc1dTPC90ZXh0Pjwvc3dpdGNoPjwvZz48cGF0aCBkPSJNIDQzMCAxMTAgUSA0MzAgMTEwIDQ4My42MyAxMTAiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDQ4OC44OCAxMTAgTCA0ODEuODggMTEzLjUgTCA0ODMuNjMgMTEwIEwgNDgxLjg4IDEwNi41IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjMxMCIgeT0iODAiIHdpZHRoPSIxMjAiIGhlaWdodD0iNjAiIHJ4PSI5IiByeT0iOSIgZmlsbD0iIzFiYTFlMiIgc3Ryb2tlPSIjMDA2ZWFmIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDExOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDExMHB4OyBtYXJnaW4tbGVmdDogMzExcHg7Ij48ZGl2IHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyIgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogI2ZmZmZmZjsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6IHJnYigyNTUsIDI1NSwgMjU1KTsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgb3ZlcmZsb3ctd3JhcDogbm9ybWFsOyI+R0VOTkVUPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjM3MCIgeT0iMTE0IiBmaWxsPSIjZmZmZmZmIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjEycHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPkdFTk5FVDwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSA0MzAgMTkwIFEgNDYwIDE5MCA0NjAgMTUwIFEgNDYwIDExMCA0ODMuNjMgMTEwIiBmaWxsPSJub25lIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA0ODguODggMTEwIEwgNDgxLjg4IDExMy41IEwgNDgzLjYzIDExMCBMIDQ4MS44OCAxMDYuNSBaIiBmaWxsPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48cGF0aCBkPSJNIDQzMCAxOTAgUSA0MzAgMTkwIDQ4My42MyAxOTAiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDQ4OC44OCAxOTAgTCA0ODEuODggMTkzLjUgTCA0ODMuNjMgMTkwIEwgNDgxLjg4IDE4Ni41IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjMxMCIgeT0iMTYwIiB3aWR0aD0iMTIwIiBoZWlnaHQ9IjYwIiByeD0iOSIgcnk9IjkiIGZpbGw9IiMxYmExZTIiIHN0cm9rZT0iIzAwNmVhZiIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxMThweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAxOTBweDsgbWFyZ2luLWxlZnQ6IDMxMXB4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiIGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6ICNmZmZmZmY7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMjU1LCAyNTUsIDI1NSk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPkNvcmVTYWM8L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iMzcwIiB5PSIxOTQiIGZpbGw9IiNmZmZmZmYiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+Q29yZVNhYzwvdGV4dD48L3N3aXRjaD48L2c+PHJlY3QgeD0iNDkwIiB5PSI4MCIgd2lkdGg9IjEyMCIgaGVpZ2h0PSI2MCIgcng9IjkiIHJ5PSI5IiBmaWxsPSIjMWJhMWUyIiBzdHJva2U9IiMwMDZlYWYiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTE4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMTEwcHg7IG1hcmdpbi1sZWZ0OiA0OTFweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7IiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiAjZmZmZmZmOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDI1NSwgMjU1LCAyNTUpOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij5TQUNORVQ8L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iNTUwIiB5PSIxMTQiIGZpbGw9IiNmZmZmZmYiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+U0FDTkVUPC90ZXh0Pjwvc3dpdGNoPjwvZz48cmVjdCB4PSI0OTAiIHk9IjE2MCIgd2lkdGg9IjEyMCIgaGVpZ2h0PSI2MCIgcng9IjkiIHJ5PSI5IiBmaWxsPSIjMWJhMWUyIiBzdHJva2U9IiMwMDZlYWYiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTE4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMTkwcHg7IG1hcmdpbi1sZWZ0OiA0OTFweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7IiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiAjZmZmZmZmOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDI1NSwgMjU1LCAyNTUpOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij5TZXJ2aWNpb3MgU0FDPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjU1MCIgeT0iMTk0IiBmaWxsPSIjZmZmZmZmIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjEycHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPlNlcnZpY2lvcyBTQUM8L3RleHQ+PC9zd2l0Y2g+PC9nPjwvZz48c3dpdGNoPjxnIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSIvPjxhIHRyYW5zZm9ybT0idHJhbnNsYXRlKDAsLTUpIiB4bGluazpocmVmPSJodHRwczovL3d3dy5kaWFncmFtcy5uZXQvZG9jL2ZhcS9zdmctZXhwb3J0LXRleHQtcHJvYmxlbXMiIHRhcmdldD0iX2JsYW5rIj48dGV4dCB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEwcHgiIHg9IjUwJSIgeT0iMTAwJSI+VGV4dCBpcyBub3QgU1ZHIC0gY2Fubm90IGRpc3BsYXk8L3RleHQ+PC9hPjwvc3dpdGNoPjwvc3ZnPg==
```



Para la instalación de un paquete NUGET en un proyecto .NET, se requiere configurar en el IDE el origen de fuentes de los paquetes según las instrucciones indicadas en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones, en este mismo repositorio, se encuentran las instrucciones operativas para la instalación de un paquete nuget.

## 6.7.	Actualizar paquetes NUGET.

a tarea operativa de actualizar los paquetes NUGET de GENERAL existentes en un proyecto, está sujeta a que previamente se encuentre identificado en el análisis del requerimiento en desarrollo, la necesidad de actualizar el paquete o exista un requerimiento específico para la actualización.

Para los proyectos WEB .NET, la actualización de los paquetes NUGET se presenta en los siguientes escenarios:

1.	En los proyectos como portales o servicios web, los cuales se encuentran desactualizados respecto al CORE del propio sistema, para estos casos la actualización del CORE debe realizarse por medio de un requerimiento con el alcance respectivo o durante la actualización del mismo servicio si requiere actualizar las clases relacionadas al servicio en el CORE.

2.	Previo a la integración en el tronco de los desarrollos de los proyectos web que tengan una dependencia también en desarrollo como un CORE, primero se debe integrar el proyecto CORE, para generar por medio de las tareas de integración continua el paquete NUGET respectivo, de esta manera antes de integrar el proyecto web se actualiza el NUGET correspondiente con el generado en la integración del CORE.

3.	Previo a la generación de un TAG del proyecto, ya que previamente se debe crear el TAG correspondiente al proyecto del CORE, de esta manera, el integrador obtiene el paquete del CORE correspondiente al TAG y lo actualiza en la rama tronco del proyecto web o de otro tipo, para generar el TAG de este mismo proyecto.

Para la actualización de un paquete NUGET en un proyecto .NET, se requiere configurar en el IDE el origen de fuentes de los paquetes según las instrucciones indicadas en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones, en este mismo repositorio, se encuentran las instrucciones operativas para la actualización de un paquete nuget.

## 6.8.	Restaurar paquetes NUGET.

La tarea de restauración de paquetes NUGET se requerida cuando se descarga un proyecto .NET de su correspondiente repositorio y se requiere compilar o publicar el proyecto, la restauración descarga las dependencias requeridas, ya que el proyecto no es versionado en el repositorio con las dependencias.

Para la restauración de un paquete NUGET en un proyecto .NET, se requiere configurar en el IDE el origen de fuentes de los paquetes según las instrucciones indicadas en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones/OrigenPaquetes, en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones/Restaurar se encuentran las instrucciones operativas para la restauración de paquete nuget.

## 6.9.	Desinstalar paquetes NUGET.

La tarea operativa de desinstalar un paquete NUGET existente en un proyecto, está sujeta a que previamente se encuentre identificado en el análisis del requerimiento en desarrollo, la necesidad de eliminar el paquete teniendo en cuenta que no se requiere el uso de las librerías que sirve el paquete.

Para la desinstalación de un paquete NUGET en un proyecto .NET, se requiere configurar en el IDE el origen de fuentes de los paquetes según las instrucciones indicadas en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones, en este mismo repositorio, se encuentran las instrucciones operativas para la desinstalación de un paquete nuget.

## 6.10.	Eliminar paquetes NUGET.

Como parte del mantenimiento del servidor de paquetes nuget, se definen las siguientes condiciones para la eliminación de paquetes nuget del servidor:

- Solo se permite eliminar los paquetes nuget correspondientes a versiones preliminares, es decir, aquellas que tienen un sufijo que indican esta condición, por ejemplo el sufijo –rc, las cuales corresponden a una revisión de una rama en el repositorio diferente a la rama TAG.
- Los paquetes nuget que corresponden a versiones estables, es decir, que no tienen un sufijo, no deben ser eliminadas.
- Para la eliminación de los paquetes de versiones preliminares se define que serán eliminados los paquetes correspondientes a versiones inferiores a la penúltima versión estable del paquete, o teniendo en cuenta como referencia el repositorio de cada proyecto, se deben eliminar los paquetes correspondientes a revisiones inferiores al penúltimo TAG generado y que correspondan a una rama diferente a TAG.

Ejemplo:

![eliminar_paquetes_nuget.png](/recursos/imagenes/eliminar_paquetes_nuget.png)

![nuget](https://upload.wikimedia.org/wikipedia/commons/2/25/NuGet_project_logo.svg){.align-abstopright}