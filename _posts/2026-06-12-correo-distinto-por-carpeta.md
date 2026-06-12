---
layout: post
title: "Usar un correo distinto según la carpeta del proyecto"
date: 2026-06-12
categories: git
---

## Para qué sirve

Si quieres que una carpeta (y todos los repositorios dentro de ella) usen siempre el mismo correo y nombre de autor, Git tiene una herramienta llamada `includeIf`. Es útil cuando trabajas con varias cuentas (personal y trabajo) y no quieres configurar `user.email` manualmente en cada repositorio.

## Cómo funciona

```text
~/proyectos/
├── personal/
│   └── (repos personales) → usa email personal
└── trabajo/
    └── (repos de trabajo)  → usa email de trabajo
```

Git detecta en qué carpeta está el repositorio y carga automáticamente la configuración correspondiente.

## Paso 1 — Crear un archivo de configuración por contexto

```bash
nano ~/.gitconfig-personal
```

```ini
[user]
    name = Tu Nombre
    email = tu_email_personal@ejemplo.com
```

```bash
nano ~/.gitconfig-trabajo
```

```ini
[user]
    name = Tu Nombre Trabajo
    email = tu_email_trabajo@empresa.com
```

## Paso 2 — Añadir `includeIf` al `.gitconfig` global

```bash
nano ~/.gitconfig
```

```ini
[includeIf "gitdir:~/proyectos/personal/"]
    path = ~/.gitconfig-personal

[includeIf "gitdir:~/proyectos/trabajo/"]
    path = ~/.gitconfig-trabajo
```

> **¿Qué hace `includeIf "gitdir:..."`?**  
> Le dice a Git: "si el repositorio en el que estoy trabajando está dentro de esta ruta, carga esta configuración adicional". Es automático — no hay que recordar configurarlo repositorio por repositorio.

## Verificación

Dentro de un repositorio en `~/proyectos/trabajo/algun-repo`:

```bash
git config user.email
```

Debe mostrar el email de trabajo, sin haberlo configurado manualmente en ese repositorio.

## Resumen

| Ubicación del repo | Configuración aplicada |
| --- | --- |
| `~/proyectos/personal/...` | `~/.gitconfig-personal` |
| `~/proyectos/trabajo/...` | `~/.gitconfig-trabajo` |
| Cualquier otra ruta | Configuración global por defecto |

## Relacionado

Para gestionar claves SSH distintas por cuenta, ver [Usar varias cuentas de GitHub]({% post_url 2026-06-12-multiples-cuentas-github %}).