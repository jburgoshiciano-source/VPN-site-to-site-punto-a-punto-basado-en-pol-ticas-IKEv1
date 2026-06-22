# VPN-site-to-site-punto-a-punto-basado-en-politicas-IKEv1

**Autor:** Juan Francisco Burgos Hiciano
**Matrícula:** 2023-1981

---

## 📖 1. Introducción

Este documento describe la configuración completa de VPN Site-to-Site IPSec en routers Cisco IOS, cubriendo los tres modelos de implementación más utilizados en entornos empresariales:

* **Policy-Based VPN:** el tráfico a cifrar se define mediante listas de control de acceso (ACL).
* **Route-Based VPN (VTI):** el cifrado se aplica a través de una interfaz de túnel virtual.
* **GRE over IPSec:** GRE encapsula el tráfico (incluyendo multicast) e IPSec lo cifra.

Cada tipo se configura con ambas versiones del protocolo IKE:

* **IKEv1:** protocolo más antiguo, ampliamente soportado. Usa 9 mensajes en Main Mode.
* **IKEv2:** RFC 7296. Más eficiente (4 mensajes), NAT-T automático, mayor seguridad.

---

## 🗺️ 2. Topología de Red

La topología base es común para los tres tipos de VPN:

| Dispositivo | Interfaz | Dirección IP | Descripción |
| :--- | :--- | :--- | :--- |
| **Router A (Peer 1)** | eth0 — LAN | 192.168.1.1/24 | Gateway LAN Site A |
| **Router A (Peer 1)** | eth1 — WAN | 200.0.0.1/30 | Enlace hacia ISP |
| **Router ISP** | eth0 | 200.0.0.2/30 | Hacia Router A |
| **Router ISP** | eth1 | 200.0.0.6/30 | Hacia Router B |
| **Router B (Peer 2)** | eth1 — WAN | 200.0.0.5/30 | Enlace hacia ISP |
| **Router B (Peer 2)** | eth0 — LAN | 192.168.2.1/24 | Gateway LAN Site B |
| **Túnel GRE/VTI** | Tunnel0 | 10.0.0.1/30 — 10.0.0.2/30 | IPs internas de túnel |

> El Router ISP actúa como tránsito. El tráfico IPSec (UDP 500, protocolo ESP 50) pasa de forma transparente — el ISP no participa en la negociación VPN.

---

## ⚠️ 3. Requisitos de IOS y Solución de Errores Comunes

### 3.1 Feature Set requerido

Para que los comandos `crypto` funcionen, el IOS debe contener **K9** en el nombre de la imagen, indicando el feature set de criptografía:
