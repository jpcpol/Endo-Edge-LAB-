# DT-Master — Registro de Deuda Técnica
## Proyecto Endo-Edge LAB+
**Última auditoría:** 2026-04-02
**Auditor:** Claude Code (claude-sonnet-4-6)
**Versión:** 1.0

---

## Estado general del proyecto

| Dimensión | Estado | Detalle |
|---|---|---|
| Documentación técnica | ✅ Completa | PROMPT_MAESTRO, DOC-EE-SW-001, AMD01, DOC-EE-PIPE-001 |
| Documentación legal/regulatoria | ✅ Completa | NDA-B/C/D, Convenio Co-PI, PI-001 v1.3, Protocolo v2.1 |
| Arquitectura de sistema | ✅ Definida | 3 nodos, hardware, stacks tecnológicos especificados |
| Simulador Pipeline | ✅ Funcional | simulador_pipeline_fusion_temporal.html (19 KB) |
| Estructura de repositorio | ✅ Creada | endoedge-node1/ con árbol completo de directorios y stubs |
| **Implementación de código** | ❌ **Pendiente 100%** | **Todos los archivos fuente están vacíos (0 bytes)** |
| Tests | ❌ Sin implementar | Carpetas creadas, archivos vacíos |
| Entorno de build | ❌ Sin configurar | Cargo.toml y CMakeLists.txt vacíos |

---

## Resumen de auditoría — Lo que ESTÁ hecho

### Documentación (Completada)
- [x] **PROMPT_MAESTRO_EndoEdge_LAB+.txt** — Fuente de verdad del proyecto. Incluye visión, arquitectura de 3 nodos, parámetros numéricos críticos, inventario de documentos, tareas pendientes e instrucciones para el asistente.
- [x] **DOC-EE-SW-001 v1.0** — Arquitectura SW Nodo 1 (versión base MAX78000, congelada).
- [x] **DOC-EE-SW-001-AMD01 v1.0** — Enmienda MAX78000→MAX78002. Incluye tabla comparativa, código dual-core (cnn_worker.c + mailbox.h), modelo LSTM vs TCN, criterios de aceptación actualizados.
- [x] **DOC-EE-PIPE-001 v1.0** — Pipeline Fusión Temporal. Capa 1 (Edge), Capa 2 (Nodo 2 MFT), Capa 3 (NNC LSTM). Tabla de 19 parámetros numéricos. Recursos FHIR R4.
- [x] **Arquitectura_Onpremise_fhir_Endoedge.svg** — Diagrama visual arquitectura On-Premise FHIR.
- [x] **Protocolo EE-2026-002-FASE1B v2.1** — Protocolo científico-legal. N=50, 3 grupos cohorte, 18 meses.
- [x] **PI-001 v1.3** — Declaración DNDA de titularidad. Chancay (a)-(h)(k), Cicchitti 50% (i)(j).
- [x] **NDA-B** — Universidad/CONICET (anti-co-titularidad automática).
- [x] **NDA-C** — Medtech (tabla de cláusulas prohibidas).
- [x] **NDA-D + Convenio Cicchitti** — Red de laboratorios + convenio Co-PI (7 responsabilidades, 3 hitos).
- [x] **Cartas de invitación (6)** — Co-PI, Bioquímico, Cardiólogo, Nefrólogo, Hepatólogo, Oncólogo.
- [x] **Simulador HTML** — Simulador del pipeline de fusión temporal, funcional en navegador.

### Repositorio endoedge-node1/ (Estructura creada, código vacío)
- [x] Árbol de directorios completo: `firmware/`, `comms/`, `model/`, `tests/`, `tools/`
- [x] Stubs de archivos fuente creados (nomenclatura correcta, coherente con DOC-EE-SW-001)

---

## Deuda Técnica Activa

### Clasificación por criticidad

#### 🔴 CRÍTICO — Bloqueante para inicio de desarrollo

| ID | Ítem | Categoría | Archivo/Módulo | Referencia doc |
|---|---|---|---|---|
| DT-001 | Cargo.toml del crate `comms` (nRF52840) vacío — sin dependencias embassy-rs, rustls, etc. | Configuración | `endoedge-node1/comms/Cargo.toml` | DOC-EE-SW-001-AMD01 §Stack Rust |
| DT-002 | CMakeLists.txt del firmware vacío — sin targets, sin MSDK v1.3, sin flags MISRA | Configuración | `endoedge-node1/firmware/CMakeLists.txt` | DOC-EE-SW-001-AMD01 §Build |
| DT-003 | FreeRTOSConfig.h vacío — sin configuración de tareas, prioridades ni memoria | Firmware RTOS | `firmware/rtos/FreeRTOSConfig.h` | PROMPT_MAESTRO §Tasks FreeRTOS |
| DT-004 | main.c vacío — sin inicialización del sistema (MCU, periféricos, FreeRTOS) | Firmware | `firmware/main.c` | DOC-EE-SW-001 §N1-H1/H2 |
| DT-005 | model/requirements.txt vacío — sin dependencias Python (PyTorch, ONNX, etc.) | ML | `endoedge-node1/model/requirements.txt` | PROMPT_MAESTRO §Para VS Code |

