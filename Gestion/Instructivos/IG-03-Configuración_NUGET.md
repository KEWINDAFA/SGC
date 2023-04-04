---
title: IG-03 Configuración_NUGET
description: Instructivo gestión de la configuración de NuGets
published: true
date: 2023-04-04T20:03:46.349Z
tags: nuget, gestion de la configuracion, instructivos, ig-03
editor: markdown
dateCreated: 2023-04-04T19:44:54.567Z
---

# Descripción General

## 1. Objetivos

Este instructivo tiene como propósito establecer la metodología para gestionar la configuración de los paquetes que son identificados como dependencias en un sistema de información .NET mediante la tecnología NUGET.

## 2. ALCANCE

Este instructivo es aplicable a las dependencias generadas para los proyectos de desarrollo .NET. Las actividades de gestión de configuración de las dependencias comprenden:

- Identificar las dependencias.
-	Generar paquetes para las dependencias.
-	Versionar los paquetes para las dependencias.
-	Controlar el cambio de los paquetes.
-	Almacenar los paquetes en el repositorio.
-	Servir los paquetes en los IDE de desarrollo (Visual Studio).

## 3.	POLITICAS

Todos los productos de trabajo deben ser puestos bajo control del Sistema de Gestión de Configuración, cumpliendo con sus lineamientos con el fin de velar por su integridad.

## 4.	CONDICIONES GENERALES

-	Se considera objeto de configuración para desarrollos WEB *(códigos fuentes de las páginas web, librerías javas, archivos .jar, librerías dll, librerías C#.)*{.caption} 
-	Los desarrollos en ambiente web .NET se trabajan por proyectos los cuales son realizados en la herramienta VISUAL STUDIO.
-	La gestión de configuración para proyectos WEB se realiza a través de SVN Subversión.
-	Todo cambio que afecte o no la funcionalidad de un programa que se encuentre en producción debe generar una nueva versión. Cada vez que se genere un nuevo objeto de los sistemas, se debe definir su nombre según lo indicado en los instructivos ID-05 *`Construcción Software`*.
-	Los backups de los objetos de configuración se realizan siguiendo el instructivo IA-04 `*Realización Backups`*.

## 5.	DOCUMENTOS REFERENCIADOS  

- [•	Instructivo ID-05 *Construcción Software*](/Analisis_Diseño/Instructivos/ID-05-Construcción_de_software.md)
{.links-list}