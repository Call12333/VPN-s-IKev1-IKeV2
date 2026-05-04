# 🔐 Site-to-Site VPN Lab (IKEv1 / IKEv2) – PNETLab

Un laboratorio práctico de redes donde se implementa una **VPN IPsec site-to-site** entre dos redes privadas a través de Internet, utilizando **IKEv1 e IKEv2** para negociación segura.

---

## 🧠 Objetivo

Establecer un túnel VPN funcional que permita la comunicación cifrada entre dos LAN separadas, simulando un entorno empresarial real.

---

## 🌐 Topología

* 2 Routers (Enterprise)
* 2 Switches
* Hosts en cada red (Pentesting, Threat Hunting, etc.)
* Conexión a través de una nube (Internet)

📌 El tráfico entre redes viaja **cifrado mediante IPsec**




---

## 🔐 Tecnologías usadas

* IPsec (Encapsulación segura)
* IKEv1 / IKEv2 (Intercambio de claves)
* AES / SHA (Cifrado e integridad)
* Pre-Shared Key (PSK)

---

## ⚙️ Configuración general

### 1. Fase 1 – IKE (ISAKMP)

Establece el canal seguro inicial:

```
encryption aes
hash sha
authentication pre-share
group 2
```

---

### 2. Fase 2 – IPsec

Define qué tráfico será cifrado:

```
access-list 100 permit ip [LAN_LOCAL] [WILDCARD] [LAN_REMOTA] [WILDCARD]

crypto ipsec transform-set MYSET esp-aes esp-sha-hmac
```

---

### 3. Crypto Map

Aplica la política VPN al router:

```
crypto map VPN 10 ipsec-isakmp
 set peer [IP_PUBLICA_REMOTA]
 set transform-set MYSET
 match address 100
```

```
interface g0/0
 crypto map VPN
```

---

## 🧪 Validación

* 🔁 Ping entre hosts de ambas redes
* 📊 Verificar estado del túnel:

```
show crypto isakmp sa
show crypto ipsec sa
```

✅ Estado esperado: `QM_IDLE`
✅ Tráfico: paquetes encapsulados/decapsulados aumentando

---

## ⚡ IKEv1 vs IKEv2

| Característica | IKEv1  | IKEv2      |
| -------------- | ------ | ---------- |
| Eficiencia     | Media  | Alta       |
| Seguridad      | Básica | Mejorada   |
| Complejidad    | Mayor  | Más simple |

📌 Recomendación: usar **IKEv2 en entornos modernos**

---

## 💀 Problemas comunes

* ❌ PSK incorrecta
* ❌ ACL mal definida
* ❌ Subred/Wildcard errónea
* ❌ Crypto map no aplicado
* ❌ NAT interfiriendo

---

## 📌 Conclusión

Este lab simula un escenario real de comunicación segura entre sedes, integrando conceptos clave de redes y ciberseguridad que son fundamentales tanto para administración como para pentesting.
