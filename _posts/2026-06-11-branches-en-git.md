---
layout: post
title: "Branches en Git — cuándo y cómo usarlas"
date: 2026-06-11
categories: git
---

## Idea principal

Las ramas (branches) sirven para separar trabajo en curso del código estable. Así evitas romper lo que ya funciona en `main` mientras pruebas algo nuevo.

## ¿Cuándo crear una rama?

No hace falta abrir una rama por cada archivo (uno para HTML, otro para CSS...). Lo normal es crear una rama por **funcionalidad o tarea concreta**.

Ejemplos:

```bash
git checkout -b estructura-html      # feat: añade estructura básica en HTML
git checkout -b estilos-css          # feat: agrega estilos base en CSS
git checkout -b feature/menu-responsive
git checkout -b fix/arregla-bug-header
```

![Branches flujo recomendado](/assets/img/branches-flujo.png)


## Regla de oro

Crear una rama cada vez que vayas a trabajar en algo que no quieras romper en el código estable.

- Proyecto personal pequeño: puedes empezar trabajando todo en `main`.
- Si quieres acostumbrarte a buenas prácticas: usa ramas aunque trabajes sola.

## Comandos más usados

### Crear ramas

```bash
git branch nombre-rama          # crea la rama, sin cambiarte a ella
git checkout -b nombre-rama     # crea y cambia a la rama
git switch -c nombre-rama       # alternativa moderna
```

### Moverte entre ramas

```bash
git checkout nombre-rama
git switch nombre-rama          # alternativa moderna
```

### Ver ramas

```bash
git branch        # ramas locales
git branch -a     # ramas locales y remotas
```

### Renombrar ramas

```bash
git branch -m nuevo-nombre
```

### Borrar ramas

```bash
git branch -d nombre-rama   # solo si ya está fusionada
git branch -D nombre-rama   # forzado, aunque no esté fusionada
```

### Subir una rama nueva al remoto

```bash
git push -u origin nombre-rama
```

### Fusionar ramas (merge)

```bash
git switch main
git merge nombre-rama
```

### Rebase — alternativa al merge

Mantiene el historial más lineal y limpio:

```bash
git switch feature/estilos-css
git rebase main
```

## Resumen para proyectos pequeños

- `main` → código estable
- `feature/...`, `fix/...` → ramas para cambios concretos
- Al terminar: `merge` o `rebase` a `main`