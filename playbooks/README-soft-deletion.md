El Playbook de soft-deletion debera contener los siguientes variables, que pueden ser inyectadas mediante Survey o mediante una credencial


| Dato            | Origen         | Variable                 |
| --------------- | -------------- | ------------------------ |
| URL AAP         | Survey         | `aap_url`                |
| Host a eliminar | Survey         | `hostname_delete`        |
| Usuario         | Credential AAP | `CONTROLLER_USERNAME`    |
| Password        | Credential AAP | `CONTROLLER_PASSWORD`    |
| OAuth Token     | Credential AAP | `CONTROLLER_OAUTH_TOKEN` |
