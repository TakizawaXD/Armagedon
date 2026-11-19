# Armaggedon — Tutorial de uso

Guia en espanol para entender, instalar y operar el sistema. Esta pensada para
alguien que llega por primera vez y quiere saber para que sirve cada cosa, en
que orden usarla y donde estan los limites de lo que ya funciona.

> Nota sobre el nombre: en esta guia el sistema se llama **Armaggedon**. Los
> comandos tecnicos (paquete, binario `tempest`, variables `TEMPEST_*` y
> `T3MP3ST_*`, rutas de configuracion) todavia conservan el identificador
> original del codigo. Cuando leas `tempest` o `t3mp3st` en un comando,
> entiende que es el motor de Armaggedon.

---

## 1. Que es Armaggedon

Armaggedon es un **framework de seguridad ofensiva multi-agente**. Su idea
central es convertir el agente de programacion con IA que ya usas (Claude Code,
Codex, Hermes, OpenCode, Oh My Pi) o un modelo que corras **totalmente local**
(Ollama, LM Studio, vLLM) en un cazador de vulnerabilidades.

En lugar de pedirte una nueva clave de API o una cuenta en la nube, Armaggedon
usa como "cerebro" el agente que ya tienes conectado y monta a su alrededor toda
la maquinaria de ataque: reconocimiento, explotacion y reporte. Tu agente piensa;
Armaggedon pone el arsenal.

Tres principios lo definen:

1. **Reproducible.** Cada cifra de rendimiento se vuelve a calcular desde datos
   ya guardados en el repo con un solo comando (`npm run verify-claims`). Si un
   numero no se puede reproducir, no se publica.
2. **Sin llaves (keyless).** El agente de IA que ya corre en tu maquina es la
   columna vertebral. No hace falta otra clave ni otra factura.
3. **Honesto sobre el alcance.** Hay una tabla de estado que marca que es
   estable, que es experimental y que es solo plan a futuro.

### Uso autorizado unicamente

Armaggedon es una herramienta **ofensiva**, pensada para pruebas **autorizadas**,
investigacion y educacion. Apuntala **solo** a sistemas que son tuyos o para los
que tienes permiso **explicito y por escrito**. El acceso no autorizado a
computadoras, redes o datos es ilegal en casi todas las jurisdicciones, y la
responsabilidad es tuya. Se entrega **tal cual, bajo licencia AGPL-3.0, sin
garantia ni responsabilidad** por danos o mal uso. Consigue permiso, mantente
dentro del alcance acordado.

---

## 2. Instalacion y primer arranque

Requisitos: **Node.js** y **npm** en tu PATH. Para actualizaciones y algunos
benchmarks tambien **git**.

### Camino mas rapido: la Sala de Guerra (War Room)

```bash
npm install
npm run server        # Sala de Guerra -> http://127.0.0.1:3333/ui/
```

Abre esa direccion en el navegador. Dentro:

1. Entra en **Settings** (Ajustes) y conecta un agente local (Claude Code,
   Codex, Hermes, OpenCode u Oh My Pi).
2. Describe un objetivo autorizado en lenguaje natural a **Op Admiral** (el
   coordinador) y lanza la mision.

El agente que conectaste es el cerebro. No necesitas ninguna clave.

### Alternativa: traer tu propia clave

Si prefieres un proveedor con clave en vez de un agente local, exporta una de
estas variables y te saltas el paso de conectar agente:

```bash
export OPENROUTER_API_KEY=...     # o VENICE_API_KEY / ANTHROPIC_API_KEY / OPENAI_API_KEY
export XAI_API_KEY=...            # modelo de codigo de xAI, con tool-calling nativo
export NOVITA_API_KEY=...         # API hospedada compatible con OpenAI
```

Novita es un proveedor externo opcional. Si lo usas, se envian a Novita el id del
modelo, los prompts y contexto, la salida generada y metadatos de la peticion.
Revisa su politica de privacidad antes de enviar datos sensibles.

### Alternativa: totalmente offline (sin clave, sin nube)

Por defecto usa Ollama; tambien sirve cualquier servidor compatible con OpenAI
(LM Studio, vLLM, llama.cpp):

