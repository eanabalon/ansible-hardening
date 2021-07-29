# README

## Playbooks

### add_admin_user.yml

Se encarga de crear el usuario a utilizar por ansible en todos los servidores.

Por defecto crea el usuario "ansible", lo agrega al sudousers y copia la llave SSH del servidor utilizado para correr los playbooks.

### setup.yml

Se encarga de preparar los servidores a ser administrados instalando los paquetes
necesarios y copiando las políticas a ser utilizadas.

### openscap_report.yml

Ejecuta un scan de todos los servidores y genera un reporte en HTML de cada servidor
en su estado actual, utilizando como métricas las políticas.

### hardening_playbook.yml

Encargado de configurar todos los puntos de seguridad definidos.

## Estructura

### Inventarios

En el repositorio existe la carpeta "inventories" y esta se divide en 3 subdirectorios más: "desarrollo", "qa", "prod".

Todos los servidores y configuraciones específicas son separadas por cada entorno.

Subdirectorios en cada ambiente:

- hosts: Listado de servidores, con dos grupos: all y proxy.
- group_vars/: Directorio que almacena archivos YML en donde se definen variables por cada grupo definido en el archivo hosts (nombre del archivo YML igual al del grupo definido en el archivo hosts)
  - all.yml
  - proxy.yml

## Setup

Ejecutar el playbook **add_admin_user.yml** especificando "user=ansible". Este playbook crea el usuario ansible con privilegios administrativos en todos los nodos listados en el entorno de desarrollo.

```bash
$ ansible-playbook add_admin_user.yml -e "user=ansible" -u root -k
```

Luego, testear haciendo ping:

```bash
$ ansible all -m ping  # ping a todo el grupo "all"
```

Por último, ejecutar el playbook **setup.yml**, este se encarga de copiar las políticas de seguridad definidas e instalar los paquetes necesarios para poder realizar los escaneos y reportes en todos los servidores.

```bash
$ ansible-playbook setup.yml
```

## Reportes

Para obtener un reporte de todos los servidores (nodos) se ejecuta el playbook **openscap_report**. Se puede especificar que ambiente (inventory) a utilizar.

```bash
$ ansible-playbook -i inventories/desarrollo/hosts openscap_report.yml
```
