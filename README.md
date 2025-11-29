# **Wazuh Monitoring Stack – Docker Compose**

![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker)
![Zabbix](https://img.shields.io/badge/Wazuh-4.14-D50000?style=for-the-badge)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-17.6-336791?style=for-the-badge&logo=postgresql)

Stack completo y modular para desplegar **Wazuh 4.14** en entornos de producción utilizando **Docker Compose**.
Incluye base de datos, servidor, interfaz web.

---

# 📦 **Arquitectura del Stack**

| Componente          | Versión | Descripción                             |
| ------------------- | ------- | --------------------------------------- |
| **Wazuh Indexer**   | 4.14.1  | Base de datos de Wazuh                  |
| **Wazuh Manager**   | 4.14.1  | Servidor principal                      |
| **Wazuh Dashborad** | 4.14.1  | Interfaz gráfica                        |
| **Wazuh Agent**     | 4.14.1  | Agente avanzado para monitoreo del host |

---

# 🔌 **Mapa de Puertos**

| Puerto    | Protocolo | Uso                                 |
| --------- | --------- | ----------------------------------- |
| **1514**  | TCP       | Agents ↔ Server                     |
| **1515**  | TCP       | Registro y Autenticación de Agentes |
| **514**   | UDP       | Syslog                              |
| **55000** | TCP       | Wazuh server API                    |
| **9200**  | TCP       | Wazuh indexer API                   |
| **443**   | TCP       | Interfaz Web (HTTP)                 |

---

# 💾 **Volúmenes Persistentes**

| Volumen                   | Contenedor      | Uso                          |
| ------------------------- | --------------- | ---------------------------- |
| `wazuh_api_configuration` | Wazuh Manager   | Base de datos Wazuh          |
| `wazuh_etc`               | Wazuh Manager   | Configuración y runtime      |
| `wazuh_logs`              | Wazuh Manager   | Almacenamiento de traps SNMP |
| `wazuh_queue`             | Wazuh Manager   | Configuración del frontend   |
| `wazuh_var_multigroups`   | Wazuh Manager   | Configuración del frontend   |
| `wazuh_integrations`      | Wazuh Manager   | Certificados SSL/TLS         |
| `wazuh_active_response`   | Wazuh Manager   | Certificados SSL/TLS         |
| `wazuh_agentless`         | Wazuh Manager   | Certificados SSL/TLS         |
| `wazuh_wodles`            | Wazuh Manager   | Certificados SSL/TLS         |
| `filebeat_etc`            | Wazuh Manager   | Certificados SSL/TLS         |
| `filebeat_var`            | Wazuh Manager   | Certificados SSL/TLS         |
| `wazuh-indexer-data`      | Wazuh Indexer   | Base de datos Wazuh          |
| `wazuh-dashboard-config`  | Wazuh Dashboard | Certificados SSL/TLS         |
| `wazuh-dashboard-custom`  | Wazuh Dashboard | Certificados SSL/TLS         |

Todos los volúmenes persisten automáticamente entre reinicios.

---

# 🚀 **Implementación Rápida**

### **1. Prerrequisitos**

- **Docker Engine 20.10+**
- **Docker Compose 2.0+**
- **Puertos 8080/4443/10051/162 disponibles**

---

## **2. Descargar o Clonar el repositorio**

````bash
wget https://github.com/rsol9000/zabbix-docker/archive/main.zip -O zabbix-stack.zip
cd zabbix-stack

```bash
git clone <repository-url>
cd zabbix-stack
````

---

## **3. Configurar variables de entorno**

```bash
cp .env.pub .env
nano .env
```

### **Variables críticas**

```env
POSTGRES_USER=zabbix_admin
POSTGRES_PASSWORD=super_secure_password
```

### **Variables opcionales**

```env
ZBX_SERVER_HOST=zabbix-server
ZBX_SERVER_NAME=Zabbix Monitoring
ZBX_TIMEZONE=America/Costa_Rica
```

---

# ▶️ **Despliegue**

```bash
docker-compose up -d
```

Comprobar estado:

```bash
docker-compose ps
```

Logs del servidor:

```bash
docker-compose logs -f zabbix-server
```

---

# 🌐 **Acceso a la Interfaz Web**

| Protocolo | URL                          |
| --------- | ---------------------------- |
| **HTTP**  | `http://<IP-SERVIDOR>:8080`  |
| **HTTPS** | `https://<IP-SERVIDOR>:4443` |

---

# 🔐 **Habilitar HTTPS**

```bash
cp ssl/cert.pem ssl/key.pem ./certificates/
docker-compose restart zabbix-web
```

Requiere certificados válidos (Let’s Encrypt, ACME u otros).

---

# 🖥️ **Agentes Zabbix**

El stack incluye **Zabbix Agent2 local**.
Para agentes remotos:

```bash
ZabbixServer=<SERVER_IP>
ZabbixServerActive=<SERVER_IP>
```

Ideal para servidores Linux, contenedores o equipos remotos.

---

# 📡 **Monitoreo por SNMP Traps**

El receptor escucha en **UDP 162**.

Ejemplo para routers/switches:

```
snmp-server host <ZABBIX_SERVER_IP> traps version 2c public
```

---

# 🛠️ **Comandos Útiles de Administración**

```bash
# Estado del stack
docker-compose ps

# Logs globales
docker-compose logs -f

# Backup de la base de datos
docker-compose exec postgres pg_dump -U $POSTGRES_USER zabbix > backup.sql

# Reinicio de servicios
docker-compose restart zabbix-server

# Bajar el stack completo
docker-compose down
```

---

# 🔒 **Recomendaciones de Seguridad**

- Cambiar todas las contraseñas por defecto
- Restringir puertos con firewall (ufw, nftables)
- Usar HTTPS en producción
- Implementar políticas de backup automático
- Revisar logs de acceso y traps regularmente
- Mantener las imágenes de Docker actualizadas

---

# 📝 **Notas de Versión**

- **Zabbix 7.4.6** — Versión LTS estable
- **PostgreSQL 17.6** — Óptimo para cargas intensivas
- **Agent2** — Mejor capacidad para contenedores, plugins modernos

---
