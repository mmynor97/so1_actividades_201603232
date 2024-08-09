## UNIVERSIDAD SAN CARLOS DE GUATEMALA ##
## FACULTAD DE INGENIERIA ##
## ESCUELA DE CIENCIAS Y SISTEMAS ##
## Sistemas operativos 1##
## SECCIÓN A ##

### Actividad 1 ###

- **Nombre:** Mynor Francisco Morán García   **Carne:** 201603232

>>


<div id='content'/>

## Contenido

# Servicio de Saludo con Fecha Actual

Este es un servicio `systemd` que ejecuta un script que imprime un saludo junto con la fecha actual en un bucle infinito con una pausa de un segundo.

## Instalación

1. **Crear el script**: Guardar el siguiente script en `/usr/local/bin/saludo.sh`:

    ```bash
    #!/bin/bash
    
    while true; do
        echo "Hola, la fecha actual es: $(date)"
        sleep 1
    done
    ```

    Hacerlo ejecutable:

    ```bash
    sudo chmod +x /usr/local/bin/saludo.sh
    ```

2. **Crear la unidad de servicio**: Guardar el siguiente contenido en `/etc/systemd/system/saludo.service`:

    ```ini
    [Unit]
    Description=Saludo con fecha actual
    After=network.target
    
    [Service]
    ExecStart=/usr/local/bin/saludo.sh
    Restart=always
    User=nobody
    Group=nogroup
    
    [Install]
    WantedBy=multi-user.target
    ```

3. **Habilitar e iniciar el servicio**:

    ```bash
    sudo systemctl daemon-reload
    sudo systemctl enable saludo.service
    sudo systemctl start saludo.service
    ```

## Comprobación del Servicio

- **Estado del servicio**:

    ```bash
    sudo systemctl status saludo.service
    ```

- **Logs del servicio**:

    ```bash
    sudo journalctl -u saludo.service -f
    ```

## Desinstalación

Para detener y deshabilitar el servicio:

```bash
sudo systemctl stop saludo.service
sudo systemctl disable saludo.service

