 🛠️ Laboratorio: Ataque DHCP Starvation en GNS3

 Descripción
Este laboratorio demuestra un ataque de **DHCP Starvation** utilizando **Kali Linux** en GNS3.  
El objetivo es enviar múltiples solicitudes DHCP con direcciones MAC falsas para agotar el pool de direcciones del servidor legítimo.

---

 Topología
- **Router c7200** → Servidor DHCP legítimo.  
- **Kali Linux (atacante)** → Genera solicitudes DHCP falsas.  
- **VPCS (víctima)** → Cliente que intenta obtener IP.  
- **Switch Ethernet** → Conecta todos los dispositivos.  

---

Configuración del atacante
1. Guardar el script `dhcp_starvation.py` en Kali.  
2. Ajustar la interfaz de red (`iface = "eth0"`).  
3. Ejecutar:
   ```bash
   sudo python3 dhcp_starvation.py
 Comprobación del ataque
Antes del ataque:

Código
VPCS> ip dhcp
VPCS> show ip
→ La víctima recibe una IP legítima.

Después del ataque:

Código
VPCS> ip dhcp -r
VPCS> ip dhcp
→ El servidor no asigna IP (pool agotado).

Wireshark: Captura en el enlace muestra múltiples solicitudes DHCP Discover con MAC falsas.

 Mitigación en redes reales
DHCP Snooping → Solo permite respuestas de servidores autorizados.

Rate limiting → Limita número de solicitudes DHCP por puerto.

Port Security → Restringe número de MAC por puerto.

802.1X → Autenticación de clientes legítimos.

Ejemplo en Cisco IOS:

bash
Switch(config)# ip dhcp snooping
Switch(config)# ip dhcp snooping vlan 10
Switch(config)# interface FastEthernet0/1
Switch(config-if)# ip dhcp snooping trust
Switch(config-if)# storm-control broadcast level 5.00
