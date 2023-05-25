DESCRIPCIÓN 


Este rol realiza una instalación del servicio prometheus mediante la descarga y utilizacion del paquete de github de Prometheus (https://github.com/prometheus/prometheus/releases/) y la posterior creacion de este servicio. 

Solo debe ejecutarse en el host que actuará como servidor "Prometheus"



VARIABLES


    - usuario: indica el usuario que se creará en el sistema y que ejcutará el servicio prometheus que se crea. Por defecto: 'prometheus'

    - version_prometheus: versión de prometheus que se descargará (Lista versiones: https://github.com/prometheus/prometheus/releases/). Por defecto: 2.44.0

    - path_binarios*: path donde se guardarán los binarios del servicio. Por defecto: '/usr/local/bin'

    - path_config*: path donde se guardará el fichero de configuración. Por defecto: '/etc/prometheus'

    - path_storage*: path donde se guardará el storage del servicio. Por defecto: '/var/lib/prometheus'

    
    * NOTA: a la ruta no se le debe añadir al final el caracter "/". Ejemplos: /usr/local/bin (bien), /usr/local/bin/ (mal)


  

TEMPLATES


    Se disponen de dos templates:
        - prometheus.service.j2
        - prometheus.yml.j2

    'prometheus.yml.j2' contiene la configuración del servicio prometheus.
    En 'prometheus.yml.j2' se ha añadido la linea "#targets node_exporter" (linea 34) que servirá como linea de referencia para añadir posteriormente las lineas de los nodos de node_exporter.


    'prometheus.service.j2' se encarga de la creación del servicio prometheus.




HANDLERS


    El rol contiene un handler que reiniciará el servicio 'prometheus' en caso de algun cambio realizado a posteriori en el template 'prometheus.yml.j2'.




INVENTARIO Y PLAYBOOK

    Ejemplo de inventario:

    [PROMETHEUS]
    server

    [NODE_EXPORTER]
    nodo1
    nodo2
    nodo3

    
    Este rol solo debe ejecutarse para los host del grupo 'PROMETHEUS'. Ejemplo playbook.yml que ejecute el rol utilizando el inventario anterior:


        - name: Instalacion Prometheus
          hosts: PROMETHEUS
          become: true
          roles:
            - role: prometheus




MOLECULE

    Se ha realizado el testeo de este rol mediante:

        molecule 4.0.4 using python 3.9 
        ansible:2.14.3
        docker:2.1.0 from molecule_docker requiring collections: community.docker>=3.0.2 ansible.posix>=1.4.0

    utilizando como contenedor de docker la imagen: registry.access.redhat.com/ubi8/ubi-init