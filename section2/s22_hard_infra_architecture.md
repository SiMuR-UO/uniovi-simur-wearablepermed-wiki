---
layout: default
title: "Hard Infrastructure"
nav_order: 2
parent: "System Architecture"
---

# Description
This diagram represent the hardware infrastructure that SIMUR group offer to its projects

- **Server Node**: this node represent the servers where we train our models. 

- **NAS Nodes**: This node represent the servers where the input and output files are synchronized and mirror all the time to be preserved.

If you need access to some of these repositories contact with [Admin Support](mailto:amlopez@uniovi.es)

<object
    data="{{ site.baseurl }}/section2/assets/images/hard_infra_architecture.svg"
    type="image/svg+xml"
    style="max-width: 800px; width: 100%;">
</object>

# Protocolo de administración y uso: Dataset WPM (v2.1)

**Sistema:** Synology DS225+ | **Servidores:** Ameno / Aburrido | **Fecha:** Mayo 2026

---

## 1. Acceso al sistema (NAS)

| Campo | Valor |
|---|---|
| Acceso web (prioritario) | `quickconnect.to/simur-wearablepermed` |
| App móvil / ID QuickConnect | `simur-wearablepermed` |
| Usuario administrador | `******` |
| Contraseña | `******` |

> **Nota para usuarios de Linux:** Usar la interfaz web o el cliente de escritorio con sincronización selectiva. La función "Sincronización a petición" no está disponible en Linux.

---

## 2. Estructura de directorios en el NAS

El acceso se centraliza en el grupo `wpm-dataset-group`. Todos los miembros pueden ver todas las subcarpetas de experimentos.

| Carpeta | Contenido | Permisos |
|---|---|---|
| `/wpm-dataset` | Dataset crudo recopilado en CLM | Solo lectura (inalterable) |
| `/wpm-curated-dataset` | Dataset curado por SiMuR | Solo lectura (inalterable) |
| `/wpm-experiments` | Datos de cada experimento | Lectura general |
| `/wpm-experiments/nom_exp/` | Carpeta específica por experimento | Escritura: responsable + asignados |

---

## 3. Usuarios en servidores (Ameno / Aburrido)

Se crea **un único usuario por experimento** para tareas de entrenamiento. Si participa más de un investigador, deben compartir la cuenta o delegar el acceso en uno de ellos.

> **Importante:** El acceso a experimentos distintos al propio debe realizarse mediante **NFS o SMB**, nunca sincronizando carpetas adicionales al servidor, para no sobrecargar el almacenamiento local.

---

## 4. Procedimiento para nuevo experimento (administrador)

### 4.1 Espacio de trabajo en el NAS

1. Crear la subcarpeta `/wpm-experiments/nom_exp/` en File Station.
2. Asignar permiso de **escritura** al investigador responsable e investigadores asignados desde la pestaña "Permisos" de File Station.

### 4.2 Cuenta en servidor Ameno / Aburrido _(si se requiere entrenamiento)_

1. Crear el usuario en el servidor correspondiente (Ameno o Aburrido).
2. Sincronizar **exclusivamente** la carpeta del experimento (`/wpm-experiments/nom_exp/`) en la carpeta local del usuario. No sincronizar otras carpetas.
3. Para acceder a otros experimentos, configurar acceso vía NFS o SMB al NAS.