#### 🟠 ALTA — Componentes core del sistema

| ID | Ítem | Categoría | Archivo/Módulo | Referencia doc |
|---|---|---|---|---|
| DT-006 | HAL ADC (DMA 256 Hz, ECG) no implementado | Firmware HAL | `firmware/hal/adc.c/.h` | DOC-EE-SW-001 §N1-H2 |
| DT-007 | HAL UART SLIP (MAX78002 → nRF52840, 921600 bps) no implementado | Firmware HAL | `firmware/hal/uart.c/.h` | DOC-EE-SW-001 §N1-H2 |
| DT-008 | HAL SPI, GPIO, Timer no implementados | Firmware HAL | `firmware/hal/spi.c/.h`, `gpio.c/.h`, `timer.c/.h` | DOC-EE-SW-001 §N1-H2 |
| DT-009 | FIR orden 51 (0.5-40 Hz @ 256 Hz, coeficientes Q15) no implementado | Señales | `firmware/signal/ecg_filter.c/.h` | DOC-EE-PIPE-001 §Capa 1 |
| DT-010 | Pan-Tompkins INT8 (sensib. ≥98%, especif. ≥97%, MIT-BIH) no implementado | Señales | `firmware/signal/ecg_filter.c/.h` | DOC-EE-PIPE-001 §Capa 1 |
| DT-011 | HRV RMSSD + LF/HF via Goertzel no implementado (delta < 2 ms vs Kubios) | Señales | `firmware/signal/hrv.c/.h` | DOC-EE-PIPE-001 §Capa 1 |
| DT-012 | N-Zscore individual (mu/sigma personal, clip [-3,3]) no implementado | Señales | `firmware/signal/zscore.c/.h` | DOC-EE-PIPE-001 §Capa 1 |
| DT-013 | Corrección pH-lactato (5 rangos, FC 0.62-0.68) no implementada | Señales | `firmware/signal/lactate_ph.c/.h` | DOC-EE-PIPE-001 §Capa 1 |
| DT-014 | Gate de iones (tasa sudoración ≥ 20 nL/min/glándula) no implementado | Señales | `firmware/signal/ion_gate.c/.h` | DOC-EE-PIPE-001 §Capa 1 |
| DT-015 | Contexto IMU (6 códigos 0x00–0xFF) no implementado | Señales | `firmware/signal/imu_context.c/.h` | DOC-EE-PIPE-001 §Capa 1 |
| DT-016 | 7 Tasks FreeRTOS no implementadas (ecg_capture, signal_process, result_handler, packet, sensors, baseline, watchdog) | Firmware RTOS | `firmware/rtos/tasks.c` | PROMPT_MAESTRO §Tasks FreeRTOS |
| DT-017 | DSS Engine Edge (DSS uint8 0-100, umbral alerta) no implementado | Inferencia | `firmware/inference/dss_engine.c/.h` | PROMPT_MAESTRO §Outputs |
| DT-018 | ONNX Runner INT8 (criterio: delta DSS < 2 pts, AUROC < 2 pp) no implementado | Inferencia | `firmware/inference/onnx_runner.c/.h` | DOC-EE-SW-001-AMD01 §Modelo Edge |
| DT-019 | Cuantización/dequantización INT8 para el modelo LSTM no implementada | Inferencia | `firmware/inference/quantize.c/.h` | DOC-EE-SW-001-AMD01 §AMD01 |
| DT-020 | model.h (pesos CNN como C array, generado por ai8xize.py) ausente | Inferencia | `firmware/inference/model.h` | DOC-EE-SW-001-AMD01 §Despliegue |
| DT-021 | cnn_worker.c (loop RISC-V + mailbox M4F↔RISC-V) ausente en repo | Inferencia | `firmware/inference/cnn_worker.c` | DOC-EE-SW-001-AMD01 §Dual-core |
| DT-022 | mailbox.h (CNN_InputMsg_t, CNN_OutputMsg_t) ausente en repo | Inferencia | `firmware/inference/mailbox.h` | DOC-EE-SW-001-AMD01 §Dual-core |
| DT-023 | EndoEdgePacket builder (MessagePack ~640 bytes, header + señales + DSS + DQI) no implementado | Paquete | `firmware/packet/builder.c/.h` | PROMPT_MAESTRO §Protocolo |
| DT-024 | Signer RSA-2048 (SHA-256 del payload, Ley 25.506) no implementado | Paquete | `firmware/packet/signer.c/.h` | PROMPT_MAESTRO §PKI |
| DT-025 | fhir_obs.c (serialización de observaciones como FHIR R4) no implementado | Paquete | `firmware/packet/fhir_obs.c/.h` | DOC-EE-PIPE-001 §FHIR |
| DT-026 | Gestor de línea base (storage NVRAM mu/sigma, actualización mensual) no implementado | Baseline | `firmware/baseline/storage.c/.h`, `updater.c/.h` | PROMPT_MAESTRO §Capa 1 |

#### 🟡 MEDIA — Stack de comunicaciones Rust (nRF52840)

| ID | Ítem | Categoría | Archivo/Módulo | Referencia doc |
|---|---|---|---|---|
| DT-027 | BLE 5.2 Central (recepción desde micro-nodos relé 0-5 cm) no implementado | Comms Rust | `comms/src/ble/central.rs` | PROMPT_MAESTRO §Stack Rust |
| DT-028 | GATT Client (protocolo para dispositivos satélite) no implementado | Comms Rust | `comms/src/ble/gatt_client.rs` | PROMPT_MAESTRO §Stack Rust |
| DT-029 | Relay nodes manager (nRF52811/nRF52805) no implementado | Comms Rust | `comms/src/ble/relay_nodes.rs` | PROMPT_MAESTRO §BAN |
| DT-030 | SLIP decoder (recepción de paquetes MAX78002 via UART 921600 bps) no implementado | Comms Rust | `comms/src/transport/slip.rs` | PROMPT_MAESTRO §Stack Rust |
| DT-031 | Cola offline (48h de paquetes en flash NVM nRF52840, backoff exp.) no implementada | Comms Rust | `comms/src/transport/queue.rs` | PROMPT_MAESTRO §Stack Rust |
| DT-032 | Scheduler de transmisión no implementado | Comms Rust | `comms/src/transport/scheduler.rs` | PROMPT_MAESTRO §Stack Rust |
| DT-033 | TLS 1.3 client (rustls + embedded-tls hacia Nodo 2) no implementado | Comms Rust | `comms/src/tls/client.rs` | PROMPT_MAESTRO §Stack Rust |
| DT-034 | Cert store (almacenamiento de device-cert + CA, rotación 12 meses) no implementado | Comms Rust | `comms/src/tls/cert_store.rs` | PROMPT_MAESTRO §PKI |
| DT-035 | main.rs del crate comms (entry point embassy-rs) vacío | Comms Rust | `comms/src/main.rs` | DOC-EE-SW-001-AMD01 |

#### 🟡 MEDIA — Modelo ML y pipeline Python

| ID | Ítem | Categoría | Archivo/Módulo | Referencia doc |
|---|---|---|---|---|
| DT-036 | Arquitectura LSTM bidireccional 2L×64 + atención temporal no implementada | ML | `model/train/architecture.py` | DOC-EE-SW-001-AMD01 §Modelo Edge |
| DT-037 | Pipeline de entrenamiento (PyTorch, input [B,12,7]) no implementado | ML | `model/train/train.py` | PROMPT_MAESTRO §Para VS Code |
| DT-038 | Dataset loader y preprocesamiento no implementados | ML | `model/train/dataset.py` | PROMPT_MAESTRO §Para VS Code |
| DT-039 | Script de evaluación (AUROC, delta DSS, Catálogo 10 patologías) no implementado | ML | `model/train/evaluate.py` | PROMPT_MAESTRO §Rendimiento |
| DT-040 | Simulador de pacientes sintéticos no implementado | ML Datos | `model/synthetic/patient_sim.py` | PROMPT_MAESTRO §Para VS Code |
| DT-041 | Generador de señales sintéticas (ECG, GSR, glucosa, etc.) no implementado | ML Datos | `model/synthetic/signal_gen.py` | PROMPT_MAESTRO §Para VS Code |
| DT-042 | Conversor PyTorch → ONNX INT8 no implementado | ML Conversión | `model/convert/to_onnx.py` | DOC-EE-SW-001-AMD01 §Conversión |
| DT-043 | Conversor → TFLite no implementado | ML Conversión | `model/convert/to_tflite.py` | DOC-EE-SW-001-AMD01 |
| DT-044 | Conversor → C array (ai8xize.py wrapper) no implementado | ML Conversión | `model/convert/to_c_array.py` | DOC-EE-SW-001-AMD01 §Despliegue |
| DT-045 | Validación de cuantización (delta DSS < 2 pts, AUROC < 2 pp) no implementada | ML Conversión | `model/convert/validate_quant.py` | DOC-EE-SW-001-AMD01 §Criterios |

