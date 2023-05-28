## DESCRIPCIÓN 


Este rol realiza una instalación del servicio grafana mediante la descarga y utilizacion del paquete del repositorio de grafana. 

No es obligatorio ejecutarse en el host que actuará como servidor "Prometheus"




## INVENTARIO Y PLAYBOOK

Ejemplo de inventario (en este caso se instalaría sobre server, donde tambien estaria instalado prometheus):

    [PROMETHEUS]
    server

    [NODE_EXPORTER]
    nodo1
    nodo2
    nodo3

    [PROXY]
    proxy


Este rol puede ejecutarse para los host del grupo 'PROMETHEUS'. Ejemplo playbook.yml que ejecute el rol utilizando el inventario anterior:


        - name: Instalacion grafana
          hosts: PROMETHEUS
          become: true
          roles:
            - role: grafana



Otro ejemplo (se instalaria en un host a parte al servidor prometheus):

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

    
Este rol puede ejecutarse en un host a parte del grupo 'PROMETHEUS'. Ejemplo playbook.yml que ejecute el rol utilizando el inventario anterior:


        - name: Instalacion grafana
          hosts: GRAFANA
          become: true
          roles:
            - role: grafana



## MOLECULE

Se ha realizado el testeo de este rol mediante:

        molecule 4.0.4 using python 3.9 
        ansible:2.14.3
        docker:2.1.0 from molecule_docker requiring collections: community.docker>=3.0.2 ansible.posix>=1.4.0

utilizando como contenedor de docker la imagen: registry.access.redhat.com/ubi8/ubi-init