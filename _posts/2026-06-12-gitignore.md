---
layout: post
title: ".gitignore — qué es y cómo usarlo"
date: 2026-06-12
categories: git
---

## Para qué sirve

`.gitignore` le dice a Git qué archivos y carpetas **no debe rastrear ni subir** al repositorio. Útil para no subir cosas como dependencias, archivos de configuración local, claves, o archivos generados automáticamente.

## Por qué es importante

Sin `.gitignore`, podrías acabar subiendo:

- `node_modules/` — carpetas de dependencias con miles de archivos
- `.env` — variables de entorno con contraseñas o claves API
- `_site/` — carpetas generadas automáticamente (como en Jekyll)
- Archivos de configuración del editor (`.vscode/`, `.idea/`)

## Cómo crear uno

En la raíz del proyecto:

```bash
nano .gitignore
```

Cada línea es un patrón de archivo o carpeta a ignorar:

```text
node_modules/
.env
_site/
*.log
.DS_Store
```

## Sintaxis básica

| Patrón | Qué ignora |
| --- | --- |
| `archivo.txt` | Ese archivo concreto, en cualquier carpeta |
| `carpeta/` | Toda la carpeta y su contenido |
| `*.log` | Todos los archivos con extensión `.log` |
| `!importante.log` | Excepción — no ignora este archivo aunque coincida con una regla anterior |
| `/config.json` | Solo el `config.json` de la raíz, no los de subcarpetas |

## Ejemplo real (proyecto Jekyll)

```text
_site/
.jekyll-cache/
.bundle/
vendor/
Gemfile.lock
```

## Si ya subiste un archivo que querías ignorar

Añadirlo a `.gitignore` no lo elimina si Git ya lo está rastreando. Hay que quitarlo del seguimiento:

```bash
git rm --cached archivo-a-ignorar
git commit -m "chore: dejar de rastrear archivo-a-ignorar"
```

El archivo se mantiene en tu disco, solo se elimina del repositorio.

## Plantillas predefinidas

GitHub ofrece plantillas de `.gitignore` según el lenguaje/framework al crear un repositorio nuevo, o en:

```text
https://github.com/github/gitignore
```

Útil para no escribir desde cero las reglas típicas de Node, Python, Java, etc.

## Resumen

| Acción | Comando |
| --- | --- |
| Crear el archivo | `nano .gitignore` |
| Ignorar carpeta | `carpeta/` |
| Ignorar por extensión | `*.log` |
| Dejar de rastrear algo ya subido | `git rm --cached archivo` |