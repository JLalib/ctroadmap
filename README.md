# 🗺️ CTRoadmap

[![GitHub](https://img.shields.io/badge/GitHub-NoobCity99%2FCTRoadmap-181717?logo=github)](https://github.com/NoobCity99/CTRoadmap)
[![Docker](https://img.shields.io/badge/Docker-ghcr.io%2Fnoobcity99%2Fctroadmap-2496ED?logo=docker)](https://github.com/NoobCity99/CTRoadmap/pkgs/container/ctroadmap)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 📋 Descripción general

**CTRoadmap** es una herramienta web open source diseñada específicamente para documentar y visualizar la topología completa de infraestructura homelab, permitiendo crear diagramas interactivos con soporte para nodos, servicios, containers, volúmenes, scripts y relaciones tipadas sin necesidad de ejecutar comandos en la infraestructura (**documentación pura**).

Es la alternativa moderna a draw.io/Visio especializada en homelabs y infraestructura técnica. Desarrollada en **Node.js + React**, ligera, multi-arquitectura (amd64, arm64) y bajo licencia **MIT**.

## ✨ Características principales

- 🎨 **Diagramas interactivos drag-and-drop** — Arrastra elementos, reorganiza topología visualmente de forma intuitiva
- 🧱 **13 tipos de tiles tipados** — Nodos, servicios, containers, drives, mounts, scripts, configs, secretos, flows, IoT, URLs, checks, notas
- 🔗 **Relaciones tipadas con labels** — Conecta tiles con relaciones, agrega labels, notas, directionality
- 🔍 **Búsqueda + filtro instantáneo** — Encuentra tiles rápidamente (`/` para search), filtra por tipo o características
- 🌳 **Nodos padres + hijos anidados** — Organiza tiles dentro de tiles, jerarquía clara, topología modular
- 🔐 **Passcode auth local** — Autenticación opcional en browser, protege acceso local
- 🌓 **Tema claro/oscuro** — Eye-friendly, toggle entre themes, preferencia guardada
- ⌨️ **Hotkeys productivos** — `Ctrl/Cmd+S` save, `Ctrl/Cmd+D` duplicate, `Delete` elimina
- 📦 **Export/Import JSON** — Portabilidad, backup diagrama, versioning via git
- 📱 **Responsive design** — Funciona en mobile y desktop
- 📄 **Documentación integrada** — Flow steps, check commands (documentación pura, sin ejecución)
- 🛣️ **Routing connectors avanzado** — Through tiles o avoid, visualización neta de relaciones
- ☁️ **Zero dependencies ejecutables** — Sin SSH, Docker calls, live checks. Local browser-based
- 🐳 **Docker easy deploy** — Multiarch (amd64, arm64), imagen ligera ~50-200 MB RAM
- 🟢 **MIT open source** — Código abierto, beta version, comunidad activa en desarrollo

## 📋 Requisitos del sistema

- Docker
- Docker Compose v2
- 50 MB - 200 MB RAM mínimo (Node.js ligero)
- 50 MB espacio disco (imagen + BD local)
- Puerto 8088 (configurable)
- Volumen persistente para data (diagrama JSON)
- curl (para instalador script)
- Navegador moderno (Chrome, Firefox, Safari, Edge)
- Opcional: git (para versionado diagrama)

## 🐳 Instalación

### Opción 1: Script instalador (recomendado)

```bash
curl -fsSL https://raw.githubusercontent.com/NoobCity99/CTRoadmap/main/CTR_install.sh -o CTR_install.sh
chmod +x CTR_install.sh
./CTR_install.sh
```

**One-liner alternativo:**

```bash
curl -fsSL https://raw.githubusercontent.com/NoobCity99/CTRoadmap/main/CTR_install.sh | bash
```

### Opción 2: Instalación custom (directorio específico)

```bash
CTR_INSTALL_DIR=/opt/ctroadmap-beta ./CTR_install.sh
```

### Opción 3: Docker Compose manual

```bash
mkdir -p ctroadmap-beta
cd ctroadmap-beta
cat > docker-compose.yml << 'EOF'
version: '3.8'
services:
  ctroadmap:
    image: ghcr.io/noobcity99/ctroadmap:beta
    container_name: ctroadmap
    restart: unless-stopped
    ports:
      - "8088:3000"
    volumes:
      - ctroadmap_data:/app/data
    environment:
      - NODE_ENV=production
volumes:
  ctroadmap_data:
EOF
docker compose up -d
```

### Acceder (primer uso)

```
http://localhost:8088  →  CTRoadmap (dibujar infraestructura)
```

### Verificar instalación

```bash
docker compose logs -f
# O desde directorio ctroadmap-beta
cd ~/ctroadmap-beta
docker compose up -d
```

## ⚙️ Configuración

1. **Puerto externo** — Modifica `"8088:3000"` en `docker-compose.yml` si necesitas otro puerto
2. **Directorio de datos** — El volumen `ctroadmap_data` persiste el diagrama JSON en `/app/data`
3. **Variables de entorno** — `NODE_ENV=production` para modo producción
4. **Passcode auth** — Configurable desde la UI en Settings → Passcode (opcional)
5. **Tema** — Light/Dark mode configurable en Settings → Theme (preferencia guardada en localStorage)

## 🚀 Primeros pasos

1. **Acceder a CTRoadmap**  
   Abre `http://localhost:8088` — Canvas en blanco aparece (listo para dibujar). Cero contraseña necesaria (passcode opcional en Settings).

2. **Crear primer nodo (servidor)**  
   Click derecho en canvas → "Add Tile" o botón "+" → Tipo: "Node" (servidor, máquina física) → Nombre: "Server Principal" → Aparece rectángulo en canvas (movible).

3. **Agregar servicios dentro nodo**  
   Dentro del nodo padre, click derecho Add Tile → "Service" → Nombre: "Docker" (o "Nginx", "PostgreSQL", etc) → Service aparece como hijo del nodo (indentado/anidado).

4. **Agregar containers**  
   Dentro nodo/servicio, Add Tile → "Container" → Nombre: "WordPress", "MySQL", etc → Construye jerarquía: Node → Service → Container.

5. **Crear relaciones (conectar tiles)**  
   Click en tile → "Create Relationship" o drag-connect → Selecciona otro tile destino → Línea conecta tiles automáticamente → Click relación → edita label, notas, directionality.

6. **Documentar con notas y flows**  
   Add Tile → "Note" o "Flow" → Note: texto libre (documentación pura) → Flow: pasos workflow (sin ejecutar, solo doc) → Conecta con relaciones a tiles relevantes.

7. **Agregar storage y volúmenes**  
   Add Tile → "Drive" (almacenamiento físico) → Add Tile → "Mount" (puntos montaje) → Conecta mounts a drives con relaciones → Documenta punto montaje e información.

8. **Crear diagrama completo típico**  
   Estructura ejemplo:
   ```
   Homelab Node (primario)
   ├─ Docker Service
   │  ├─ Container: WordPress
   │  ├─ Container: MariaDB
   │  └─ Container: Nginx
   ├─ Disk Storage (Drive)
   └─ Network Check (verificar conectividad)
   ```

9. **Buscar y filtrar tiles**  
   Presiona `/` en canvas → Search box aparece → Escribe nombre o tipo tile → Resultados instantáneos.

10. **Guardar diagrama (Ctrl+S)**  
    `Ctrl+S` en browser → O Settings → Export JSON para backup manual.

11. **Activar passcode auth (opcional)**  
    Settings → Passcode → Ingresa passcode (protege acceso local) → Siguiente visitor debe ingresar passcode.

12. **Cambiar tema (dark mode)**  
    Settings → Theme → Toggle Dark/Light → Preferencia guardada automático.

## 💡 Casos de uso

- 📚 **Documentación infraestructura** — Mapear setup homelab completo, referencia visual, compartir con equipo
- 🎯 **Planning nuevos servicios** — Diseñar topología antes deploy, iteración visual, decisiones informadas
- 🔧 **Troubleshooting** — Entender relaciones, seguir flows, root cause diagnosis
- 👥 **Onboarding equipo** — Mostrar setup visual, training material, menos time explaining
- 💾 **Backup/recovery planning** — Documentar dependencies, criticality, recovery flows
- ✅ **Compliance/auditoría** — Documentar infraestructura, mostrar security controls, checks integrados

## 🔒 Acceso remoto seguro

### HTTPS con Caddy (producción)

```caddyfile
ctroadmap.tudominio.com {
    reverse_proxy localhost:8088
}
```

**Acceso remoto seguro:** `https://ctroadmap.tudominio.com` con HTTPS automático

> **IMPORTANTE:** Passcode protege acceso. Activar passcode en Settings para proteger diagrama infraestructura (información sensible). Combina con HTTPS para security máximo.

## 🛠️ Gestión y mantenimiento

### Ver logs
```bash
cd ~/ctroadmap-beta
docker compose logs -f
```

### Backup de diagrama
```bash
# Desde UI: Settings → Export JSON
# O backup volume directamente:
docker cp ctroadmap:/app/data ./ctroadmap-backup-$(date +%Y%m%d)
```

### Restore diagrama
```bash
docker stop ctroadmap
docker cp ./ctroadmap-backup-YYYYMMDD/data ctroadmap:/app/
docker start ctroadmap
```

### Gestionar desde directorio instalación
```bash
cd ~/ctroadmap-beta
# Ver logs
docker compose logs -f
# Reiniciar
docker compose down && docker compose up -d
# Actualizar imagen
docker compose pull && docker compose up -d
# Eliminar instalación (desinstalar)
curl -fsSL https://raw.githubusercontent.com/NoobCity99/CTRoadmap/main/CTR_uninstall.sh | bash
```

### Monitorear consumo
```bash
docker stats ctroadmap
# Típicamente: ~50-200 MB RAM, CPU bajo
```

### Versionado diagrama con git
```bash
cd ~/ctroadmap-beta/ctroadmap_data
git init
git add .
git commit -m "Initial infrastructure diagram"
# Cada cambio puede git-tracked para auditoría/history
```

## 📝 Licencia

Este proyecto está licenciado bajo la **Licencia MIT** — ver el archivo [LICENSE](LICENSE) para más detalles.

---

> **Referencia:** Basado en el post ["Cómo instalar CTRoadmap en Docker - Visualización y documentación de infraestructura homelab autohospedada"](https://genbyte.blogspot.com/2026/08/como-instalar-ctroadmap-en-docker.html) de Genbyte.