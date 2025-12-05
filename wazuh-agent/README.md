# 🚀 **Instalación del agente**

### **1. Prerrequisitos**

- **El servicio docker manager debe existir**
- **Se debe conocer la dirección IP o FQDN del Wazuh manager**
- **Los puertos 1515 y 1514 debes ser accesibles desde el Host a monitorear hacia el host que alberga a el servicio de wazuh manager**

Instalación del agente con el grupo por defecto

```bash
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.14.1-1_amd64.deb && sudo WAZUH_MANAGER='10.20.200.106' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='docker-siua' dpkg -i ./wazuh-agent_4.14.1-1_amd64.deb
````
Si presenta error por dependencias, se puede usar en Linux

```bash
apt --fix-broken install
```
Si presenta error por permisos usar "sudo"

Para terminar 
```bash
systemctl daemon-reload
````
```bash
systemctl enable wazuh-agent
````
```bash
systemctl start wazuh-agent
````
Ahora el agente llamado "docker-siua" debería apracer en la interfaz web de Wazuh