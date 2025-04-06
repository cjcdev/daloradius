# Daloradius  - WPA Enterprise Testing

- This is a fork of [daloradius](https://github.com/lirantal/daloradius).
- Original README.md is at [README-original.md](README-original.md)
- Some tweaks to make the container build work (update maria version) in `docker-compose.yml`.
- Some tweaks to make it work with a client AP for WPA Enterprise testing in `init-freeradius.sh`.


## How to use

1. Set the `DEFAULT_CLIENT_IP` in `docker-compose.yml` to the IP Address of the Access Point.
2. Build and bring up containers
    ```
    docker compose up -d
    ```
3. Once everyting is up and running you will need to configure a couple of things in the dalraius webapp.
    - http://container-ip:8000/
    - Username: administrator
    - Password: radius
    - Management -> User -> New User
        - Auth Type: username and password
        - Group: Empty
        - Username: <your choice, used by device to login>
        - Password: <your choice>
    - Management -> Nas -> New NAS
        - NAS IP/Host: value of DEFAULT_CLIENT_IP in docker-compose.yml
        - NAS Secret: DEFAULT_CLIENT_SECRET in docker-compose.yml
        - NAS Type: other
        - NAS shortname: makeup a name

4. Configure access point to use WPA2/3 Enterprise and to point to the radius server running in the container.
    - IP: IP of the container
    - Port: 1812
    - Secret: Same value as the DEFAULT_CLIENT_SECRET in docker-compose.yml

5. Connect to the access point with a device with the login:
    - identity: Username of user created in daloradius webapp
    - password: Password of user created in daloradius webapp
    - Cert: dont' validate
