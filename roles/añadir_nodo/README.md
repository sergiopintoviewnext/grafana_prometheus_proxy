## DESCRIPCIÓN 


Este rol agrega los nodos con node_exporter al servidor prometheus mediante la edición del fichero de configuración de prometheus. En concreto añade las lineas con los targets de los hosts que se desean añadir. 

Utiliza los hostname o ip address del inventario (grupo NODE_EXPORTER) para la configuracion. Por lo que es necesario incluir en el grupo '[NODE_EXPORTER]' los hosts que se quieren añadir. 

**IMPORTANTE**: En caso de utIlizar hostnames en el inventario, asegurarse que el servidor prometheus es capaz de resolverlas.

Este rol solo debe ejecutarse en el host que actua como servidor "Prometheus".



## VARIABLES


- **path_config***: path donde se ubica el fichero de configuración en el servidor prometheus. Por defecto: '/etc/prometheus'

    
***NOTA**: a la ruta no se le debe añadir al final el caracter "/". Ejemplos: /usr/local/bin (bien), /usr/local/bin/ (mal)




## HANDLERS


El rol contiene un handler que reiniciará el servicio 'prometheus' en caso de haber algun cambio en el fichero de configuración de prometheus (por defecto, /etc/prometheus/prometheus.yml).




## INVENTARIO Y PLAYBOOK

Ejemplo de inventario:

    [PROMETHEUS]
    server

    [NODE_EXPORTER]
    nodo1
    nodo2
    nodo3


Para la ejecucion de este rol es importante tener un grupo llamado 'NODE_EXPORTER' en el que se ubicarán los hostname o ip address de los nodos de node_exporter que se quieren agregar al servidor prometheus.

    
Este rol solo debe ejecutarse para los host del grupo 'PROMETHEUS'. Ejemplo playbook.yml que ejecute el rol utilizando el inventario anterior:


        - name: Instalacion Prometheus
          hosts: PROMETHEUS
          become: true
          roles:
            - role: añadir_nodo




## MOLECULE

Se ha realizado el testeo de este rol mediante:

        molecule 4.0.4 using python 3.9 
        ansible:2.14.3
        docker:2.1.0 from molecule_docker requiring collections: community.docker>=3.0.2 ansible.posix>=1.4.0

utilizando como contenedor de docker la imagen: registry.access.redhat.com/ubi8/ubi-init