---
layout: post
title: "Usar varias cuentas de GitHub (trabajo y personal)"
date: 2026-06-12
categories: git
---

## Para qué sirve

Cuando tienes una cuenta de GitHub personal y otra del trabajo, cada una necesita su propia clave SSH. Git no distingue automáticamente cuál usar — hay que configurarlo mediante el archivo `config` de SSH.

## Idea principal

```text
~/.ssh/
├── id_ed25519_personal      (clave privada personal)
├── id_ed25519_personal.pub  (clave pública personal)
├── id_ed25519_trabajo       (clave privada trabajo)
├── id_ed25519_trabajo.pub   (clave pública trabajo)
└── config                   (le dice a SSH qué clave usar y cuándo)
```

## Paso 1 — Generar una clave por cuenta

```bash
ssh-keygen -t ed25519 -C "tu_email_personal@ejemplo.com" -f ~/.ssh/id_ed25519_personal
ssh-keygen -t ed25519 -C "tu_email_trabajo@empresa.com" -f ~/.ssh/id_ed25519_trabajo
```

El flag `-f` indica el nombre del archivo, para no sobrescribir una clave existente.

## Paso 2 — Añadir cada clave pública a su cuenta de GitHub

```bash
cat ~/.ssh/id_ed25519_personal.pub
cat ~/.ssh/id_ed25519_trabajo.pub
```

Copiar cada una en su cuenta correspondiente:

*GitHub (cuenta personal) → Settings → SSH and GPG keys → New SSH key*

*GitHub (cuenta trabajo)  → Settings → SSH and GPG keys → New SSH key*

## Paso 3 — Crear el archivo de configuración SSH

```bash
nano ~/.ssh/config
```

Añadir un "alias" de host para cada cuenta:

```text
# Cuenta personal
Host github.com-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal

# Cuenta trabajo
Host github.com-trabajo
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_trabajo
```

> **¿Qué hace esto?**  
> Crea dos "alias" (`github.com-personal` y `github.com-trabajo`) que apuntan al mismo servidor (`github.com`) pero usando claves distintas. Git usará uno u otro según la URL del remoto.

## Paso 4 — Clonar o configurar repositorios usando el alias

En lugar de:

```bash
git clone git@github.com:usuario/repo.git
```

Usar el alias correspondiente:

```bash
git clone git@github.com-personal:usuario-personal/repo.git
git clone git@github.com-trabajo:usuario-trabajo/repo.git
```

## Paso 5 — Cambiar el remoto de un repositorio existente

Si un repositorio ya clonado usa la cuenta equivocada:

```bash
git remote set-url origin git@github.com-trabajo:usuario-trabajo/repo.git
```

## Paso 6 — Configurar nombre y email por repositorio (opcional)

Para que los commits de cada proyecto aparezcan con el usuario correcto:

```bash
git config user.name "Tu Nombre Trabajo"
git config user.email "tu_email_trabajo@empresa.com"
```

Sin `--global`, esto solo afecta al repositorio actual.

## Verificación

```bash
ssh -T git@github.com-personal
ssh -T git@github.com-trabajo
```

Cada uno debería responder con el usuario correspondiente:

```text
Hi usuario-personal! You've successfully authenticated, but GitHub does not provide shell access.
```

## Resumen

| Qué cambia | Cómo |
| --- | --- |
| Clave SSH usada | Alias en `~/.ssh/config` (`github.com-personal` / `github.com-trabajo`) |
| URL del remoto | `git@github.com-ALIAS:usuario/repo.git` |
| Autor de los commits | `git config user.name` / `user.email` (sin `--global`, por repo) |