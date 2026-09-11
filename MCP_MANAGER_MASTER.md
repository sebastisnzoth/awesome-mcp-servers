# MCP Manager — Documento Maestro

## Objetivo
Construir un gerenciador web de servidores MCP que permita descubrir, organizar, configurar, activar, validar y exportar stacks MCP por proyecto.

## Fuente de catálogo
Este repositorio `awesome-mcp-servers` funcionará inicialmente como fuente de descubrimiento y referencia de servidores MCP.

## Principios
- Mantener el catálogo separado de las configuraciones sensibles.
- Nunca almacenar API keys o secretos en GitHub.
- Permitir perfiles por proyecto.
- Priorizar configuración portable y exportable.
- Incorporar diagnóstico y health checks desde el MVP.
- Mantener compatibilidad progresiva con clientes MCP como Codex y otros entornos compatibles.

## MVP

### 1. Dashboard
- MCP instalados/configurados
- MCP activos
- MCP con error
- Proyectos configurados
- Últimos health checks

### 2. Catálogo MCP
- Buscar por nombre
- Filtrar por categoría
- Ver repositorio fuente
- Ver método de instalación
- Marcar favoritos
- Agregar a proyecto

### 3. Proyectos
Cada proyecto tendrá su propio perfil MCP.

Ejemplos iniciales:
- UGO Desarrollo
- UGO Testing
- UGO Producción
- Diseño
- GitHub / Codex

### 4. Configurador
- Activar/desactivar servidores
- Definir comando
- Definir argumentos
- Definir variables de entorno por referencia
- Generar JSON de configuración
- Copiar configuración
- Descargar configuración

### 5. Diagnóstico
- Estado de configuración
- Dependencias requeridas
- Comando válido
- Variables faltantes
- Health check
- Logs resumidos

## Arquitectura propuesta

### Frontend
- React
- Vite
- TypeScript

### Backend / persistencia
- Supabase

### Deploy
- Vercel

### Integraciones
- GitHub para catálogo y repositorios
- Supabase para proyectos, perfiles y metadata
- Runtime local o agente auxiliar para health checks que requieran ejecución local

## Modelo de datos inicial

### mcp_servers
- id
- name
- slug
- description
- category
- repository_url
- docs_url
- install_command
- transport
- status
- metadata

### projects
- id
- name
- description
- environment

### project_mcp_servers
- id
- project_id
- mcp_server_id
- enabled
- command
- args
- env_schema
- config

### health_checks
- id
- project_mcp_server_id
- status
- message
- checked_at

## Seguridad
- No persistir secretos en texto plano.
- Usar variables de entorno o secret stores del runtime.
- Separar metadata pública de credenciales.
- Validar comandos y argumentos antes de ejecución.
- Tratar servidores externos como código no confiable hasta verificar origen y mantenimiento.

## Roadmap

### Fase 1 — Base
- Documento maestro
- Estructura frontend
- Catálogo mock
- Dashboard
- Proyectos
- Generador JSON

### Fase 2 — GitHub
- Importación desde awesome-mcp-servers
- Sincronización de metadata
- Detección de repositorios archivados/inactivos

### Fase 3 — Diagnóstico
- Health checks
- Dependencias
- Validación de transportes
- Logs

### Fase 4 — Inteligencia
- Recomendación de MCP según stack del proyecto
- Perfiles sugeridos
- Detección de redundancias
- Score de confianza / mantenimiento

## Perfil inicial recomendado para UGO
- GitHub
- Filesystem
- Supabase/PostgreSQL
- Playwright
- Fetch/API
- Mapas/geolocalización
- Observabilidad

## Regla operativa
El MCP Manager administra configuraciones y metadata. La ejecución de servidores MCP que necesiten acceso local debe hacerse en un runtime explícitamente autorizado, no desde el navegador sin mediación.
