<p align="center">
  <img src="../assets/logo.svg" width="96" alt="ARMAGGEDON">
</p>

<h1 align="center">Documentacion de ARMAGGEDON</h1>

<p align="center">
  Indice central. Empieza aqui para instalar, operar, integrar o contribuir.<br>
  <a href="../README.md">&larr; Volver al README</a> · <a href="../TUTORIAL.md">Tutorial completo</a>
</p>

---

## Empezar

| Documento | Para que sirve |
|---|---|
| [Tutorial (espanol)](../TUTORIAL.md) | Guia completa en espanol: que es, instalacion, componentes, comandos y flujo de trabajo. **El mejor punto de partida.** |
| [Inicio / Getting Started](GETTING_STARTED.md) | Instalacion, primer arranque, primera mision segura, CLI y actualizaciones. |
| [Matriz de instalacion](INSTALL_MATRIX.md) | Preparacion por sistema operativo (macOS / Linux). |

## Operar

| Documento | Para que sirve |
|---|---|
| [Alcance y autorizacion](SCOPE_AND_AUTHORIZATION.md) | Reglas de enganche, recibos de alcance y requisitos de evidencia. |
| [Cabeceras de objetivo](TARGET_HEADERS.md) | Como pasar cabeceras de autenticacion para un objetivo autorizado. |
| [Flujos autenticados](AUTHENTICATED_WORKFLOWS.md) | Soporte actual de peticiones autenticadas y el limite de aprobacion. |
| [Plan de activacion del Arsenal](ARSENAL_ACTIVATION_PLAN.md) | Instalar herramientas externas opcionales. |
| [Informe de ejemplo](INFORME_EJEMPLO.md) | Plantilla de informe de una mision, lista para rellenar. |

## Desarrollar e integrar

| Documento | Para que sirve |
|---|---|
| [Guia de desarrollo](DEVELOPER_GUIDE.md) | Mapa del codigo fuente, scripts, SDK y puntos de extension. |
| [Referencia de la API](API_REFERENCE.md) | Rutas de la API HTTP local, agrupadas por flujo. |
| [Guia MCP](MCP_GUIDE.md) | Servidor Model Context Protocol y la herramienta `security_recon`. |
| [Recibos de contribucion](CONTRIBUTION_RECEIPTS.md) | Plantilla de evidencia para PRs y cambios de afirmaciones. |

## Benchmarks y procedencia

| Documento | Para que sirve |
|---|---|
| [Procedencia verificada](VERIFIED_PROVENANCE.md) | Como los hallazgos se atan a evidencia de herramientas. |
| [XBOW / XBEN](XBOW_BASELINE.md) · [Cybench](CYBENCH.md) | Notas de los benchmarks. |
| [Forense del muro](WALL_FORENSICS.md) | Fallos por reto y analisis de benchmark. |
| [Libro de integridad](INTEGRITY_LEDGER.md) | Auditoria de contaminacion y retractaciones. |
| [Matriz de modelos](MODEL_MATRIX.md) | Matriz reproducible de benchmark entre modelos. |
| [OBSIDIVM](OBSIDIVM.md) | Campo web local propio y notas de evaluacion. |

## Release y mantenimiento

| Documento | Para que sirve |
|---|---|
| [Lista de release](RELEASE_CHECKLIST.md) | Los filtros que un release debe pasar. |
| [Changelog](CHANGELOG.md) | Historial de cambios. |
| [Docker](DOCKER.md) | Guia de despliegue en contenedor. |

---

## Mapa rapido del sistema

```
ARMAGGEDON
├─ Sala de Guerra (UI web)      → npm run server → http://127.0.0.1:3333/ui/
├─ CLI                          → npx tempest
├─ API HTTP                     → POST /api/mission/start, GET /api/mission/status
├─ Servidor MCP                 → node dist/mcp-server.js  (expone security_recon)
├─ Arsenal                      → 36 herramientas (111 con T3MP3ST_FULL_ARSENAL)
├─ 8 operadores                 → Recon · Scanner · Exploiter · Infiltrator ·
│                                 Exfiltrator · Ghost · Coordinator · Analyst
└─ Verificacion                 → npm run verify-claims  (recalcula cada cifra)
```

> Recuerda: consigue permiso, mantente dentro del alcance y usa ARMAGGEDON solo
> contra objetivos que te pertenecen o para los que tienes autorizacion explicita.
