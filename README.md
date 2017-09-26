# Docker-run (Ansible Role)
=========

Reusable role for any Docker containers (like docker-compose).

Requirements
------------
* Ansible 2.3.0

## Variables
```
docker_container_prefix: prefix
# Create network
docker_container_network: network_name
# Create volume
docker_container_volume: volume_name
# Check http port when container is started
docker_check_port: 8080
# HTTP Headers
docker_check_headers:
    API-Key: "test_key"
    Host: "example.com"

docker_container:
  - name: container-name
    path: ../path-to-build     # OPTIONAL | Local docker build if needed
    image: image/name
    restart: always
    volumes:
      - "volume-name:/path/mount/to"
    env:
      "{{ env_variables_dict }}"
    networks:
      - networks_to_connect
```
## Playbook example:
```
---
- hosts: localhost
  connection: local
  roles:
    - docker-run

  vars:
    docker_container_prefix: mon
    docker_container_network: monitoring
    docker_container_volume:
        - grafana_data
        - db_data

    docker_container:
      - name: postgres
        path: ../postgres
        restart: always
        volumes:
          - "db_data:/var/lib/postgresql/data"
        env:
          "{{ postgres_variables }}"
        networks:
          - monitoring

      - name: grafana
        image: grafana/grafana
        ports:
          - 3000:3000
        volumes:
          - grafana_data:/var/lib/grafana
        env:
          "{{ grafana_variables }}"
        networks:
          - monitoring
```

License
-------
Apache 2.0

Author Information
------------------
Ivan Tuzhilkin ivan.tuzhilkin@gmail.com
