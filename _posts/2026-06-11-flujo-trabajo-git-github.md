---
layout: post
title: "Flujo de trabajo recomendado con Git/GitHub"
date: 2026-06-11
categories: git
---

## Para qué sirve

Un flujo de trabajo ordenado evita pisar el trabajo de otros, mantiene el historial limpio y facilita revisar cambios antes de incorporarlos al proyecto principal.

## El flujo en 6 pasos

![El flujo en 6 pasos](/assets/img/flujo-trabajo.png)


## Paso 1 — Actualizar tu copia local

Antes de empezar a trabajar, traer los últimos cambios del repositorio remoto:

```bash
git pull
```

Evita conflictos por trabajar sobre una versión desactualizada del proyecto.

## Paso 2 — Crear una rama nueva

No se trabaja directamente sobre `main`. Cada cambio o funcionalidad va en su propia rama:

```bash
git checkout -b nombre-rama
```

Ejemplos de nombres descriptivos:

```bash
git checkout -b feature/login-page
git checkout -b fix/error-conexion-db
```

> **¿Por qué una rama nueva?**  
> Si algo sale mal, `main` no se ve afectado. Además permite que varias personas trabajen en paralelo sin interferir entre sí.

## Paso 3 — Hacer los cambios

Editar el código normalmente con tu editor.

## Paso 4 — Guardar los cambios (commit)

```bash
git add .
git commit -m "feat: añadir página de login"
```

> Usa el formato de [Conventional Commits]({% post_url 2026-06-11-conventional-commits %}) para que el mensaje sea claro y consistente.

## Paso 5 — Subir la rama a GitHub

```bash
git push -u origin nombre-rama
```

Esto sube la rama nueva a GitHub. El flag `-u` la vincula con su equivalente remoto, así que los siguientes `push` en esa rama solo necesitan `git push`.

## Paso 6 — Crear un Pull Request

En GitHub, al subir la rama aparece un botón **Compare & pull request**. Ahí se revisa el código antes de fusionarlo (merge) con `main`.

Una vez aprobado y fusionado, se puede borrar la rama:

```bash
git branch -d nombre-rama
```

## Resumen

| Paso | Comando |
| --- | --- |
| Actualizar | `git pull` |
| Crear rama | `git checkout -b nombre-rama` |
| Guardar cambios | `git add . && git commit -m "..."` |
| Subir rama | `git push -u origin nombre-rama` |
| Fusionar | Pull Request en GitHub |