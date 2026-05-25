# Informe de configuración de DMZ con Cisco Packet Tracer

---

## 1. Objetivo del laboratorio

El objetivo de este laboratorio fue implementar una DMZ segura y funcional utilizando un router Cisco ISR en Cisco Packet Tracer. Se configuró direccionamiento IP, NAT estático y listas de control de acceso (ACLs) para permitir el acceso seguro a un servidor web desde la red externa, protegiendo al mismo tiempo la red LAN interna.

---

## 2. Topología implementada

- Cantidad de redes: 3 redes
- Dispositivos usados:
  - 1 Router Cisco ISR (Router_FW)
  - 3 Switches Cisco 2960
  - 1 PC_Internal
  - 1 Server-PT Web_DMZ
  - 1 PC_External

### Descripción de las zonas

- **LAN Interna:** Red privada donde se encuentra el usuario interno (PC_Internal).
- **DMZ:** Zona intermedia donde se ubica el servidor web accesible desde Internet.
- **Red Externa:** Simula Internet y contiene el PC_External que accede al servidor publicado.

---

## 3. Plan de direccionamiento IP

| Dispositivo             | IP              | Máscara           | Gateway           |
|-------------------------|------------------|-------------------|-------------------|
| PC_Internal             | 192.168.1.10     | 255.255.255.0     | 192.168.1.1       |
| Server_DMZ              | 192.168.2.10     | 255.255.255.0     | 192.168.2.1       |
| PC_External             | 192.168.3.10     | 255.255.255.0     | 192.168.3.1       |
| Router_FW Gi0/0 (LAN)   | 192.168.1.1      | 255.255.255.0     | N/A               |
| Router_FW Gi0/1 (DMZ)   | 192.168.2.1      | 255.255.255.0     | N/A               |
| Router_FW Gi0/2 (Ext)   | 192.168.3.1      | 255.255.255.0     | N/A               |

---

## 4. Configuración aplicada (resumen)

### Configuración de interfaces

```bash
Router> enable
Router# configure terminal
Router(config)# hostname Router_FW

Router_FW(config)# interface GigabitEthernet0/0
Router_FW(config-if)# ip address 192.168.1.1 255.255.255.0
Router_FW(config-if)# no shutdown

Router_FW(config)# interface GigabitEthernet0/1
Router_FW(config-if)# ip address 192.168.2.1 255.255.255.0
Router_FW(config-if)# no shutdown

Router_FW(config)# interface GigabitEthernet0/2
Router_FW(config-if)# ip address 192.168.3.1 255.255.255.0
Router_FW(config-if)# no shutdown
```

### Configuración NAT estático

```bash
Router_FW(config)# interface GigabitEthernet0/1
Router_FW(config-if)# ip nat inside

Router_FW(config)# interface GigabitEthernet0/2
Router_FW(config-if)# ip nat outside

Router_FW(config)# ip nat inside source static 192.168.2.10 192.168.3.1
```

### Configuración de ACLs

**Permitir únicamente HTTP desde Internet hacia la DMZ**

```bash
Router_FW(config)# access-list 101 permit tcp any host 192.168.3.1 eq 80
Router_FW(config)# interface GigabitEthernet0/2
Router_FW(config-if)# ip access-group 101 in
```

**Bloquear tráfico desde DMZ hacia LAN interna**

```bash
Router_FW(config)# access-list 100 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
Router_FW(config)# access-list 100 permit ip any any

Router_FW(config)# interface GigabitEthernet0/1
Router_FW(config-if)# ip access-group 100 in
```

---

## 5. Verificaciones realizadas

### Pruebas de conectividad

- ping desde PC_Internal a 192.168.1.1: ✅ Exitoso
- ping desde Server_DMZ a 192.168.2.1: ✅ Exitoso
- ping desde PC_External a 192.168.3.1: ✅ Exitoso

### Verificación de NAT y acceso web

- Acceso web desde PC_External a 192.168.3.1: ✅ Página web cargó correctamente.
- Acceso web desde PC_Internal a 192.168.2.10: ✅ Página web cargó correctamente.

### Verificación de seguridad ACL

- ping desde PC_External a 192.168.3.1: ❌ Bloqueado correctamente.
- ping desde Server_DMZ a 192.168.1.10: ❌ Bloqueado correctamente.

---

## 6. Conclusiones y recomendaciones

Durante este laboratorio aprendí a implementar una DMZ funcional utilizando Cisco Packet Tracer, configurando direccionamiento IP, NAT estático y ACLs para controlar el tráfico entre redes.

También comprendí la importancia de verificar primero la conectividad básica antes de aplicar políticas de seguridad, ya que errores en gateways, máscaras o ACLs pueden bloquear completamente la comunicación.

Como recomendación, es importante documentar correctamente las ACLs y validar cada cambio mediante pruebas de conectividad y acceso web para evitar configuraciones incorrectas.

