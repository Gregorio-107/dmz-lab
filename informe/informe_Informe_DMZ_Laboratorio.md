\# Informe de configuraciÃ³n de DMZ con Cisco Packet Tracer

\#\#\# 1\. Objetivo del laboratorio

Implementar y asegurar una Zona Desmilitarizada (DMZ) utilizando un router Cisco como firewall. El objetivo principal fue permitir el acceso público a un servidor web corporativo mientras se protege la integridad de la red interna (LAN), evitando que el servidor DMZ pueda iniciar conexiones hacia la red privada.

\---

\#\#\# 2\. TopologÃ­a implementada

\- Cantidad de redes: 3 subredes independientes.  
\- Dispositivos usados: 1 Router (Cisco 2911), 3 Switches (2960), 2 PCs (PC-PT) y 1 Servidor (Server-PT).  
\- Descripción de zonas:  
	\-LAN (Verde): Red interna confiable donde se encuentra la PC\_Internal.  
	\-DMZ (Naranja): Zona semicontrolada que aloja el servidor Web\_DMZ accesible desde el exterior.  
	\-Externa (Amarillo): Representa el internet o redes públicas fuera del control de la organización.

\#\#\# 3\. Plan de direccionamiento IP

Completa la tabla con las IPs asignadas (puedes copiarla del enunciado si no cambiÃ³).

| Dispositivo             | IP              | MÃ¡scara           | Gateway           |  
|-------------------------|------------------|-------------------|-------------------|  
| PC\_Internal             | 192.168.1.10     | 255.255.255.0     | 192.168.1.1       |  
| Server\_DMZ              | 192.168.2.10     | 255.255.255.0     | 192.168.2.1       |  
| PC\_External             | 192.168.3.10     | 255.255.255.0     | 192.168.3.1       |  
| Router\_FW Gi0/0 (LAN)   | 192.168.1.1      | 255.255.255.0     |  N/A              |  
| Router\_FW Gi0/1 (DMZ)   | 192.168.2.1      | 255.255.255.0     |  N/A              |  
| Router\_FW Gi0/2 (Ext)   | 192.168.3.1      | 255.255.255.0     |  N/A              |

\#\#\# 4\. ConfiguraciÃ³n aplicada (resumen)

\- Interfaces configuradas con \`ip address\` NAT:  
	ip nat inside source static 192.168.2.10 192.168.3.1  
	interface GigabitEthernet0/2  
 		ip nat outside  
\- ACLs:  
ip access-list extended DMZ\_A\_INTERNA  
 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255  
 permit ip any any

ip access-list extended INTERNET\_A\_DMZ  
 permit tcp any host 192.168.3.1 eq 80

\#\#\# 5\. Verificaciones realizadas  
\- Acceso web desde PC\_Internal: Exitoso (TCP Successful al servidor 192.168.2.10).  
\- Acceso web desde PC\_External: Exitoso mediante la IP pública (NAT funcional).  
\- Bloqueo de acceso desde DMZ a LAN: Verificado mediante ping (Status: Fail / Correct), confirmando que el servidor no puede atacar a la      PC interna.  
\- Puntuación Final: 100% de los ítems de evaluación completados satisfactoriamente.

\#\#\# 6\. Conclusiones y recomendaciones  
Aprendí la importancia del orden de las líneas en una ACL; un error en la secuencia puede bloquear el tráfico legítimo de retorno (como el TCP). También comprobé que en simuladores como Packet Tracer, a veces es necesario forzar el tráfico o hacer un "Power Cycle" para que las tablas ARP se actualicen tras un cambio de configuración. 

