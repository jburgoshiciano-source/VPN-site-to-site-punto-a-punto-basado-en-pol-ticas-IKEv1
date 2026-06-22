# VPN-site-to-site-punto-a-punto-basado-en-politicas-IKEv1


## 📌 Información General

**Estudiante:** Juan Francisco Burgos Hiciano  
**Matrícula:** 2023-1981  
**Asignatura:** Seguridad en Redes  

---

## 📖 Descripción

Este laboratorio demuestra la implementación de una **VPN Site-to-Site IPSec basada en políticas (Policy-Based VPN)** utilizando routers Cisco IOS en GNS3.

La VPN basada en políticas utiliza listas de control de acceso (ACL) para identificar el tráfico que será protegido mediante IPSec. Cuando un paquete coincide con la ACL configurada, el tráfico es cifrado y enviado de forma segura hacia el sitio remoto.

Se implementaron las siguientes versiones del protocolo de intercambio de claves:

- **IKEv1**: método tradicional ampliamente utilizado en entornos empresariales.
- **IKEv2**: versión moderna que ofrece mejor rendimiento, mayor seguridad y una negociación más eficiente.

---
## Video explicativo

**Link del video**: https://youtu.be/wu8sxwnN3nI
---

## 🎯 Objetivos

- Configurar una VPN Site-to-Site IPSec entre dos redes remotas.
- Implementar IPSec utilizando IKEv1.
- Implementar IPSec utilizando IKEv2.
- Verificar la conectividad segura entre las LAN remotas.
- Analizar las diferencias entre IKEv1 e IKEv2.

---

## 🖼️ Topología

![Topología VPN](https://github.com/jburgoshiciano-source/VPN-site-to-site-punto-a-punto-basado-en-pol-ticas-IKEv1/blob/d23e0d65d52c49780f8bd311b8fe0ae018c5a713/vpn.png)

---

## 🌐 Direccionamiento IP

### Router A (Peer 1)

| Interfaz | Dirección IP | Descripción |
|-----------|-------------|-------------|
| e0/0 | 200.0.0.1/30 | WAN hacia ISP |
| e0/1 | 192.168.1.1/24 | LAN Site A |

### Router ISP

| Interfaz | Dirección IP | Descripción |
|-----------|-------------|-------------|
| e0/0 | 200.0.0.2/30 | Hacia Router A |
| e0/1 | 200.0.0.6/30 | Hacia Router B |

### Router B (Peer 2)

| Interfaz | Dirección IP | Descripción |
|-----------|-------------|-------------|
| e0/1 | 200.0.0.5/30 | WAN hacia ISP |
| e0/2 | 192.168.2.1/24 | LAN Site B |

### Hosts

| Equipo | Dirección IP | Gateway |
|----------|-------------|----------|
| PC-A | 192.168.1.10/24 | 192.168.1.1 |
| PC-B | 192.168.2.10/24 | 192.168.2.1 |

---

## 🔒 VPN Policy-Based

En una VPN basada en políticas, el tráfico que será cifrado se define mediante una ACL. Esta ACL es referenciada por un Crypto Map que posteriormente se aplica a la interfaz WAN.

### Componentes Utilizados

- ISAKMP Policy (IKEv1)
- IKEv2 Proposal
- IKEv2 Policy
- IKEv2 Keyring
- IKEv2 Profile
- Transform Set
- Crypto ACL
- Crypto Map
- Pre-Shared Key (PSK)

### Algoritmos Utilizados

| Parámetro | Valor |
|------------|--------|
| Cifrado | AES-256 |
| Integridad | SHA-256 |
| Grupo DH | 14 |
| Autenticación | Pre-Shared Key |
| PFS | Group 14 |

---

### Verificación

```bash
show version | include K9
show crypto ?
```

---

## 🔑 Configuración IKEv1

La implementación con IKEv1 se realiza en dos fases:

### Fase 1

- Definición de ISAKMP Policy.
- Configuración de algoritmos de cifrado.
- Autenticación mediante Pre-Shared Key.
- Intercambio seguro de claves.

### Fase 2

- Configuración del Transform Set.
- Definición de la ACL de cifrado.
- Creación del Crypto Map.
- Aplicación del Crypto Map a la interfaz WAN.

---

## 🔑 Configuración IKEv2

IKEv2 simplifica el proceso de negociación utilizando:

- Proposal
- Policy
- Keyring
- Profile

Posteriormente se configura IPSec mediante:

- Transform Set
- ACL de cifrado
- Crypto Map

---

## 📊 Comparación entre IKEv1 e IKEv2

| Característica | IKEv1 | IKEv2 |
|---------------|--------|--------|
| Mensajes de negociación | 9 | 4 |
| NAT Traversal | Manual | Automático |
| Seguridad | Buena | Mejorada |
| Rendimiento | Menor | Superior |
| Compatibilidad | Amplia | Moderna |
| Recomendación | Legacy | Nuevos despliegues |

---

## ✅ Verificación

Comandos utilizados para validar la VPN:

```bash
show crypto isakmp sa
show crypto ipsec sa
```

Para IKEv2:

```bash
show crypto ikev2 sa
```

Pruebas de conectividad:

```bash
ping 192.168.2.10
ping 192.168.1.10
```

---

## 🏆 Resultado

Se logró establecer exitosamente una VPN Site-to-Site IPSec entre las redes:

- **192.168.1.0/24**
- **192.168.2.0/24**

permitiendo una comunicación segura a través de una red pública mediante cifrado AES-256 e integridad SHA-256.

---

## 📚 Tecnologías Utilizadas

- Cisco IOS
- IPSec
- IKEv1
- IKEv2
- GNS3
- VPCS

---

