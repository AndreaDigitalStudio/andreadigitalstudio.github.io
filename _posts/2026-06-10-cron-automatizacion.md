---
layout: post
title: "Automatizar tareas con cron"
date: 2026-06-10
categories: linux 
---

## Para qué sirve

Cron es el planificador de tareas de Linux. Permite ejecutar comandos o scripts automáticamente en un momento determinado o de forma periódica — backups nocturnos, limpieza de logs, comprobación de servicios.

## Sintaxis del crontab

```text
* * * * *  comando
│ │ │ │ └── día de la semana (0-7, 0 y 7 = domingo)
│ │ │ └──── mes (1-12)
│ │ └────── día del mes (1-31)
│ └──────── hora (0-23)
└────────── minuto (0-59)
```

Ejemplos:

```bash
0 2 * * *    /scripts/backup.sh     # cada día a las 2:00
*/5 * * * *  /scripts/check.sh      # cada 5 minutos
0 0 * * 0    /scripts/limpieza.sh   # cada domingo a medianoche
```

## Paso 1 — Abrir el crontab

```bash
crontab -e
```

La primera vez pregunta qué editor usar. Elegir nano.

## Paso 2 — Añadir la tarea

Al final del archivo añadir:

```bash
*/5 * * * * /home/tuusuario/check_servicios.sh >> /var/log/check_servicios.log 2>&1
```

Cambiar `tuusuario` por el usuario real. 

> **¿Qué es el `2>&1` al final?**  
> Redirige los errores también al log. Sin eso, si el script falla el error desaparece y no sabes qué pasó.

## Paso 3 — Verificar que se guardó

```bash
crontab -l
```

Debe mostrar la línea recién añadida.

## Paso 4 — Comprobar que cron lo ejecuta

Esperar 5 minutos y comprobar el log de cron:

```bash
journalctl -u cron | tail -10
```

Luego verificar que el script escribió en el log:

```bash
cat /var/log/check_servicios.log
```

## Errores encontrados
<div class="error-box">
grep: /var/log/syslog: binary file matches
</div>
El syslog está en formato binario en versiones recientes de Ubuntu. Usar `journalctl` en su lugar:

```bash
journalctl -u cron | tail -10
```

**El log no se actualiza — problema de permisos**

El archivo `/var/log/check_servicios.log` fue creado con `sudo`, por lo que pertenece a root. Cuando cron ejecuta el script como usuario normal no tiene permiso de escritura.

Solución:

```bash
sudo chmod 666 /var/log/check_servicios.log
```

Esperar 5 minutos y volver a comprobar que aparece la entrada nueva.

## Output esperado
```
=== 2026-06-08 12:32:05 ===
nginx: active
ssh: inactive
cron: active
```