<p align="center">
  <img src="../assets/logo.svg" width="72" alt="ARMAGGEDON">
</p>

<h1 align="center">Informe de Mision — Plantilla de ejemplo</h1>

<p align="center">
  <img alt="ejemplo" src="https://img.shields.io/badge/documento-EJEMPLO-orange">
  <img alt="ficticio" src="https://img.shields.io/badge/datos-ficticios-lightgrey">
  <a href="INDICE.md">Volver al indice</a>
</p>

> **Esto es una plantilla.** Todos los datos (objetivo, hallazgos, hashes) son
> **ficticios** y sirven solo para mostrar el formato. Genera informes reales solo
> para objetivos autorizados y con evidencia verificada. ARMAGGEDON produce
> **borradores**; una persona revisa y envia.

---

## 1. Resumen ejecutivo

| Campo | Valor |
|---|---|
| **Operacion** | `OP-EJEMPLO-001` |
| **Objetivo autorizado** | `https://staging.ejemplo-interno.test` (propiedad del cliente) |
| **Ventana de pruebas** | 2026-11-10 09:00 → 2026-11-12 18:00 (UTC) |
| **Autorizacion** | Contrato firmado `SOW-2026-114`, alcance adjunto |
| **Modo de ejecucion** | Agente local (sin clave) · recon en vivo |
| **Modelo / harness** | Agente de codigo local + Arsenal de ARMAGGEDON |

**Veredicto:** se identificaron **3 hallazgos** (1 alto, 1 medio, 1 bajo). Ninguno
requirio explotacion destructiva; todos se validaron con evidencia reproducible.
El riesgo agregado es **medio-alto** por el hallazgo A-01 (exposicion de credencial
en configuracion).

```
Severidad    Cantidad
Critico      0
Alto         1   ███
Medio        1   ███
Bajo         1   ███
Informativo  2   ██████
```

## 2. Alcance y reglas de enganche

- **En alcance:** el host `staging.ejemplo-interno.test` y sus subdominios.
- **Fuera de alcance:** produccion, cualquier host de terceros, loopback y redes
  privadas. La contencion de egreso de ARMAGGEDON rechazo automaticamente
  `SCOPE DENIED` 4 intentos fuera de alcance.
- **Prohibido:** denegacion de servicio, modificacion de datos, exfiltracion real.

## 3. Metodologia

El motor de reconocimiento recorrio la cadena **recon → escaneo → validacion**
con herramientas reales; cada hallazgo se ata a la salida del comando que lo
produjo (procedencia verificada, sin "banderas fantasma").

```
nmap          → superficie de puertos/servicios
DNS           → enumeracion de subdominios
HTTP probes   → fingerprinting de stack y cabeceras
analisis      → correlacion y priorizacion por impacto
```

## 4. Hallazgos

### A-01 — Credencial expuesta en archivo de configuracion
<img alt="alto" src="https://img.shields.io/badge/severidad-ALTO-ff4444"> · CWE-200 · CVSS 7.5

- **Descripcion:** el endpoint `/.env.bak` servia una copia con una clave de API
  en texto plano.
- **Evidencia:**
  ```
  GET /.env.bak HTTP/1.1  → 200 OK
  API_TOKEN=sk_ejemplo_0000000000000000  (ficticio, redactado en entrega real)
  ```
- **Impacto:** acceso potencial a la API interna con privilegios de servicio.
- **Remediacion:** eliminar el archivo, rotar la credencial, bloquear `*.bak` en
  el servidor web y mover secretos a un gestor de secretos.

### A-02 — Cabeceras de seguridad ausentes
<img alt="medio" src="https://img.shields.io/badge/severidad-MEDIO-ffaa00"> · CWE-693 · CVSS 5.3

- **Descripcion:** faltan `Content-Security-Policy`, `X-Content-Type-Options` y
  `Strict-Transport-Security`.
- **Remediacion:** añadir las cabeceras en el proxy/inverso o en la app.

### A-03 — Divulgacion de version del servidor
<img alt="bajo" src="https://img.shields.io/badge/severidad-BAJO-0088ff"> · CWE-200 · CVSS 3.1

- **Descripcion:** la cabecera `Server:` revela version exacta del stack.
- **Remediacion:** suprimir u ofuscar la cabecera `Server`.

## 5. Evidencia y reproducibilidad

- Cada hallazgo incluye el comando, la salida cruda y una marca de tiempo en la
  **Boveda de Evidencias**.
- Las acciones intrusivas pasaron por la **compuerta de recibo** (aprobacion
  explicita antes de ejecutarse).
- Reproduccion: ver el paquete exportado `OP-EJEMPLO-001.bundle` (recon + recibos).

## 6. Recomendaciones priorizadas

1. **Inmediato:** eliminar `/.env.bak` y rotar la credencial (A-01).
2. **Corto plazo:** añadir cabeceras de seguridad (A-02).
3. **Higiene:** suprimir banners de version (A-03) y revisar reglas de egreso.

## 7. Anexos

- Reglas de enganche y autorizacion: [SCOPE_AND_AUTHORIZATION.md](SCOPE_AND_AUTHORIZATION.md)
- Como se verifica la procedencia: [VERIFIED_PROVENANCE.md](VERIFIED_PROVENANCE.md)
- Plantilla de recibos de contribucion: [CONTRIBUTION_RECEIPTS.md](CONTRIBUTION_RECEIPTS.md)

---

<p align="center"><em>Generado como borrador por ARMAGGEDON — revisar y validar antes de enviar.</em></p>
