---
layout: post
title: "git clone vs git remote add origin"
date: 2026-06-11
categories: git
---

## La diferencia en una frase

`git clone` se usa cuando **el repositorio ya existe en GitHub** y quieres traerlo a tu equipo. `git remote add origin` se usa cuando **el proyecto ya existe en tu equipo** y quieres conectarlo a un repositorio en GitHub.

## git clone — descargar un repositorio existente

Crea una copia completa de un repositorio remoto en tu equipo, con todo su historial.

```bash
git clone https://github.com/usuario/repositorio.git
```

Esto:

- Crea una carpeta nueva con el nombre del repositorio
- Descarga todos los archivos y el historial de commits
- Configura automáticamente `origin` apuntando a esa URL

No hace falta hacer `git init` ni `git remote add` después — ya queda todo conectado.

## git remote add origin — conectar un proyecto local a GitHub

Se usa cuando el proyecto **ya existe en tu equipo** (por ejemplo, lo creaste con `git init`) y quieres subirlo a un repositorio vacío en GitHub.

```bash
git remote add origin https://github.com/usuario/repositorio.git
```

Esto:

- No descarga nada
- Solo le dice a Git: "este repositorio local está conectado a esta URL remota"
- A partir de aquí puedes hacer `git push` para subir tus archivos

## Resumen visual

![git clone vs git remote add origin](/assets/img/git-remote-vs-clone.png)

## Cuándo usar cada uno

| Situación | Comando |
| --- | --- |
| Quiero descargar un proyecto que ya está en GitHub | `git clone` |
| Tengo un proyecto local y quiero subirlo a un repo nuevo en GitHub | `git remote add origin` |
| Quiero ver a qué remoto está conectado mi proyecto | `git remote -v` |