---
layout: post
title: "Simular un servidor de clínica en Linux"
date: 2026-06-10
categories: linux
---

## Para qué sirve

Simula la infraestructura básica de un servidor de clínica: usuarios y grupos por rol, directorios con permisos restrictivos, servicio web nginx y firewall configurado. Todo automatizado con cron.

## Paso 1 — Crear grupos y usuarios

Crear los tres grupos del entorno clínico:

```bash
sudo groupadd medicos enfermeria it
```

Crear usuarios asignados a su grupo:

```bash
sudo useradd -m -G medicos dr_garcia
sudo useradd -m -G it tecnico_it
```

Verificar que se crearon:

```bash
getent group medicos enfermeria it
```

Output esperado:

```text
medicos:x:1001:dr_garcia
enfermeria:x:1002:
it:x:1003:tecnico_it
```

## Paso 2 — Estructura de directorios

Crear los directorios del servidor de un solo comando:

```bash
sudo mkdir -p /srv/{historiales,imagenes,backups}
```

> **Expansión de llaves en bash**  
> `{historiales,imagenes,backups}` es una forma de escribir tres rutas en una sola línea. Bash lo expande automáticamente — equivale a ejecutar `mkdir` tres veces.

Asignar el directorio de historiales al grupo médicos con permisos `750`:

```bash
sudo chown root:medicos /srv/historiales && sudo chmod 750 /srv/historiales
```

> **¿Qué significa 750?**  
> Root puede leer, escribir y ejecutar. El grupo médicos puede leer y entrar. El resto no tiene acceso.

Verificar:

```bash
ls -la /srv/
```

## Paso 3 — Instalar y activar nginx

```bash
sudo apt install nginx -y && sudo systemctl enable --now nginx
```

Verificar que está activo:

```bash
systemctl status nginx
```

## Paso 4 — Firewall

Permitir solo SSH y HTTP, bloquear el resto:

```bash
sudo ufw allow ssh && sudo ufw allow 80 && sudo ufw enable
```

Verificar:

```bash
sudo ufw status
```

Output esperado:

```text
Status: active

To          Action    From
--          ------    ----
22/tcp      ALLOW     Anywhere
80          ALLOW     Anywhere
22/tcp (v6) ALLOW     Anywhere (v6)
80 (v6)     ALLOW     Anywhere (v6)
```

## Paso 5 — Script de monitorización con cron

El script `check_servicios.sh` ya está preparado en la guía [Automatizar tareas con cron]({% post_url 2026-06-10-cron-automatizacion %}). Solo hay que añadir la tarea al crontab:

```bash
crontab -e
```

Añadir al final:

```bash
*/5 * * * * /home/tuusuario/check_servicios.sh >> /var/log/check.log 2>&1
```

Verificar:

```bash
crontab -l
```

## Paso 6 — Verificación final
Comprobar que todo el sistema está funcionando correctamente.

```bash
systemctl status nginx && sudo ufw status && df -h && free -h
```

Output del sistema:

```text
nginx — active (running)
Firewall — active, SSH y HTTP abiertos
Disco — 79G total, 14G usado, 61G libre
RAM — 3.8G total, 2.6G disponible, swap configurado
```