Para AAP 2.7 deberia ser asi: 
  Un Job Template con Survey para indicar la URL y el hostname, y una credencial de tipo “Red Hat Ansible Automation Platform” asociada al template. Esa credencial puede inyectar automáticamente CONTROLLER_USERNAME, CONTROLLER_PASSWORD o CONTROLLER_OAUTH_TOKEN al runtime, sin poner secretos en la encuesta ni en el playbook. En AAP 2.7, Red Hat además indica que este tipo de credencial debe apuntar al Platform Gateway URL.


| Dato            | Origen         | Variable                 |
| --------------- | -------------- | ------------------------ |
| URL AAP         | Survey         | `aap_url`                |
| Host a eliminar | Survey         | `hostname_delete`        |
| Usuario         | Credential AAP | `CONTROLLER_USERNAME`    |
| Password        | Credential AAP | `CONTROLLER_PASSWORD`    |
| OAuth Token     | Credential AAP | `CONTROLLER_OAUTH_TOKEN` |
