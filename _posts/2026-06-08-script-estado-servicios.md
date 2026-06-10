---
layout: post
title: "Script — Comprobar estado de servicios"
date: 2026-06-08
categories: linux
---

## Para qué sirve

Verifica si los servicios esenciales del servidor están activos
y guarda el resultado en un log.

## El script

Crear el fichero:

```bash
nano ~/check_servicios.sh
```

Contenido:

```bash
#!/bin/bash

LOG="/var/log/check_servicios.log"
FECHA=$(date '+%Y-%m-%d %H:%M:%S')

echo "=== $FECHA ===" >> "$LOG"

for servicio in nginx ssh cron; do
    if systemctl is-active --quiet "$servicio"; then
        echo "$servicio: active" | tee -a "$LOG"
    else
        echo "$servicio: inactive" | tee -a "$LOG"
    fi
done
```

Hacerlo ejecutable:

```bash
chmod +x ~/check_servicios.sh
./check_servicios.sh
```

## Output esperado
```
=== 2026-06-08 12:32:05 ===
nginx: active
ssh: inactive
cron: active
```