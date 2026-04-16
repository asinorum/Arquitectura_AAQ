# Arquitectura de Solución de Seguridad

Este repositorio documenta la arquitectura de una solución de seguridad física y monitoreo remoto orientada a sitios industriales, integrando captura en campo, transporte de datos, núcleo de gestión y operación centralizada.

La solución contempla:

- Sensores y dispositivos de campo.
- Infraestructura de conectividad y energía.
- Núcleo de gestión sobre AAQ Core.
- Monitoreo y operación centralizada.
- Casos de despliegue por tipo de sitio.

---

## Contenido

- [Arquitectura general](#arquitectura-general)
- [Sitios específicos](#sitios-específicos)
  - [Planta de Procesos](#sitio-planta-de-procesos)
  - [Presa de Relave](#sitio-presa-de-relave)
  - [Estación Cambio de Diámetro](#sitio-estación-cambio-de-diámetro)
  - [Estación Estándar](#sitio-estación-estándar-titire-vizcachas-pellute-etc)
- [Convención visual](#convención-visual)
- [Uso en presentaciones](#uso-en-presentaciones)

---

## Arquitectura general

```mermaid
---
config:
  theme: base
  themeVariables:
    background: "#FFFFFF"
    primaryColor: "#EFF6FF"
    primaryTextColor: "#1E3A5F"
    primaryBorderColor: "#93C5FD"
    lineColor: "#3B82F6"
    secondaryColor: "#F0FDF4"
    tertiaryColor: "#F8FAFC"
    clusterBkg: "#F0F7FF"
    clusterBorder: "#3B82F6"
    fontFamily: "Segoe UI, Arial, sans-serif"
    edgeLabelBackground: "#FFFFFF"
    titleColor: "#1E3A5F"
  flowchart:
    curve: basis
    nodeSpacing: 50
    rankSpacing: 80
---
flowchart LR

    subgraph L1["NIVEL 1 — CAPTURA EN SITIOS REMOTOS"]
        direction TB
        A1["Cámaras Térmicas<br/>y Bi-espectro"]
        A2["Radares de<br/>Intrusión"]
        A3["Sensores de Fibra<br/>LAF / Vibración"]
        A4["Control de Acceso<br/>Biométrico"]
    end

    subgraph L2["NIVEL 2 — CONECTIVIDAD Y TRANSPORTE"]
        direction TB
        B1["Switch de Acceso PoE"]
        B2["Sistema Fotovoltaico<br/>/ UPS"]
        B3["Radioenlaces de Datos"]
    end

    subgraph L3["NIVEL 3 — NÚCLEO DE GESTIÓN AAQ"]
        direction TB
        C1["Red Troncal AAQ"]
        C2["Servidor VMS<br/>Avigilon"]
        C3[("Almacenamiento<br/>Central — TI")]
        C4["Estaciones de<br/>Monitoreo — Seguridad"]
    end

    A1 & A2 & A3 & A4 --> B1
    B2 -. energía de respaldo .-> B1
    B1 --> B3
    B3 ==> C1
    C1 --> C2
    C2 --> C3
    C2 --> C4

    classDef sensor  fill:#DBEAFE,stroke:#2563EB,color:#1E3A5F,stroke-width:1.5px;
    classDef network fill:#E0E7FF,stroke:#4F46E5,color:#1E1B4B,stroke-width:1.5px;
    classDef power   fill:#FEF9C3,stroke:#CA8A04,color:#713F12,stroke-width:1.5px;
    classDef core    fill:#EDE9FE,stroke:#7C3AED,color:#3B0764,stroke-width:2px;
    classDef storage fill:#F1F5F9,stroke:#64748B,color:#1E293B,stroke-width:1.5px;
    classDef ops     fill:#F0FDF4,stroke:#16A34A,color:#14532D,stroke-width:2px;

    class A1,A2,A3,A4 sensor;
    class B1,B3,C1 network;
    class B2 power;
    class C2 core;
    class C3 storage;
    class C4 ops;
```

---

## Sitios específicos

### Sitio: Planta de Procesos

**Caso de uso:** alta densidad de video para vigilancia operacional y seguridad perimetral.

```mermaid
---
config:
  theme: base
  themeVariables:
    background: "#FFFFFF"
    primaryColor: "#EFF6FF"
    primaryTextColor: "#1E3A5F"
    primaryBorderColor: "#93C5FD"
    lineColor: "#3B82F6"
    secondaryColor: "#F0FDF4"
    tertiaryColor: "#F8FAFC"
    clusterBkg: "#F0F7FF"
    clusterBorder: "#3B82F6"
    fontFamily: "Segoe UI, Arial, sans-serif"
    edgeLabelBackground: "#FFFFFF"
    titleColor: "#1E3A5F"
  flowchart:
    curve: basis
    nodeSpacing: 50
    rankSpacing: 80
---
flowchart LR

    subgraph PLANTA["PLANTA DE PROCESOS"]
        direction TB
        C1["08 PTZ<br/>Bi-espectro"]
        C2["Cámaras Bullet<br/>AcuSense"]
        S1["Switch Industrial"]
        R1["Enlace de<br/>Fibra / Radio"]

        C1 & C2 --> S1
        S1 --> R1
    end

    R1 ==> CORE["VMS Avigilon<br/>Core AAQ"]

    classDef sensor  fill:#DBEAFE,stroke:#2563EB,color:#1E3A5F,stroke-width:1.5px;
    classDef network fill:#E0E7FF,stroke:#4F46E5,color:#1E1B4B,stroke-width:1.5px;
    classDef core    fill:#EDE9FE,stroke:#7C3AED,color:#3B0764,stroke-width:2px;

    class C1,C2 sensor;
    class S1,R1 network;
    class CORE core;
```

---

### Sitio: Presa de Relave

**Caso de uso:** vigilancia de largo alcance con soporte energético autónomo.

```mermaid
---
config:
  theme: base
  themeVariables:
    background: "#FFFFFF"
    primaryColor: "#EFF6FF"
    primaryTextColor: "#1E3A5F"
    primaryBorderColor: "#93C5FD"
    lineColor: "#3B82F6"
    secondaryColor: "#F0FDF4"
    tertiaryColor: "#F8FAFC"
    clusterBkg: "#F0F7FF"
    clusterBorder: "#3B82F6"
    fontFamily: "Segoe UI, Arial, sans-serif"
    edgeLabelBackground: "#FFFFFF"
    titleColor: "#1E3A5F"
  flowchart:
    curve: basis
    nodeSpacing: 50
    rankSpacing: 80
---
flowchart LR

    subgraph PRESA["PRESA DE RELAVE"]
        direction TB
        P1["Sistema Posicionamiento<br/>Térmico DS-2TD6267"]
        SOL["Sistema<br/>Fotovoltaico"]
        SW["Switch"]
        RAD["Radioenlace<br/>de Salida"]

        P1 --> SW
        SOL -. energía DC .-> SW
        SW --> RAD
    end

    RAD ==> CORE["VMS Avigilon<br/>Core AAQ"]

    classDef sensor  fill:#DBEAFE,stroke:#2563EB,color:#1E3A5F,stroke-width:1.5px;
    classDef network fill:#E0E7FF,stroke:#4F46E5,color:#1E1B4B,stroke-width:1.5px;
    classDef power   fill:#FEF9C3,stroke:#CA8A04,color:#713F12,stroke-width:1.5px;
    classDef core    fill:#EDE9FE,stroke:#7C3AED,color:#3B0764,stroke-width:2px;

    class P1 sensor;
    class SW,RAD network;
    class SOL power;
    class CORE core;
```

---

### Sitio: Estación Cambio de Diámetro

**Caso de uso:** monitoreo de fibra LAF y correlación con alarmas de campo.

```mermaid
---
config:
  theme: base
  themeVariables:
    background: "#FFFFFF"
    primaryColor: "#EFF6FF"
    primaryTextColor: "#1E3A5F"
    primaryBorderColor: "#93C5FD"
    lineColor: "#3B82F6"
    secondaryColor: "#F0FDF4"
    tertiaryColor: "#F8FAFC"
    clusterBkg: "#F0F7FF"
    clusterBorder: "#3B82F6"
    fontFamily: "Segoe UI, Arial, sans-serif"
    edgeLabelBackground: "#FFFFFF"
    titleColor: "#1E3A5F"
  flowchart:
    curve: basis
    nodeSpacing: 50
    rankSpacing: 80
---
flowchart LR

    subgraph ECD["ESTACIÓN CAMBIO DE DIÁMETRO"]
        direction TB
        F2["Fibra Óptica<br/>Instalada"]
        F1["Sensor de<br/>Monitoreo LAF"]
        A1["Panel de<br/>Alarma"]
        SW["Switch"]

        F2 --- F1
        F1 --> SW
        A1 --> SW
    end

    SW ==> CORE["Red Troncal AAQ"]

    classDef sensor  fill:#DBEAFE,stroke:#2563EB,color:#1E3A5F,stroke-width:1.5px;
    classDef network fill:#E0E7FF,stroke:#4F46E5,color:#1E1B4B,stroke-width:1.5px;
    classDef ops     fill:#F0FDF4,stroke:#16A34A,color:#14532D,stroke-width:2px;
    classDef core    fill:#EDE9FE,stroke:#7C3AED,color:#3B0764,stroke-width:2px;

    class F1,F2 sensor;
    class SW network;
    class A1 ops;
    class CORE core;
```

---

### Sitio: Estación Estándar (Titire, Vizcachas, Pellute, etc.)

**Caso de uso:** vigilancia perimetral estándar con video, radar, control de acceso y sensor de cable perimetral.

```mermaid
---
config:
  theme: base
  themeVariables:
    background: "#FFFFFF"
    primaryColor: "#EFF6FF"
    primaryTextColor: "#1E3A5F"
    primaryBorderColor: "#93C5FD"
    lineColor: "#3B82F6"
    secondaryColor: "#F0FDF4"
    tertiaryColor: "#F8FAFC"
    clusterBkg: "#F0F7FF"
    clusterBorder: "#3B82F6"
    fontFamily: "Segoe UI, Arial, sans-serif"
    edgeLabelBackground: "#FFFFFF"
    titleColor: "#1E3A5F"
  flowchart:
    curve: basis
    nodeSpacing: 50
    rankSpacing: 80
---
flowchart LR

    subgraph EST["ESTACIÓN ESTÁNDAR — Titire, Vizcachas, Pellute, etc."]
        direction TB
        V1["Domo PTZ 42X"]
        V2["Cámara Bullet 2MP"]
        D1["Radar de Intrusión"]
        AC["Control Biométrico"]
        CS["Cable Sensor<br/>Perimetral"]
        S1["Switch"]

        V1 & V2 & D1 & AC & CS --> S1
    end

    S1 ==> RADIO["Radioenlace"]
    RADIO ==> CORE["Red Troncal AAQ"]

    classDef sensor  fill:#DBEAFE,stroke:#2563EB,color:#1E3A5F,stroke-width:1.5px;
    classDef network fill:#E0E7FF,stroke:#4F46E5,color:#1E1B4B,stroke-width:1.5px;
    classDef ops     fill:#F0FDF4,stroke:#16A34A,color:#14532D,stroke-width:2px;
    classDef core    fill:#EDE9FE,stroke:#7C3AED,color:#3B0764,stroke-width:2px;

    class V1,V2,D1,CS sensor;
    class S1,RADIO,CORE network;
    class AC ops;
```

---

## Convención visual

La documentación usa una convención de color unificada para facilitar lectura técnica y consistencia visual:

| Clase | Color base | Uso |
|---|---|---|
| `sensor` | Azul claro | Cámaras, sensores, fibra, cable perimetral |
| `network` | Índigo claro | Switches, enlaces, red troncal |
| `power` | Amarillo claro | Energía, solar, UPS |
| `core` | Violeta claro | VMS, Core AAQ |
| `storage` | Gris claro | Almacenamiento |
| `ops` | Verde claro | Operación, alarmas, control de acceso |

---

## Uso en presentaciones

Estos diagramas pueden usarse de tres maneras:

1. Directamente en GitHub como bloques `mermaid`.
2. Exportándolos a **SVG** o **PNG** para insertarlos en PowerPoint.
3. Manteniendo el código fuente en el repositorio y una versión gráfica para propuestas comerciales.

Para exportarlos como imagen, una ruta práctica es usar Mermaid Live Editor y descargar en SVG o PNG.

---

## Notas

- Si alguna variante visual no se renderiza igual en GitHub, conviene exportar el diagrama como SVG e insertarlo como imagen en la presentación.
- Si quieres mantener edición simple, deja el código Mermaid en el repositorio y usa el SVG solo para la presentación final.
