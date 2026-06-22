# VPN-site-to-site-punto-a-punto-basado-en-politicas-IKEv1

# 🔐 VPN Site-to-Site IPSec Policy-Based en Cisco IOS

![Topología VPN](cc596329-38e5-4755-acd3-0fef7de90d36.png)

## 👨‍🎓 Información del Estudiante

**Nombre:** Juan Francisco Burgos Hiciano  
**Matrícula:** 2023-1981  

---

## 📖 Introducción

Este laboratorio demuestra la implementación de una **VPN Site-to-Site IPSec basada en políticas (Policy-Based VPN)** utilizando routers Cisco IOS.

En este modelo, el tráfico que será protegido mediante IPSec se define a través de una **Lista de Control de Acceso (ACL)**. Posteriormente, dicha ACL es referenciada por un **Crypto Map**, el cual se aplica a la interfaz WAN del router para cifrar las comunicaciones entre las redes remotas.

Se presentan configuraciones utilizando:

- **IKEv1**: protocolo tradicional ampliamente soportado.
- **IKEv2**: versión más moderna, eficiente y segura.

---

## 🌐 Topología de Red

La práctica fue desarrollada en GNS3 utilizando tres routers:

- **Router A (Site A)**
- **Router ISP (Tránsito)**
- **Router B (Site B)**

El router ISP únicamente transporta el tráfico entre ambos sitios y no participa en la negociación de la VPN.

### Direccionamiento IP

| Dispositivo | Interfaz | Dirección IP |
|------------|-----------|-------------|
| Router A | LAN | 192.168.1.1/24 |
| Router A | WAN | 200.0.0.1/30 |
| Router ISP | Hacia Router A | 200.0.0.2/30 |
| Router ISP | Hacia Router B | 200.0.0.6/30 |
| Router B | WAN | 200.0.0.5/30 |
| Router B | LAN | 192.168.2.1/24 |

---

## 🔑 Características de la VPN Policy-Based

- Utiliza ACL para identificar el tráfico que será cifrado.
- Implementación sencilla y ampliamente utilizada.
- Compatible con IKEv1 e IKEv2.
- Permite proteger la comunicación entre redes remotas de forma segura.
- Utiliza IPSec para garantizar confidencialidad, integridad y autenticación.

---

## 🛠️ Requisitos

Se recomienda utilizar imágenes Cisco IOS con soporte criptográfico (**K9**), por ejemplo:

```text
c3725-adventerprisek9-mz.124-15.T14.bin
c2691-entservicesk9-mz.124-13b.bin
### 3.1 Feature Set requerido

Para que los comandos `crypto` funcionen, el IOS debe contener **K9** en el nombre de la imagen, indicando el feature set de criptografía:
