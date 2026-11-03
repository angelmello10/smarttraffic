# smarttraffic


# Sistema Escolar a Escala: Semáforos Inteligentes con IoT, IA y Gestión Web

Prototipo a escala de un sistema de control de tráfico urbano inteligente. Monitorea en tiempo real la densidad vehicular y el cruce peatonal en una intersección mediante **visión por computadora (IA)**, calculando dinámicamente el tiempo de luz verde para optimizar el flujo vial y reducir la congestión. Los datos se envían a una **plataforma web centralizada** para monitoreo, métricas históricas y control de emergencia.

---

## Descripción del Proyecto

- **Tipo de aplicación:** Sistema híbrido IoT + Inteligencia Artificial + Aplicación Web
- **Formato:** Prototipo físico a escala (maqueta) con control electrónico real
- **Arquitectura:** 4 capas → Captura/IoT → IA → Backend/BD → Dashboard Web

### Funcionalidades principales
- Detección y conteo de vehículos y peatones en tiempo real (visión por computadora)
- Cálculo dinámico del tiempo de luz verde según el conteo detectado
- Registro histórico de tráfico e incidencias en base de datos
- Dashboard web para monitoreo en vivo del estado de la intersección
- Control manual / modo de prioridad-emergencia desde el panel web
- Simulación de demanda peatonal mediante pulsadores físicos

### Actores del sistema
- **Peatón/Vehículo (simulado):** genera la demanda detectada por la cámara e IA
- **Sistema de IA (YOLOv8 Nano):** detecta y cuenta objetos, envía datos al backend
- **Microcontrolador (ESP32/Arduino):** ejecuta las órdenes de encendido/apagado de LEDs
- **Backend/API:** procesa datos, aplica lógica de semaforización, almacena en BD
- **Operador/Administrador:** usa el dashboard para monitorear y tomar control manual

---

## Herramientas de Desarrollo

| Categoría | Herramientas |
|---|---|
| Hardware | ESP32 DevKit V1 / Arduino UNO, módulo cámara (ESP32-CAM o webcam USB), LEDs, resistencias, pulsadores |
| IA / Visión por computadora | YOLOv8 Nano, Python |
| Comunicación | Protocolo MQTT o HTTP/WebSockets |
| Backend | API REST, Base de datos |
| Frontend | Dashboard web interactivo |

---

## Estructura del Repositorio


Cada integrante trabaja en su propia rama y se integra al repositorio general por sprint.

---

## Organización por Sprints

### Sprint 1 — Diseño & Prototipado IoT

**Objetivo:** Armado de la maqueta a escala y control físico del circuito de semáforos.

**Actividades:**
- Armado de la maqueta a escala
- Conexión del circuito de semáforos con ESP32/Arduino
- Programación básica del ciclo fijo de luces

**Historia de Usuario cubierta:**

> **HU03 — Solicitud de Cruce Peatonal mediante Botón Físico**
> Como *Peatón en la maqueta*, quiero presionar el pulsador físico de cruce, para solicitar que el semáforo cambie a luz roja para los vehículos y me otorgue paso seguro.
> **Criterios de aceptación:** Dado que el semáforo vehicular está en verde y el pulsador no ha sido activado, cuando el peatón lo presione, entonces el ESP32/Arduino debe registrar el evento, notificar al backend y programar el cambio a luz roja vehicular, encendiendo el indicador de paso peatonal.

**Entregable:** Circuito funcional controlado por microcontrolador.

---

### Sprint 2 — Modelo de IA & Visión + Backend & Lógica Dinámica

**Objetivo:** Detección de vehículos mediante visión por computadora y cálculo dinámico de tiempos de semáforo.

**Actividades:**
- Configuración de YOLOv8 Nano para detectar carritos a escala
- Calibración de la Zona de Interés (ROI) y conteo
- Creación de la base de datos (tablas de tráfico, estados, alertas)
- Algoritmo que calcula tiempos de verde según conteo de IA

**Historias de Usuario cubiertas:**

> **HU01 — Detección y Conteo Vehicular en Tiempo Real (IA)**
> Como *Sistema de IA*, quiero procesar la señal de video de la cámara para contar los vehículos en la ROI, para determinar la densidad vehicular en tiempo real.
> **Criterios de aceptación:** Dado que la cámara transmite video en vivo, cuando un vehículo ingrese a la ROI, entonces el sistema debe detectarlo, incrementar el conteo y enviarlo al backend.

> **HU02 — Ajuste Dinámico del Tiempo de Luz Verde**
> Como *Sistema de Semaforización Inteligente*, quiero calcular automáticamente la duración del ciclo de verde según el conteo recibido, para optimizar el flujo vial.
> **Criterios de aceptación:** Dado que el backend recibe un conteo de alta densidad, cuando se calcule el siguiente ciclo, entonces el sistema debe extender el tiempo de verde y transmitir la orden al ESP32/Arduino.

> **HU06 — Registro Histórico de Tráfico e Incidencias**
> Como *Administrador del Sistema*, quiero que todos los conteos, solicitudes peatonales y cambios de modo se almacenen en la base de datos, para consultar reportes históricos.
> **Criterios de aceptación:** Dado que ocurre un evento en el sistema, cuando la API REST lo procese, entonces debe guardarse un registro en las tablas correspondientes (RegistrosTrafico, EventosPeatones, etc.).

**Entregable:** Script en Python con detección/conteo en tiempo real + API funcional con lógica de adaptación dinámica.

---

### Sprint 3 — Aplicación Web Dashboard + Integración & Pruebas

**Objetivo:** Visualización en tiempo real, control manual del sistema e integración completa del prototipo.

**Actividades:**
- Diseño de la interfaz web para monitoreo en vivo
- Visualización de métricas de tráfico y control manual
- Integración completa IoT-IA-Web
- Pruebas de congestión, cruce peatonal y ajustes de latencia

**Historias de Usuario cubiertas:**

> **HU04 — Monitoreo en Tiempo Real desde el Dashboard Web**
> Como *Administrador del Sistema Vial*, quiero visualizar un dashboard interactivo con el estado de la intersección y métricas en vivo, para supervisar el sistema.
> **Criterios de aceptación:** Dado que el usuario está en el panel web, cuando el microcontrolador o la IA actualicen el estado, entonces la interfaz debe reflejarlo de inmediato sin recargar la página.

> **HU05 — Control Manual de Emergencia / Prioridad**
> Como *Operador de Tráfico*, quiero cambiar manualmente el semáforo a modo emergencia o prioridad desde el dashboard, para dar paso preferencial en situaciones extraordinarias.
> **Criterios de aceptación:** Dado que el semáforo opera en modo automático, cuando el operador seleccione "Modo Emergencia/Manual" y elija la vía prioritaria, entonces el sistema debe pausar el algoritmo automático, enviar la orden al ESP32 y registrar la incidencia en la base de datos.

**Entregable:** Panel Web conectado al backend + Prototipo integral funcional a escala.

---

## Notas Técnicas

Para mantener los costos bajos y evitar hardware de IA dedicado (como una Jetson Nano), se recomienda que la cámara transmita el video a una computadora personal donde se ejecute el modelo de IA en Python, comunicándose con el microcontrolador mediante **MQTT** o **HTTP/WebSockets**.

---

## Ramas del Repositorio

Cada integrante del equipo cuenta con su propia rama de trabajo, integrándose posteriormente a la rama principal por sprint.
