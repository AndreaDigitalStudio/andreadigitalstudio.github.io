---
layout: post
title: "Subir un proyecto al repositorio de GitHub"
date: 2026-06-11
categories: git
---

## Para qué sirve

Pasos básicos para subir un proyecto local nuevo a un repositorio de GitHub por primera vez.

## Paso 1 — Crear el repositorio en GitHub

En GitHub, hacer clic en **New repository**. Darle un nombre y dejarlo vacío (sin README, sin `.gitignore`, sin licencia) para evitar conflictos al hacer el primer push.

## Paso 2 — Inicializar Git en el proyecto local

Dentro de la carpeta del proyecto:

```bash
git init
```

## Paso 3 — Añadir los archivos

```bash
git add .
```

Esto añade todos los archivos del proyecto al área de preparación (staging).

## Paso 4 — Hacer el primer commit

```bash
git commit -m "feat: primer commit del proyecto"
```

## Paso 5 — Conectar el repositorio local con GitHub

Copiar la URL del repositorio creado en GitHub (botón **Code** → HTTPS o SSH):

```bash
git remote add origin https://github.com/usuario/repositorio.git
```

> **Recomendado: usar SSH en lugar de HTTPS**  
> Con HTTPS, GitHub pide un Personal Access Token cada vez que no hay credenciales guardadas. Con SSH, una vez configurada la clave, no vuelves a introducir credenciales. Ver [Configurar SSH para GitHub]({% post_url 2026-06-11-configurar-ssh-github %}).
>
> URL con SSH:
> ```bash
> git remote add origin git@github.com:usuario/repositorio.git
> ```

## Paso 6 — Subir el proyecto

```bash
git push -u origin main
```

El flag `-u` vincula la rama local `main` con `origin/main`, así que en los siguientes pushes basta con:

```bash
git push
```

## Verificación

Recargar la página del repositorio en GitHub — los archivos deberían aparecer ahí.

## Notas

- Si el repositorio en GitHub no está vacío (tiene README o licencia), el push fallará con `[rejected]`. Ver la guía de [bugs de Git-GitHub](/git/bugs/) para solucionarlo.
- Crear un archivo `.gitignore` antes del primer commit evita subir archivos innecesarios (`node_modules/`, `.env`, etc.).