---
layout: post
title: "Configurar SSH para GitHub"
date: 2026-06-11
categories: git
---

## Para qué sirve

Configurar una clave SSH permite conectarte a GitHub sin tener que introducir usuario y contraseña (o token) cada vez que haces `push` o `pull`. Tu equipo guarda una clave privada, y GitHub guarda la clave pública correspondiente — cuando coinciden, te identifica automáticamente.

## Paso 1 — Comprobar si ya tienes una clave SSH

```bash
ls -al ~/.ssh
```

Si ves archivos como `id_ed25519` y `id_ed25519.pub`, ya tienes una clave y puedes ir directamente al Paso 3.

## Paso 2 — Generar una clave SSH nueva

```bash
ssh-keygen -t ed25519 -C "tu_email_de_github"
```

Pulsar Enter en todas las preguntas para aceptar la ubicación por defecto (opcionalmente puedes añadir una passphrase para más seguridad).

Esto genera dos archivos en `~/.ssh/`:

- `id_ed25519` — clave privada. No se comparte nunca, se queda en tu equipo.
- `id_ed25519.pub` — clave pública. Esta es la que se sube a GitHub.

## Paso 3 — Activar el agente SSH y añadir la clave

El agente SSH guarda tu clave en memoria para que no tengas que introducir la passphrase en cada conexión.

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

## Paso 4 — Copiar la clave pública

```bash
cat ~/.ssh/id_ed25519.pub
```

Copiar todo el contenido que aparece (empieza por `ssh-ed25519...`).

## Paso 5 — Añadir la clave a GitHub

En GitHub:
*Settings → SSH and GPG keys → New SSH key*

Pegar la clave pública copiada y guardar.

## Paso 6 — Probar la conexión

```bash
ssh -T git@github.com
```

Respuesta esperada:

```text
Hi usuario! You've successfully authenticated, but GitHub does not provide shell access.
```

Si ves este mensaje, la clave está configurada correctamente.

## Paso 7 — Usar SSH en tus repositorios

Para repositorios nuevos, usar la URL SSH al añadir el remoto:

```bash
git remote add origin git@github.com:usuario/repositorio.git
```

Para repositorios existentes que usan HTTPS, cambiar el remoto:

```bash
git remote set-url origin git@github.com:usuario/repositorio.git
```

A partir de aquí, `git push` y `git pull` funcionan sin pedir credenciales.