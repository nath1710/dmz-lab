# Laboratorio: Configuración de DMZ con Cisco Packet Tracer

Repositorio del laboratorio de implementación de una **DMZ (Zona Desmilitarizada)** segura utilizando un router Cisco ISR en Cisco Packet Tracer.

## Objetivo

Implementar una DMZ funcional que permita el acceso seguro a un servidor web desde una red externa (Internet), protegiendo al mismo tiempo la red LAN interna. Para ello se configura:

- Direccionamiento IP en tres redes (LAN interna, DMZ y red externa).
- NAT estático para publicar el servidor web hacia Internet.
- Listas de control de acceso (ACLs) para controlar el tráfico permitido entre zonas.

## Arquitectura de red

El laboratorio utiliza tres zonas separadas por un router que actúa como firewall:

- **LAN Interna** (`192.168.1.0/24`) — Red privada de usuarios internos.
- **DMZ** (`192.168.2.0/24`) — Zona intermedia donde se ubica el servidor web publicado.
- **Red Externa** (`192.168.3.0/24`) — Simula Internet y el acceso de usuarios externos.

## Contenido del repositorio

| Archivo / Carpeta | Descripción |
|-------------------|-------------|
| `README.md` | Este archivo. Descripción general del laboratorio y del repositorio. |
| `evidencias/` | Capturas de pantalla que documentan la configuración, las pruebas de conectividad y las verificaciones de seguridad realizadas. |
| `informe/Informe_DMZ_Laboratorio.md` | Informe completo del laboratorio: objetivo, topología, plan de direccionamiento, configuración aplicada, verificaciones y conclusiones. |
| `DMZ_PROJECT.pka` | Archivo de simulación de Cisco Packet Tracer con la topología implementada. |

## Requisitos

- Cisco Packet Tracer (versión recomendada 8.0 o superior).

## Resumen de la configuración

- Configuración de las tres interfaces del router (`Gi0/0`, `Gi0/1`, `Gi0/2`).
- NAT estático para mapear el servidor de la DMZ hacia la red externa.
- ACL que permite únicamente tráfico HTTP desde Internet hacia la DMZ.
- ACL que bloquea el tráfico originado en la DMZ hacia la LAN interna.

Para los detalles técnicos completos y los comandos aplicados, consulta el [informe del laboratorio](informe/Informe_DMZ_Laboratorio.md).

## Conclusión

El laboratorio demuestra cómo segmentar una red en zonas de confianza y aplicar políticas de seguridad mediante NAT y ACLs, garantizando que un servicio público sea accesible sin exponer la red interna.
