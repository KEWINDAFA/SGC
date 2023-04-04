---
title: IG-03 Configuración_NUGET
description: Instructivo gestión de la configuración de NuGets
published: true
date: 2023-04-04T20:43:24.977Z
tags: nuget, gestion de la configuracion, instructivos, ig-03, dependencias, librerias, paquetes
editor: markdown
dateCreated: 2023-04-04T19:44:54.567Z
---

# Descripción General

## 1. Objetivos

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

# 6.3. Tipos de paquetes NUGET.

Teniendo en cuenta la identificación de las dependencias que son controladas con paquetes NUGET, se definen 2 tipos de paquetes NUGET para su generación.

## tipos_NuGets {.tabset}

### Paquetes NUGET de librería dll

Corresponden a los paquetes que gestionan las dependencias de proyectos .Net que solo generan librerías dll, el destino de estas librerías en los proyectos que la usan, es la carpeta **Bin**. Teniendo en cuenta que el paquete **3rosCore** entrega todas las librerías dll de terceros que son requeridas a nivel de Core, este es tipificado como paquete de librerías dll.

### Paquetes NUGET de contenido

Corresponden a los paquetes que gestionan cualquier tipo de archivo para ser ubicado dentro de la estructura del proyecto .NET. Estos paquetes pueden entregar código no compilado que debe ser compilado por el proyecto que lo usa. Para este caso se encuentra el paquete NUGET del proyecto GENNET, el cual entrega todo el contenido del proyecto ASP .NET Web Forms correspondiente a las pantallas, controles de usuario, scripts y otras clases C# que debe compilar el proyecto que lo usa. Teniendo en cuenta que el paquete 3rosWeb entrega todos los archivos, contenidos, scripts, dlls, xml y cualquier otro tipo de archivo que sea requerido a nivel de proyecto ASP .NET Web Forms desarrollado por un tercero, es tipificados como proyecto de contenido.