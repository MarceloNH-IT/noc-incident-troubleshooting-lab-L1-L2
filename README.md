# noc-incident-troubleshooting-lab-L1-L2
Laboratorio de redes y gestión de incidentes NOC, documentando flujos de escalamiento, análisis de causa raíz (RCA) y diagnósticos CLI.

# 🛠️ NOC Operations & Troubleshooting Lab: Enterprise Incident Management (L1/L2)

[![Network: Cisco IOS](https://img.shields.io/badge/Network-Cisco_IOS-blue.svg)](https://www.cisco.com)
[![ITSM: ServiceNow](https://img.shields.io/badge/ITSM-ServiceNow-green.svg)](https://www.servicenow.com)
[![Status: Lab Completed](https://img.shields.io/badge/Status-Completed-success.svg)]()

---

## 📄 1. Descripción General del Proyecto
Este repositorio documenta un **Procedimiento Operativo Estándar (SOP)** y la resolución técnica simulada de un incidente crítico de red (pérdida total de conectividad en una sucursal corporativa). El ejercicio simula con rigor el flujo operativo diario en un centro de operaciones de redes (**NOC - Network Operations Center**), integrando la gestión de servicios de TI bajo marcos **ITIL**, el diagnóstico escalonado de infraestructura (L1 a L2) y la ejecución de comandos de diagnóstico avanzado en equipos Cisco.

---

## 🏗️ 2. Topología y Componentes de Red
El laboratorio simula una red corporativa segmentada estructurada de la siguiente manera:
* **Capa de Acceso:** Switches Cisco Catalyst (ej. SW-ACC-01) conectados a los puestos de trabajo del Sector A (VLAN 10 - Data).
* **Capa de Distribución / Core:** Switch Cisco Core (SW-CORE-01) encargado del enrutamiento inter-VLAN y la puerta de enlace predeterminada.
* **Capa de Borde (Edge):** Router de salida (RTR-ISP-01) conectado al proveedor de servicios de internet (ISP) mediante enlaces dedicados con encapsulamiento estático/dinámico.
* **Servicios de Red:** Servidor DHCP corporativo y servidores DNS redundantes.

---

## 📋 3. Gestión ITSM y Modelo de Ticketing (ServiceNow)
Todo el ciclo de vida del evento se administra bajo estrictos estándares de la plataforma **ServiceNow (SNOW)**:
* **Canal de Entrada:** Alerta automática de monitoreo (SolarWinds/Zabbix) combinada con reporte telefónico de usuario.
* **Priorización de Incidente:** 
  * *Impacto:* Alto (Afectación a un sector completo de usuarios).
  * *Urgencia:* Alta.
  * *Prioridad Resultante:* P2 (SLA de atención inicial: 15 minutos | SLA de resolución: 4 horas).
* **Matriz de Escalamiento Operativo:** 
  * `NOC L1` (Triage, verificación física y comandos básicos de host) ➡️ `NOC L2` (Diagnóstico de equipos activos, análisis de CLI y mitigación) ➡️ `NOC L3 / Vendor` (Soporte avanzado, hardware RMA o ingeniería de diseño).

---

## 🚨 4. Bitácora de Resolución de Incidente: INC-0085421

### ⏱️ 09:00 AM | Fase 1: Recepción, Triage y Capa 1 (Rol: NOC L1)
El sistema de monitoreo dispara una alarma de caída de enlace en el Sector A. Varios usuarios reportan imposibilidad de trabajar.
* **Acción ITSM:** El Operador L1 abre el ticket `INC-0085421`, asigna la categoría `Network / LAN-WAN` y activa el cronómetro de SLA.
* **Verificación Física (Capa 1):** Se descartan cortes físicos masivos. Las interfaces de acceso se encuentran arriba (*Link UP*).
* **Work note en ServiceNow:** 
  > *"09:02 AM - [NOC L1] Apertura de INC-0085421 por pérdida de conectividad en Sector A. Enlaces físicos validados. Iniciando validación de parámetros IP en terminales."*

---

### ⏱️ 09:05 AM | Fase 2: Diagnóstico de Red Local - Capas 2 y 3 (Rol: NOC L1)
Se selecciona un equipo testigo para verificar la correcta integración con la LAN.
* **Comandos Ejecutados en Terminal:**
  ```cmd
  ipconfig /all
  ping 10.10.10.1
  arp -a