```bash
ollama serve && ollama pull llama3
export TEMPEST_LOCAL_BASE_URL=http://localhost:11434/api    # LM Studio: http://localhost:1234/v1
export TEMPEST_LOCAL_MODEL=llama3
npm run build                                               # solo si clonaste el repo
npx tempest                                                 # -> "Change default provider" -> local
```

El tool-calling funciona sobre cualquier modelo local (se maneja por texto), asi
que el arsenal corre incluso en modelos sin function-calling nativo.

### Verificar que todo cuadra

```bash
npm run verify-claims     # recalcula cada cifra del README desde los JSON en bench/
npm run doctor            # revisa el entorno y la configuracion
```

### Con Docker

```bash
cp .env.example .env       # configura las claves
docker compose up -d       # API -> http://localhost:3333
docker compose logs -f     # ver registros
```

El contenedor se enlaza a `127.0.0.1:3333`: accesible solo desde tu maquina, no
expuesto a la red.

---

## 3. Los 8 operadores (la celula de agentes)

Armaggedon es una **cadena de ataque (kill chain) de 8 operadores**. Cada
operador corresponde a una fase de la metodologia MITRE ATT&CK y de la Cyber Kill
Chain. Hoy el **reconocimiento esta vivo** (motor real con herramientas); las
fases posteriores corren el mismo bucle real pero aun no estan probadas como
enjambre coordinado.

| Operador | Fase | Para que sirve |
|---|---|---|
| **Recon** | Reconocimiento | OSINT, descubrimiento de red, enumeracion de activos |
| **Scanner** | Descubrimiento | escaneo de vulnerabilidades, huella de servicios |
| **Exploiter** | Acceso inicial | explotacion y entrega de payloads |
| **Infiltrator** | Movimiento lateral | post-explotacion, escalada de privilegios |
| **Exfiltrator** | Recoleccion / Exfiltracion | extraccion de datos, recoleccion de credenciales |
| **Ghost** | Persistencia | persistencia, sigilo, limpieza |
| **Coordinator** | Comando y control | control de la mision, orquestacion |
| **Analyst** | Analisis | analisis de patrones, generacion de reportes |

Como leerlo: **Recon** es el unico operador con resultados probados y herramientas
reales detras. Los demas usan el mismo motor (un bucle ReAct real con
herramientas de verdad, no simuladas), pero las cifras publicadas las logro un
**solo agente**, no la celula coordinada de 8. El motor es real; el enjambre es la
parte que aun se esta ganando su reputacion.

---

## 4. Que caza y en que dominios

Un "dominio" solo se marca como estable cuando hay un resultado reproducible
detras. Estado actual:

| Dominio | Que cubre | Estado |
|---|---|---|
| **Web** | apps, APIs, flujos de autenticacion, OWASP Top 10 | Estable (nucleo), 90.1% pass@1 en XBEN |
| **Codigo fuente** | auditoria white-box, caza estilo SAST | Probado en una caza (CVE-Zero); el motor de ingesta sigue experimental |
| **CTF** | wargames, retos, campos de practica | Probado (Cybench 23/40 sin pistas) |
| **Red / Infraestructura** | recon, huella de servicios; lateral y privesc | Recon estable; lateral/privesc experimental |
| **Embebido / IoT / OT** | firmware, robotica, ICS/SCADA de codigo abierto | Pipeline de divulgacion coordinada en vivo |
| **Cadena de suministro** | auditoria de dependencias | Real, clase dedicada |
| **Blockchain** | smart contracts, DeFi, Solidity | Solo reproduccion (Damn Vulnerable DeFi), no descubrimiento novedoso |
| **Nube (IaC)** | errores de configuracion AWS/GCP/Azure, IAM, serverless | En desarrollo |
| **Movil** | seguridad de apps Android/iOS | En desarrollo |
| **Identidad / AD** | Kerberos, pass-the-hash, ataques a Active Directory | En desarrollo |
| **Binario / RE** | overflows, ROP, desarrollo de exploits | En desarrollo, requiere herramientas especializadas |

La arquitectura por clases y escuadrones hace que los dominios nuevos se
**compongan** en vez de bifurcar el codigo: cada uno es una configuracion
(clases especialistas + arsenal + adaptador de objetivo + un benchmark).

---

## 5. Componentes principales, explicados

### Sala de Guerra (War Room)

