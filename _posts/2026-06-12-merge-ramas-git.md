---
layout: post
title: "Merge — fusionar una rama con otra"
date: 2026-06-12
categories: git
---

## Para qué sirve

`merge` une los cambios de una rama dentro de otra. Lo más común es fusionar una rama de funcionalidad (`feature/...`) dentro de `main` cuando el trabajo está terminado.

## Cómo funciona

![Diagrama merge](/assets/img/merge.png)


Git crea un nuevo commit (`M`) que une ambas historias.

## Pasos

**1. Cambiarte a la rama destino** (normalmente `main`):

```bash
git switch main
```

**2. Asegurarte de que está actualizada:**

```bash
git pull
```

**3. Fusionar la otra rama:**

```bash
git merge nombre-rama
```

Ejemplo:

```bash
git switch main
git pull
git merge feature/menu-responsive
```

**4. Subir los cambios:**

```bash
git push
```

## Tipos de merge

**Fast-forward** — si `main` no ha cambiado desde que creaste la rama, Git simplemente mueve el puntero de `main` hacia adelante, sin crear un commit nuevo:

![Diagrama merge Fast-forward](/assets/img/merge-fast-forward.png)


**Merge commit** — si `main` ha recibido otros commits mientras trabajabas en tu rama, Git crea un commit especial que une ambas ramas (como en el primer diagrama).

## Conflictos de merge

Si el mismo archivo fue modificado de forma diferente en ambas ramas, Git no puede decidir automáticamente y marca un **conflicto**.

```bash
git merge feature/menu-responsive
```

```text
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

Git marca las zonas en conflicto directamente en el archivo:

```text
<<<<<<< HEAD
contenido de main
=======
contenido de feature/menu-responsive
>>>>>>> feature/menu-responsive
```

**Resolver el conflicto:**

1. Editar el archivo y dejar el contenido correcto, eliminando las marcas `<<<<<<<`, `=======`, `>>>>>>>`.
2. Marcar como resuelto:

    ```bash
    git add archivo-resuelto.html
    ```

3. Completar el merge:

    ```bash
    git commit
    ```

## Después del merge

Si la rama ya no se necesita, se puede borrar:

```bash
git branch -d nombre-rama
```

## Resumen

| Acción | Comando |
| --- | --- |
| Ir a la rama destino | `git switch main` |
| Actualizar | `git pull` |
| Fusionar | `git merge nombre-rama` |
| Subir cambios | `git push` |
| Borrar rama fusionada | `git branch -d nombre-rama` |