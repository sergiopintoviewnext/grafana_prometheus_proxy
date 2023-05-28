## DESCRIPCIÓN 


Este rol realiza una instalación del servicio node_exporter mediante la descarga y utilizacion del paquete de github de node_exporter (https://github.com/prometheus/node_exporter/releases/) y la posterior creacion de este servicio. 

Solo debe ejecutarse en el/los hosts que actuarán como nodos de Prometheus, NO en el servidor Prometheus.




## VARIABLES


- **usuario**: indica el usuario que se creará en el sistema y que ejecutará el servicio prometheus que se crea. Por defecto: 'node_exporter'

- **version**: versión de node_exporter que se descargará (Lista versiones: https://github.com/prometheus/node_exporter/releases/). Por defecto: 1.5.0

- **path_binarios***: path donde se guardarán los binarios del servicio. Por defecto: '/usr/local/bin'

    
***NOTA**: al path no se le debe añadir al final el caracter "/". Ejemplos: /usr/local/bin (bien), /usr/local/bin/ (mal)

  


## TEMPLATES


Se dispone de un template:
- node_exporter.service.j2


'node_exporter.service.j2' se encarga de la creación del servicio node_exporter.




## INVENTARIO Y PLAYBOOK

Ejemplo de inventario:

    [PROMETHEUS]
    server

    [NODE_EXPORTER]
    nodo1
    nodo2
    nodo3

    
Este rol solo debe ejecutarse para los host del grupo 'NODE_EXPORTER'. Ejemplo playbook.yml que ejecute el rol utilizando el inventario anterior:


        - name: Instalacion node_exporter
          hosts: NODE_EXPORTER
          become: true
          roles:
            - role: node_exporter




## MOLECULE

Se ha realizado el testeo de este rol mediante:

        molecule 4.0.4 using python 3.9 
        ansible:2.14.3
        docker:2.1.0 from molecule_docker requiring collections: community.docker>=3.0.2 ansible.posix>=1.4.0

utilizando como contenedor de docker la imagen: registry.access.redhat.com/ubi8/ubi-init