La interfaz web (`npm run server`, en `http://127.0.0.1:3333/ui/`). Desde ahi
conectas agentes, describes objetivos a Op Admiral, lanzas misiones y sigues su
avance. Es el punto de entrada mas comodo.

### Op Admiral

El coordinador en lenguaje natural dentro de la Sala de Guerra. Le explicas el
objetivo autorizado en espanol o ingles corriente y el arma la mision.

### Motor de misiones

El orquestador que ejecuta la cadena de ataque a traves de los operadores. Es la
parte que mantiene el estado de la mision, reparte tareas y recoge evidencias.

### Motor de reconocimiento (Recon)

El componente mas maduro. Maneja nmap, DNS, HTTP y fingerprinting reales. Cada
hallazgo se puede rastrear hasta la salida real de una herramienta, no a una
afirmacion del modelo.

### Arsenal

El conjunto de herramientas que los operadores pueden ejecutar. Por defecto trae
**36 herramientas**. Con la opcion `T3MP3ST_FULL_ARSENAL` se amplia a **111**
(suma 75 adaptadores). Las herramientas peligrosas (metasploit, hydra, pacu,
frida) quedan tras rutas aprobadas y estrechas, no tras ejecucion generica.

### Contencion de alcance (Egress-scope)

Proteccion activa por defecto. Cuando fijas un objetivo de mision, las
herramientas con red se niegan a tocar hosts publicos fuera de alcance: no el
objetivo ni sus subdominios, no loopback ni direcciones privadas, devuelve
`SCOPE DENIED`. Es un valor por defecto endurecido, no un lanzador de herramientas
sin control.

### Pipeline de divulgacion coordinada

Para hallazgos en software de codigo abierto (robotica, OT, embebido). Combina
comprobacion de novedad contra OSV, un PoC en vivo, un panel "refutador" y
calculo de CVSS. Genera **borradores**; siempre es un humano quien los envia.

### Analisis de codigo white-box

Ingesta multi-lenguaje con web-tree-sitter (Python, JS, TS, Go, Java, C, C++).
Python conserva su parser por regex; los demas lenguajes, si fallan, lo hacen de
forma segura devolviendo cero bloques. Estado experimental.

### Servidor MCP y API HTTP

- `node dist/mcp-server.js` expone la herramienta `security_recon` a agentes que
  hablan Model Context Protocol.
- `npm run server` levanta la API HTTP (`POST /api/mission/start`,
  `GET /api/mission/status`, y mas).

### Boveda de evidencias, almacen de credenciales y libro de hallazgos

Las capas de persistencia de una mision: donde se guardan PoCs, capturas,
credenciales recogidas y el registro de hallazgos.

---

## 6. Comandos mas usados

Verificacion y diagnostico:

```bash
npm run verify-claims     # recalcula todas las cifras desde bench/
npm run doctor            # revisa entorno y configuracion
npm run setup             # asistente de configuracion inicial
```

Arranque:

```bash
npm run server            # Sala de Guerra + API (produccion: npm run server:prod)
npm run dev               # CLI en modo desarrollo
npx tempest               # CLI (tras npm run build)
```

Arsenal:

```bash
npm run arsenal:smoke     # prueba rapida del arsenal
npm run arsenal:doctor    # diagnostico de adaptadores de herramientas
```

Benchmarks de dominio (lectura de resultados ya comprometidos; `:live` corre en vivo):

```bash
npm run cve:bench         # caza de CVEs
npm run cloud:bench       # errores de configuracion en nube (IaC)
npm run mobile:bench      # analisis estatico movil
npm run binary:bench      # deteccion de sinks en binarios/RE
```

Actualizacion desde el repositorio upstream:

```bash
npm run update            # sincronizacion interactiva (pide y/N antes de cambiar nada)
npm run update:dry        # solo vista previa, no cambia nada
npm run update:hard       # reset duro a upstream/main (igual restaura rutas protegidas)
```

---

## 7. Variables de entorno utiles