#### 🟡 MEDIA — Tests

| ID | Ítem | Categoría | Archivo/Módulo | Referencia doc |
|---|---|---|---|---|
| DT-046 | Tests Unity para firmware (pan_tompkins_test.c, MIT-BIH dataset) ausentes | Testing | `tests/firmware/` | PROMPT_MAESTRO §Para VS Code |
| DT-047 | Tests Unity para HAL (adc, uart, spi) ausentes | Testing | `tests/firmware/` | DOC-EE-SW-001 §N1-H2 |
| DT-048 | Tests Unity para señales (FIR, HRV, N-Zscore) ausentes | Testing | `tests/firmware/` | DOC-EE-SW-001 §N1-H3 |
| DT-049 | Tests Unity para inferencia (delta DSS < 2 pts en hardware real) ausentes | Testing | `tests/firmware/` | DOC-EE-SW-001-AMD01 §Criterios |
| DT-050 | cargo test para stack Rust (BLE, TLS, queue) ausente | Testing | `tests/comms/` | DOC-EE-SW-001-AMD01 §Stack Rust |
| DT-051 | pytest para pipeline ML (architecture, dataset, conversión) ausente | Testing | `tests/model/` | PROMPT_MAESTRO §Para VS Code |

#### 🟡 MEDIA — Tools

| ID | Ítem | Categoría | Archivo/Módulo | Referencia doc |
|---|---|---|---|---|
| DT-052 | flash.sh (flashing MAX78002 via MSDK/OpenOCD) vacío | Tooling | `tools/flash.sh` | DOC-EE-SW-001 §N1-H1 |
| DT-053 | monitor.py (consola serial, parsing EndoEdgePacket) vacío | Tooling | `tools/monitor.py` | DOC-EE-SW-001 |
| DT-054 | baseline_tool.py (gestión línea base mu/sigma en NVRAM) vacío | Tooling | `tools/baseline_tool.py` | PROMPT_MAESTRO §Capa 1 |

#### 🔵 BAJA — Documentación pendiente (identificada en PROMPT_MAESTRO)

| ID | Ítem | Categoría | Estado |
|---|---|---|---|
| DT-055 | DOC-EE-SW-002: Arquitectura SW Nodo 2 (integración Delphi-2M + HAPI FHIR) | Documentación | No iniciado |
| DT-056 | DOC-EE-PROTO-001: Protocolo inter-nodos (comunicación Nodo1↔Nodo2) | Documentación | No iniciado |
| DT-057 | Carta de invitación para Bioestadístico | Documentación | No enviada |
| DT-058 | Consentimiento Informado v2.0 (con acceso historia clínica retrospectiva) | Legal/Regulatorio | No iniciado |
| DT-059 | Completar hashes SHA-256 del Anexo A del PI-001 v1.3 | Legal/PI | Pendiente (software no implementado) |
| DT-060 | Completar DNI/domicilio de Dra. Cicchitti en PI-001 v1.3 | Legal/PI | Pendiente datos |
| DT-061 | Consulta antecedentes INPI para patente DSS/ICM/RTM | Legal/PI | No iniciado |
| DT-062 | Plan estadístico formal con Bioestadístico | Científico | Pendiente equipo |
| DT-063 | Estrategia de financiamiento (CONICET, ANPCyT, fondos privados) | Gestión | No iniciado |

---

## Deuda Técnica Resuelta

