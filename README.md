# Role Ansible: zabbix7_server_ubuntu2404

Esta role instala e configura o **Zabbix Server 7.0 LTS** no **Ubuntu 24.04**, utilizando o **MySQL 8** como banco de dados.

## 📦 O que essa role faz

- Instala os pacotes necessários para o Zabbix Server
- Configura o serviço `zabbix-server`
- Aplica as configurações do `zabbix_server.conf` via template Jinja2
- Usa MySQL como backend
- Cria handlers para reiniciar o serviço caso o arquivo de configuração seja alterado

## 📁 Estrutura da Role

```
zabbix7_server_ubuntu2404/
├── defaults/
│   └── main.yml
├── handlers/
│   └── main.yml
├── meta/
│   └── main.yml
├── tasks/
│   └── main.yml
├── templates/
│   └── zabbix_server.conf.j2
├── tests/
│   └── test.yml
├── vars/
│   └── main.yml
└── README.md
```

## ⚙️ Variáveis padrão (`defaults/main.yml`)

```yaml
mysql_database: zabbix
mysql_zabbix_user: zabbix
mysql_zabbix_password: SenhaZabbix
```

Essas variáveis podem ser sobrescritas conforme necessidade no playbook ou inventário.

## 🧾 Template de configuração (`templates/zabbix_server.conf.j2`)

```ini
DBHost=localhost
DBName={{ mysql_database | default('zabbix') }}
DBUser={{ mysql_zabbix_user | default('zabbix') }}
DBPassword={{ mysql_zabbix_password | default('zabbix_pass') }}
DBPort=3306
ListenPort=10051

StartPollers=5
StartPollersUnreachable=1
StartPingers=2
StartTrappers=5
StartDiscoverers=1
CacheSize=32M
HistoryCacheSize=16M
TrendCacheSize=8M
ValueCacheSize=32M

PidFile=/run/zabbix/zabbix_server.pid
SocketDir=/run/zabbix

LogFile=/var/log/zabbix/zabbix_server.log
LogFileSize=10
DebugLevel=3
```

## 🔁 Handler (`handlers/main.yml`)

```yaml
- name: Restart Zabbix Server
  ansible.builtin.service:
    name: zabbix-server
    state: restarted
  become: true
```

## ✅ Teste local (`tests/test.yml`)

```yaml
- name: Teste da role zabbix7_server_ubuntu2404
  hosts: localhost
  become: true
  gather_facts: true

  roles:
    - role: zabbix7_server_ubuntu2404
```

## 🧪 Teste via linha de comando

```bash
ansible-playbook roles/install_monitoring/zabbix7_server_ubuntu2404/tests/test.yml
```

## 📋 Requisitos

- Ubuntu 24.04 LTS
- Ansible 2.9 ou superior
- MySQL 8 disponível

## 🏷️ Tags Ansible Galaxy

- `zabbix`
- `mysql`
- `ubuntu`
- `monitoring`
- `network`
- `server`

## 🧑 Autor

**José Zipf**  
Licença: MIT  
[https://nototi.com.br](https://nototi.com.br)
