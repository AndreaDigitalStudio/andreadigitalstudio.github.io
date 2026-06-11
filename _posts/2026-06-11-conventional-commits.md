---
layout: post
title: "Conventional Commits — guía de etiquetas"
date: 2026-06-11
categories: git
tags: [convencional-commits]
---

## Para qué sirve

Conventional Commits es una convención para escribir mensajes de commit de forma estructurada y consistente. Facilita entender el historial, generar changelogs automáticos y saber de un vistazo qué tipo de cambio incluye cada commit.

## Formato

```text
<tipo>: <descripción breve>
```

Ejemplo:

```text
feat: añadir página de categoría Git
fix: corregir ruta del CSS en producción
docs: documentar guía de cron
```

## Tabla de etiquetas

| Etiqueta | Significado | Cuándo usarla |
| --- | --- | --- |
| `feat` | Nueva funcionalidad | Al agregar algo nuevo que no existía |
| `fix` | Corrección de error | Al corregir un bug o comportamiento erróneo |
| `style` | Estilo (CSS, espacios, formato) | Cambios visuales o de formato sin afectar la lógica |
| `refactor` | Refactorización | Mejoras internas del código sin cambiar su funcionamiento |
| `perf` | Rendimiento | Optimizaciones para que algo sea más rápido o eficiente |
| `docs` | Documentación | Cambios en README, comentarios o guías |
| `test` | Tests | Añadir o modificar pruebas |
| `build` | Build / compilación | Cambios en procesos de build, scripts o dependencias |
| `ci` | Integración continua | Configuración de pipelines CI/CD |
| `chore` | Tarea menor | Mantenimiento, limpieza, archivos como `.gitignore` |
| `revert` | Revertir cambios | Para deshacer un commit anterior |

## Ejemplos aplicados al proyecto

```bash
git commit -m "feat: añadir layout categoria-git"
git commit -m "fix: rutas CSS con relative_url"
git commit -m "docs: guía conventional commits"
git commit -m "chore: añadir plataforma x86_64-linux al lockfile"
```