| ID | Ítem | Fecha cierre | Evidencia |
|---|---|---|---|
| DT-R001 | Arquitectura de 3 nodos definida | 2026-03 | PROMPT_MAESTRO v1.0 |
| DT-R002 | Hardware seleccionado y justificado (MAX78002, nRF52840, sensores BAN) | 2026-03 | DOC-EE-SW-001-AMD01 |
| DT-R003 | Pipeline de fusión temporal especificado (tensor [B,288,12], 12 features) | 2026-03 | DOC-EE-PIPE-001 |
| DT-R004 | Marco legal-regulatorio definido (Ley 25.326, 26.529, ISO 14155, IEC 62304) | 2026-03 | Protocolo EE-2026-002 |
| DT-R005 | Modelo LSTM bidireccional seleccionado vs TCN (con justificación técnica) | 2026-03 | DOC-EE-SW-001-AMD01 |
| DT-R006 | Restricción de hormonas no medibles documentada y mitigación definida | 2026-03 | PROMPT_MAESTRO §Restricción Dura |
| DT-R007 | Red BAN con micro-nodos relé nRF52811/nRF52805 (solución bus digital) | 2026-03 | PROMPT_MAESTRO §BAN |
| DT-R008 | Protocolo EndoEdgePacket definido (MessagePack, RSA-2048, ~640 bytes) | 2026-03 | PROMPT_MAESTRO §Protocolo |
| DT-R009 | PKI definida (Root-CA offline, Device-CA online, rotación 12 meses) | 2026-03 | PROMPT_MAESTRO §PKI |
| DT-R010 | Cohorte N=50 diseñada (Grupos A/B/C, 18 meses, diseño híbrido) | 2026-03 | Protocolo EE-2026-002 |
| DT-R011 | Estructura de repositorio monorepo creada (stubs de archivos) | 2026-03 | endoedge-node1/ |
| DT-R012 | Simulador HTML del pipeline de fusión temporal | 2026-03 | simulador_pipeline_fusion_temporal.html |
| DT-R013 | Cronograma Nodo 1 definido (19 semanas, hitos N1-H1 a N1-H7) | 2026-03 | PROMPT_MAESTRO §Cronograma |
| DT-R014 | NDA-B, NDA-C, NDA-D firmados o en proceso | 2026-03 | Documentación/NDA/ |
| DT-R015 | Convenio Co-PI Dra. Cicchitti firmado | 2026-03 | NDA-D_y_Convenio_Cicchitti |
| DT-R016 | Cartas de invitación equipo científico (6 roles) | 2026-03 | Cartas_Invitacion.docx |
| DT-R017 | Catálogo de 10 patologías Fase I definido con criterios AUROC | 2026-03 | PROMPT_MAESTRO §Catálogo |
| DT-R018 | Panel LAB+ de 16 analitos con códigos LOINC definido | 2026-03 | PROMPT_MAESTRO §Panel LAB |

---

## Mapa de hitos de implementación (Cronograma N1)

Según DOC-EE-SW-001-AMD01 y PROMPT_MAESTRO:

| Hito | Duración | Deuda técnica asociada | Estado |
|---|---|---|---|
| N1-H1: Entorno MAX78002EVKIT + MSDK v1.3 | 1 sem | DT-001, DT-002, DT-052 | ❌ Pendiente |
| N1-H2: HAL + drivers (ADC DMA, UART SLIP, GPIO) | 2 sem | DT-006, DT-007, DT-008, DT-047 | ❌ Pendiente |
| N1-H3: Pipeline señales (FIR + Pan-Tompkins + HRV + N-Zscore) | 3 sem | DT-009–DT-016, DT-026, DT-046, DT-048 | ❌ Pendiente |
| N1-H4: Modelo LSTM + MAX78002 Model Converter | 4 sem | DT-036–DT-045, DT-020 | ❌ Pendiente |
| N1-H5: Inferencia dual-core (cnn_worker.c + mailbox M4F↔RISC-V) | 3 sem | DT-017–DT-022, DT-049 | ❌ Pendiente |
| N1-H6: Stack Rust completo (BLE + TLS + queue offline) | 4 sem | DT-027–DT-035, DT-050 | ❌ Pendiente |
| N1-H7: Integración completa + tests de aceptación | 2 sem | DT-023–DT-025, DT-046–DT-054 | ❌ Pendiente |

---

## Próximos pasos recomendados (orden de prioridad)

1. **[DT-001, DT-002]** Configurar entorno de build: `Cargo.toml` con dependencias embassy-rs y `CMakeLists.txt` con MSDK v1.3 → desbloquea N1-H1.
2. **[DT-005]** Completar `model/requirements.txt` → permite trabajo paralelo en el pipeline ML.
3. **[DT-003, DT-004]** Implementar `FreeRTOSConfig.h` y `main.c` → esqueleto del sistema.
4. **[DT-036, DT-040, DT-041]** Implementar generadores de datos sintéticos y arquitectura LSTM → permite entrenamiento en paralelo con el firmware.
5. **[DT-055]** Redactar DOC-EE-SW-002 (Nodo 2) → pendiente de documentación de alta prioridad.
6. **[DT-057]** Carta para Bioestadístico → necesario para plan estadístico de la Fase I.

---

*Este documento se mantiene como registro activo de deuda técnica. Actualizar al cerrar cada ítem con fecha y evidencia.*
