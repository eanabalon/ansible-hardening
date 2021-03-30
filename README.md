# README

## Playbooks

### setup.yml

Se encarga de preparar los servidores a ser administrados instalando los paquetes
necesarios y copiando las políticas a ser utilizadas.

### openscap_report.yml

Ejecuta un scan de todos los servidores y genera un reporte en HTML de cada servidor
en su estado actual, utilizando como métricas las políticas.

### hardening_playbook.yml

Encargado de configurar todos los puntos de seguridad todos los puntos de seguridad
definidos.

## Estructura

### Inventarios

En el repositorio existe la carpeta "inventories" y esta se divide en 3 subdirectorios más: "desarrollo", "qa", "prod".

Todos los servidores y configuraciones específicas son separadas por cada entorno.

Subdirectorios en cada ambiente:

- hosts: Listado de servidores, con dos grupos: all y proxy.
- group_vars/: Variables a utilizar, archivo YML por cada grupo en archivo hosts
  - all.yml
  - proxy.yml

## Setup

Ejecutar el playbook **add_admin_user.yml** especificando "user=ansible" como admin. este playbook crea el usuario ansible con privilegios administrativos en todos los nodos listados en el entorno de desarrollo.

```bash
$ ansible-playbook add_admin_user.yml -e "user=ansible" -u root -k
```

Luego, testear haciendo ping:

```bash
$ ansible all -m ping  # ping a todo el grupo "all"
```