| Variable | Para que sirve |
|---|---|
| `OPENROUTER_API_KEY` / `VENICE_API_KEY` / `ANTHROPIC_API_KEY` / `OPENAI_API_KEY` | Clave de proveedor si no usas agente local |
| `XAI_API_KEY` / `NOVITA_API_KEY` | Proveedores adicionales opcionales |
| `TEMPEST_LOCAL_BASE_URL` | URL del servidor local compatible con OpenAI/Ollama |
| `TEMPEST_LOCAL_MODEL` | Nombre del modelo local a usar |
| `T3MP3ST_FULL_ARSENAL` | Activa el arsenal completo (111 herramientas) |
| `T3MP3ST_LOCAL_AGENT_TIMEOUT_MS` | Tiempo maximo por llamada al agente local (ms) |
| `T3MP3ST_TASK_TIMEOUT_MS` | Tiempo maximo por tarea de mision (ms) |
| `T3MP3ST_GENERAL_TIMEOUT_MS` | Tiempo maximo para peticiones de planificacion (ms) |
| `T3MP3ST_TRUST_CLAUDE_SESSION` | Permite reutilizar la sesion de Claude Code (desactivado por defecto por seguridad) |

La reutilizacion de sesion de Claude Code esta desactivada por defecto porque una
sesion reanudada puede conservar contexto y autoridad de herramientas fuera del
limite de recibos del Arsenal. Solo actívala (`=1`) si confias explicitamente en
tu configuracion local.

---

## 8. Resultados (benchmarks) y como leerlos

Cada cifra se recalcula desde JSON comprometido con `npm run verify-claims`.

| Suite | Resultado | Contexto |
|---|---|---|
| **XBEN** (104 retos, black-box) | pass@1 medio **90.1%** | XBOW reporta 85% en la misma suite |
| **XBEN** (white-box, aparte) | pass@1 **98.7%** | nunca se mezcla con el numero black-box |
| **Cybench** (40 tareas, sin pistas) | **23/40 (58%)** pass@1 de una sola corrida | graduado contra un oraculo comprometido |
| **CVE-Zero** (10 CVEs reales post-cutoff, 7 lenguajes) | **8/10** archivo/linea/CWE exactos; **10/10** encontrados | a prueba de memorizacion: posteriores al corte del modelo |

Como interpretarlos:

- Cada bandera resuelta se gradua contra una verdad de referencia comprometida,
  no contra un auto-reporte.
- Black-box (sin fuente) y white-box (con fuente) se reportan por separado y
  nunca se mezclan.
- Las cifras las logro un **bucle de un solo agente**, no el enjambre de 8
  operadores. El enjambre es arquitectura, no lo que logro estos numeros.

El valor no es el numero, es el **recibo**: clona, corre `npm run verify-claims`
y cada veredicto se recalcula delante de ti desde su oraculo comprometido.

---

## 9. Flujo de trabajo tipico (de principio a fin)

1. **Instala** y arranca la Sala de Guerra (`npm install`, `npm run server`).
2. **Conecta** un agente local en Settings, o exporta una clave de proveedor.
3. **Define el objetivo autorizado**: confirma que tienes permiso por escrito y
   que esta dentro del alcance.
4. **Describe la mision** a Op Admiral en lenguaje natural y lanzala.
5. **Observa el reconocimiento**: el motor de Recon enumera activos y servicios
   con herramientas reales; la contencion de alcance bloquea lo que este fuera.
6. **Revisa hallazgos y evidencias** en la boveda y el libro de hallazgos.
7. **Para OSS**, usa el pipeline de divulgacion coordinada para generar un
   borrador; un humano lo envia al proveedor.
8. **Verifica** cualquier cifra o afirmacion con `npm run verify-claims`.

---

## 10. Donde seguir leyendo

La documentacion original (en ingles) esta en la carpeta `docs/`:

- `docs/GETTING_STARTED.md` — instalacion, primera mision segura, actualizaciones.
- `docs/DEVELOPER_GUIDE.md` — mapa del codigo fuente, scripts y puntos de extension.
- `docs/API_REFERENCE.md` — referencia de la API HTTP.
- `docs/MCP_GUIDE.md` — configuracion del servidor MCP y `security_recon`.
- `docs/SCOPE_AND_AUTHORIZATION.md` — modelo de autoridad, recibos de alcance,
  reglas de evidencia.
- `FEATURES.md` — estado funcion por funcion.

---

Recuerda: consigue permiso, mantente dentro del alcance y usa Armaggedon solo
contra objetivos que te pertenecen o para los que tienes autorizacion explicita.
