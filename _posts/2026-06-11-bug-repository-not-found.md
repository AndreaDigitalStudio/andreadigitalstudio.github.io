---
layout: post
title: "Bug — Repository not found al hacer push"
date: 2026-06-11
categories: git
tags: [bugs]
---

## El error

Al hacer `git push` aparece:

```text
remote: Repository not found.
fatal: repository 'https://github.com/usuario/repo.git/' not found
```

## Por qué ocurre

El remoto `origin` de tu repositorio local apunta a una URL de GitHub que no existe, no es accesible, o tiene un nombre/usuario distinto al real. Las causas más comunes son:

- El nombre del repositorio no coincide exactamente con el de GitHub (mayúsculas/minúsculas incluidas — GitHub distingue entre `python` y `Python`)
- El usuario u organización de la URL no es el correcto
- El repositorio aún no se ha creado en GitHub
- No tienes permisos de escritura sobre ese repositorio

## Diagnóstico

Comprobar qué remoto está configurado actualmente:

```bash
git remote -v
```

Comparar la URL que muestra con la URL real del repositorio en GitHub: entrar al repo → botón **Code** → copiar la URL exacta (HTTPS o SSH).

## Solución

### Si el repositorio ya existe en GitHub

Corregir el remoto con la URL correcta:

```bash
git remote set-url origin https://github.com/usuario/repo.git
git push -u origin main
```

Si no existía ningún remoto configurado:

```bash
git remote add origin https://github.com/usuario/repo.git
git push -u origin main
```

### Si el repositorio no existe todavía

Crear un repositorio vacío en GitHub con el nombre deseado y luego:

```bash
git remote add origin https://github.com/usuario/repo.git
git push -u origin main
```

## Autenticación con HTTPS

GitHub ya no acepta contraseña para `git push`. Al pedir credenciales:

- Usuario: tu nombre de usuario de GitHub
- Contraseña: un **Personal Access Token (PAT)**, generado en GitHub → Settings → Developer settings → Personal Access Tokens

## Alternativa: usar SSH

Con SSH no hace falta introducir usuario/token en cada `push`.

**1. Comprobar si ya existe una clave SSH**

```bash
ls -al ~/.ssh
```

Si aparecen `id_ed25519` y `id_ed25519.pub` (o `id_rsa`/`id_rsa.pub`), ya hay una clave creada.

**2. Generar una clave nueva (si no existe)**

```bash
ssh-keygen -t ed25519 -C "tu_email_de_github"
```

Pulsar Enter en todas las preguntas para aceptar los valores por defecto.

**3. Activar el agente SSH y añadir la clave**

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

**4. Copiar la clave pública a GitHub**

```bash
cat ~/.ssh/id_ed25519.pub
```

Copiar el contenido completo (empieza por `ssh-ed25519...`) y añadirlo en **GitHub → Settings → SSH and GPG keys → New SSH key**.

**5. Cambiar el remoto a SSH**

```bash
git remote set-url origin git@github.com:usuario/repo.git
```

**6. Probar la conexión**

```bash
ssh -T git@github.com
```

Respuesta esperada:

```text
Hi usuario! You've successfully authenticated, but GitHub does not provide shell access.
```

**7. Subir el código**

```bash
git push -u origin main
```

## Notas

- La URL del remoto no necesita la barra final (`.git/`); usar `.git` sin más.
- Comprobar siempre que el repo no esté en una organización distinta al usuario esperado.