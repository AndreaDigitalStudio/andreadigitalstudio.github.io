---
layout: post
title: "Deshacer cambios — reset, revert y checkout"
date: 2026-06-12
categories: git
---

## Para qué sirve

Git ofrece varias formas de "deshacer" cosas, pero cada una actúa en un nivel distinto: un archivo concreto, el último commit, o todo el historial. Usar la equivocada puede hacer que pierdas trabajo, así que es importante saber cuál corresponde a cada situación.

## Resumen rápido

| Quiero... | Comando |
| --- | --- |
| Descartar cambios sin confirmar en un archivo | `git checkout -- archivo` |
| Deshacer el último commit pero mantener los cambios | `git reset HEAD~1` |
| Deshacer el último commit y descartar los cambios | `git reset --hard HEAD~1` |
| Revertir un commit ya subido, sin reescribir historial | `git revert <commit>` |

## git checkout -- archivo

Descarta los cambios sin confirmar de un archivo, devolviéndolo a como estaba en el último commit.

```bash
git checkout -- archivo.txt
```

> En versiones modernas de Git se recomienda usar `git restore`:
> ```bash
> git restore archivo.txt
> ```

**Cuándo usarlo:** modificaste un archivo, no te gusta el resultado, y quieres volver a la última versión guardada — sin afectar al historial, porque el cambio nunca se confirmó.

<div class="warning-box">
IMPORTANTE: Esto descarta los cambios para siempre, no hay forma de recuperarlos después.
</div>

## git reset

Mueve el puntero de la rama actual a un commit anterior. Hay tres modos según qué pasa con los cambios.

```bash
git reset --soft HEAD~1
```

Deshace el último commit, pero deja los cambios en el área de preparación (staging) — como si hubieras hecho `git add` pero no `git commit`.

```bash
git reset HEAD~1
```

(modo por defecto, `--mixed`) Deshace el último commit y también lo saca del staging — los cambios quedan en tu directorio de trabajo, sin confirmar.

```bash
git reset --hard HEAD~1
```

Deshace el último commit **y elimina los cambios por completo**. Vuelve todo exactamente a como estaba antes de ese commit.

### Diagrama

```text
Antes:        A───B───C  (HEAD)

reset --soft:  A───B  (HEAD)     C queda como cambios "staged"
reset:         A───B  (HEAD)     C queda como cambios sin confirmar
reset --hard:  A───B  (HEAD)     C desaparece, sin rastro
```

**Cuándo usarlo:** cuando el commit **no se ha subido todavía** a GitHub. Si ya hiciste `push`, usar `reset` reescribe el historial y complica las cosas para quien haya descargado esos commits.

## git revert

Crea un **nuevo commit** que deshace los cambios de un commit anterior, sin borrar nada del historial.

```bash
git revert <hash-del-commit>
```

```text
Antes:    A───B───C
Después:  A───B───C───D   (D deshace lo que hizo C)
```

**Cuándo usarlo:** cuando el commit **ya está subido** a GitHub y otras personas pueden tenerlo. `revert` es seguro porque no reescribe el historial — simplemente añade un commit nuevo que anula el anterior.

## Diferencia clave: reset vs revert

| | `reset` | `revert` |
| --- | --- | --- |
| Reescribe el historial | Sí | No |
| Seguro si ya hiciste push | No | Sí |
| Resultado | El commit desaparece | Se añade un commit nuevo que lo anula |

## Resumen

```text
¿El cambio está sin confirmar?
  → git restore archivo (o checkout -- archivo)

¿El commit no se ha subido (push)?
  → git reset (soft / mixed / hard)

¿El commit ya está en GitHub?
  → git revert <commit>
```