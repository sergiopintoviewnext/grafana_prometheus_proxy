## DESCRIPCIÓN


Se disponen de **5 roles**:

- **prometheus**: realiza una instalación del servicio prometheus mediante la descarga y utilizacion del paquete de github de Prometheus (https://github.com/prometheus/prometheus/releases/) y la posterior creación de este servicio.

- **node_exporter**: realiza una instalación del servicio node_exporter mediante la descarga y utilizacion del paquete de github de node_exporter (https://github.com/prometheus/node_exporter/releases/) y la posterior creacion de este servicio.

- **añadir_nodo**: este rol agrega los nodos con node_exporter al servidor prometheus mediante la edición del fichero de configuración de prometheus. En concreto añade las lineas con los targets de los hosts que se desean añadir.


- **grafana**: con este rol se realiza la instalación de grafana.


- **proxy_nginx**: realiza la instalación y configuración de nginx como proxy inverso para redireccionar el tráfico entrante por el puerto 80 TCP hacia el servidor grafana (puerto 3000 TCP)


En cada rol existe un README.md donde se explica particularmente cada uno de ellos más especificamente.




## INVENTORY


Para la correcta ejecución de estos roles se debe tener en el inventario un grupo llamado 'NODE_EXPORTER' con los hosts donde se instalará node_exporter y que posteriormente se agregarán al servidor Prometheus. Este grupo de inventario es necesario para la ejecución del rol 'añadir_nodo' ya que el rol lo utiliza para identificar los hosts a agregar al servidor prometheus.


Ejemplo de inventario:

		[PROMETHEUS]
		server

		[NODE_EXPORTER]
		nodo1
		nodo2
		nodo3
		
		[PROXY]
		proxy


Otro ejemplo:

		[PROMETHEUS]
		server

    [GRAFANA]
    grafana

		[NODE_EXPORTER]
		nodo1
		nodo2
		nodo3
		
		[PROXY]
		proxy




## PLAYBOOK


Ejemplo de playbook.yml para la ejecución de estos roles (grafana y prometheus en el mismo servidor):


    - name: Instalación node_exporter
      hosts: NODE_EXPORTER
      become: true
      roles:
        - role: node_exporter

    - name: Instalación y configuracion Prometheus
      hosts: PROMETHEUS
      become: true
      roles:
        - role: prometheus
        - role: añadir_nodo
        - role: grafana
         
		- name: Instalación y configuracion proxy nginx
      hosts: PROXY
      become: true
      roles:
        - role: proxy_nginx



Otro ejemplo (servidor grafana en otro host):


    - name: Instalación node_exporter
      hosts: NODE_EXPORTER
      become: true
      roles:
        - role: node-exporter

    - name: Instalación y configuracion Prometheus
      hosts: PROMETHEUS
      become: true
      roles:
        - role: prometheus
        - role: añadir_nodo

    - name: Instalación grafana
      hosts: GRAFANA
      become: true
      roles:
        - role: grafana

		- name: Instalación y configuracion proxy nginx
      hosts: PROXY
      become: true
      roles:
        - role: proxy_nginx


    
El rol 'añadir_nodo' se puede usar individualemente más adelante para agregar nuevos nodos de node_exporter al servidor prometheus. Unicamente, se debe añadir en el grupo 'NODE_EXPORTER' el hostname o ip address de este nuevo nodo a incluir.