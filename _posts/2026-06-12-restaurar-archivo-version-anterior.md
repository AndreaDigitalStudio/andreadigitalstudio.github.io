---
layout: post
title: "Restaurar un archivo a una versión anterior"
date: 2026-06-12
categories: git
---

## Para qué sirve

A veces necesitas recuperar la versión de un archivo tal como estaba en un commit anterior — sin deshacer todo el historial, solo ese archivo concreto. Para esto necesitas el **ID (hash) del commit** donde estaba la versión que quieres recuperar.

## Paso 1 — Encontrar el commit

```bash
git log --oneline -- archivo.txt
```

Muestra solo los commits que modificaron ese archivo:

```text
a1b2c3d feat: actualizar configuración del servidor
e4f5g6h fix: corregir typo en config
i7j8k9l feat: versión inicial de la configuración
```

## Paso 2 — Ver cómo estaba el archivo en ese commit (opcional)

Antes de restaurar, puedes revisar el contenido sin tocar nada:

```bash
git show i7j8k9l:archivo.txt
```

Esto muestra el contenido del archivo tal como estaba en ese commit, directamente en la terminal.

## Paso 3 — Restaurar el archivo

```bash
git checkout i7j8k9l -- archivo.txt
```

> En versiones modernas de Git se recomienda usar `git restore`:
> ```bash
> git restore --source=i7j8k9l -- archivo.txt
> ```

Esto trae la versión del archivo de ese commit y la coloca en tu directorio de trabajo, **como un cambio sin confirmar**.

## Paso 4 — Confirmar el cambio

Revisar que es lo que esperabas:

```bash
git diff archivo.txt
```

Si está bien, hacer commit normalmente:

```bash
git add archivo.txt
git commit -m "fix: restaurar configuración anterior de archivo.txt"
```

## Diferencia con `git revert`

| | Restaurar archivo (`checkout`/`restore`) | `git revert` |
| --- | --- | --- |
| Afecta a | Un archivo concreto | Todo el commit (todos sus archivos) |
| Crea un commit automáticamente | No, tú decides cuándo | Sí, automáticamente |

## Resumen

```text
1. git log --oneline -- archivo.txt                 → encontrar el commit
2. git show <commit>:archivo.txt                    → (opcional) ver contenido
3. git restore --source=<commit> -- archivo.txt     → recuperar versión
4. git add archivo.txt && git commit -m "..."       → confirmar
```