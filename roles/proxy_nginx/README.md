## DESCRIPCIÓN 


Este rol realiza la instalación y configuración de nginx como proxy inverso para redireccionar el tráfico entrante por el puerto 80 TCP hacia el servidor grafana (puerto 3000 TCP).


## VARIABLES


    - servidor_http: se indicará la dirección al servidor al que se quiere redirigir el trafico entrando indicando el puerto del otro servidor. Ejemplo: http://ip_address:8080. En este caso la ip_address será el servidor grafana.


## TEMPLATES


Se dispone de un dichero template. Este dichero corresponde con el fichero de configuracion de nginx (/etc/nginx/nginx.conf). En el se configura el proxy mediante la variable anteriormente indicada, donde se apunta la redirección al host indicado en esta.



## HANDLERS


Existe un handler en este rol que se ejecuta cuando existe algun cambio en la tarea de copiar el template. Se ha añadido por si en caso de realizar algun cambio a posteriori en el fichero de configuración se reinicie el servicio nginx.




## INVENTARIO Y PLAYBOOK

Ejemplo de inventario (en este caso se instalaría sobre server, donde tambien estaria instalado prometheus y grafana):

    [PROMETHEUS]
    server

    [NODE_EXPORTER]
    nodo1
    nodo2
    nodo3


Este rol puede ejecutarse para los host del grupo 'PROMETHEUS' donde estaria instalado tanto prometheus y grafana. Ejemplo playbook.yml que ejecute el rol utilizando el inventario anterior:


        - name: Instalacion y configuracion nginx
          hosts: PROMETHEUS
          become: true
          roles:
            - role: proxy_nginx



Otro ejemplo (se instalaria en un host a parte al servidor prometheus y/o grafana):

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

    
Este rol puede ejecutarse en un host a parte del grupo 'PROMETHEUS' y/o 'GRAFANA'. Ejemplo playbook.yml que ejecute el rol utilizando el inventario anterior:


        - name: Instalacion y configuracion nginx
          hosts: PROXY
          become: true
          roles:
            - role: proxy_nginx



## MOLECULE

Se ha realizado el testeo de este rol mediante:

        molecule 4.0.4 using python 3.9 
        ansible:2.14.3
        docker:2.1.0 from molecule_docker requiring collections: community.docker>=3.0.2 ansible.posix>=1.4.0

utilizando como contenedor de docker la imagen: registry.access.redhat.com/ubi8/ubi-init