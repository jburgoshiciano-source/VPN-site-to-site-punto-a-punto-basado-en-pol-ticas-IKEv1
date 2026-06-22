# VPN-site-to-site-punto-a-punto-basado-en-politicas-IKEv1

#  VPN Site-to-Site IKEv1 Basada en Políticas

## 🎯 1. Objetivo de la VPN

El objetivo de este laboratorio es implementar una conexión segura punto a punto entre dos sitios (Peer_A y Peer_B) utilizando el protocolo **IPSec IKEv1**. La arquitectura es **basada en políticas**, lo que significa que el tráfico es cifrado únicamente cuando coincide con la Lista de Control de Acceso (ACL) que define las redes LAN origen y destino.

---

## 🗺️ 2. Topología, Interfaces y Direccionamiento IP

| Dispositivo | Interfaz | Dirección IP / Máscara | Gateway / Peer |
| :--- | :--- | :--- | :--- |
| **ISP** | Eth0/0 (Hacia A) | 20.24.1.1/30 | - |
| **ISP** | Eth0/1 (Hacia B) | 20.24.2.1/30 | - |
| **Peer_A (HQ)** | Eth0/0 (WAN) | 20.24.1.2/30 | 20.24.1.1 |
| **Peer_A (HQ)** | Eth0/1 (LAN) | 14.14.1.1/24 | - |
| **Peer_B (BR)** | Eth0/0 (WAN) | 20.24.2.2/30 | 20.24.2.1 |
| **Peer_B (BR)** | Eth0/1 (LAN) | 14.14.2.1/24 | - |
| **PC_A** | vpc | 14.14.1.10/24 | 14.14.1.1 |
| **PC_B** | vpc | 14.14.2.10/24 | 14.14.2.1 |

---

## Foto de la topología

<img width="575" height="486" alt="image" src="https://github.com/user-attachments/assets/da085365-31cf-4fea-ab54-fc71d9bc59dc" />

## ⚙️ 3. Parámetros Técnicos (Configuración de Criptografía)

### Fase 1 (ISAKMP)
* **Algoritmo de Cifrado:** AES-128.
* **Algoritmo de Hash:** SHA (SHA-1).
* **Grupo Diffie-Hellman:** Grupo 14 (2048-bit).
* **Autenticación:** Pre-Shared Key: `cisco123`.

### Fase 2 (IPSec)
* **Transform-set:** ESP-AES y ESP-SHA-HMAC.
* **Modo:** Tunnel Mode.
* **ACL de Política:** 100 (Permite tráfico entre 14.14.1.0/24 y 14.14.2.0/24).

---

## Prueba de Conectividad (Ping)

<img width="574" height="250" alt="image" src="https://github.com/user-attachments/assets/d461e655-232b-4772-8960-3615fa019e31" />

## Verificación de Túnel Activo

<img width="743" height="536" alt="image" src="https://github.com/user-attachments/assets/1512e023-80da-4652-95ca-c0b9e5405365" />

## 💻 4. Scripts de Configuración (Cisco CLI)

### Configuración Peer_A (Local)

```cisco
configure terminal
hostname Peer_A
! --- Interfaces ---
interface Ethernet0/0
 ip address 20.24.1.2 255.255.255.252
 no shutdown
interface Ethernet0/1
 ip address 14.14.1.1 255.255.255.0
 no shutdown
exit
! --- Enrutamiento hacia el ISP ---
ip route 0.0.0.0 0.0.0.0 20.24.1.1
! --- Configuración Fase 1 ---
crypto isakmp policy 10
 encr aes
 hash sha
 authentication pre-share
 group 14
crypto isakmp key cisco123 address 20.24.2.2
! --- Configuración Fase 2 ---
crypto ipsec transform-set TS esp-aes esp-sha-hmac 
 mode tunnel
exit
! --- ACL de Tráfico Interesante ---
access-list 100 permit ip 14.14.1.0 0.0.0.255 14.14.2.0 0.0.0.255
! --- Crypto Map ---
crypto map VPNMAP 10 ipsec-isakmp 
 set peer 20.24.2.2
 set transform-set TS 
 match address 100
exit
! --- Aplicación en Interfaz WAN ---
```

Avísame cuando tengas el resto de la configuración (el cierre de Peer_A, Peer_B completo e ISP) y te paso el bloque adicional para que lo agregues al mismo README.
