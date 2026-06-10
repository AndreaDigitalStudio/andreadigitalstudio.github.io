---
layout: post
title: "Configurar SSH entre dos VMs"
date: 2026-06-10
categories: linux
---

## Para qué sirve

SSH con claves evita usar contraseña en cada conexión. VM1 genera un par de claves: la privada se queda en VM1 y nunca sale, la pública se copia a VM2. Al conectarse, VM2 comprueba si la clave pública coincide con la privada — si coincide, acceso directo.

## Requisitos

- VirtualBox con una VM Ubuntu ya configurada
- Conexión a internet para instalar paquetes

## Paso 1 — Crear la segunda VM 

Necesitas dos VMs Ubuntu. 

Si solo tienes una, clónala: *clic derecho → **Clonar** → **Clonación completa***.

## Paso 2 — Configurar red

Las dos VMs necesitan verse entre ellas y tener acceso a internet para instalar paquetes. Usar **Red NAT** (no NAT simple).

> **NAT vs Red NAT**  
> - NAT da internet a la VM pero las VMs no se ven entre ellas.  
> - Red NAT da internet y permite comunicación entre VMs de la misma red.

Crear la Red NAT primero en VirtualBox:

*Archivo → Herramientas → Network Manager → Nueva red NAT*

Aplicar la misma red NAT en ambas VMs:

*Configuración → Red → Adaptador 1 → Red NAT → [nombre de tu red]*

Verificar conectividad desde cada VM:

```bash
ping 8.8.8.8
```

Anotar la IP de cada VM:

```bash
ip a
```

## Paso 3 — Instalar OpenSSH en VM2 (si no lo tienes)

```bash
sudo apt install openssh-server -y
```

## Paso 4 — Generar claves en VM1

```bash
ssh-keygen -t ed25519
```

Pulsar Enter a todo. Genera dos archivos en `~/.ssh/`:

- `id_ed25519` — clave privada (no compartir nunca)
- `id_ed25519.pub` — clave pública (esta se copia a VM2)

## Paso 5 — Copiar la clave pública a VM2

Ejecutar desde VM1:

```bash
ssh-copy-id usuario@IP_VM2
```

Pedirá la contraseña de VM2 por última vez.

## Paso 6 — Conectarse sin contraseña

Desde VM1:

```bash
ssh usuario@IP_VM2
```

## Paso 7 — Ejecutar comandos remotos sin entrar en VM2
Una de las ventajas de SSH con claves es poder lanzar comandos en una máquina remota sin conectarte interactivamente. Útil para scripts de monitorización o automatización.

Por ejemplo, ver el uso de disco de VM2 desde VM1:

```bash
ssh usuario@IP_VM2 "df -h"
```

El resultado aparece directamente en la terminal de VM1.

## Errores encontrados

**Connection refused al hacer ssh-copy-id**

<div class="error-box">
ssh: connect to host 192.168.56.102 port 22: Connection refused
</div>

**Causa:** OpenSSH no estaba instalado en VM2. Solución: instalar `openssh-server` primero.

**Error al instalar openssh-server**

<div class="error-box">
Temporary failure resolving 'es.archive.ubuntu.com'
</div>

**Causa:** VM2 sin acceso a internet. La configuración de red era NAT simple en lugar de Red NAT. Solución: crear una Red NAT en Network Manager y aplicarla a ambas VMs.