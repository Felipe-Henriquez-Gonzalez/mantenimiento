Para AAP 2.7 deberia ser asi: 
  Un Job Template con Survey para indicar la URL y el hostname, y una credencial de tipo “Red Hat Ansible Automation Platform” asociada al template. Esa credencial puede inyectar automáticamente CONTROLLER_USERNAME, CONTROLLER_PASSWORD o CONTROLLER_OAUTH_TOKEN al runtime, sin poner secretos en la encuesta ni en el playbook. En AAP 2.7, Red Hat además indica que este tipo de credencial debe apuntar al Platform Gateway URL.


| Dato            | Origen         | Variable                 |
| --------------- | -------------- | ------------------------ |
| URL AAP         | Survey         | `aap_url`                |
| Host a eliminar | Survey         | `hostname_delete`        |
| Usuario         | Credential AAP | `CONTROLLER_USERNAME`    |
| Password        | Credential AAP | `CONTROLLER_PASSWORD`    |
| OAuth Token     | Credential AAP | `CONTROLLER_OAUTH_TOKEN` |


Credencial en AAP

En Automation Execution → Infrastructure → Credentials, puedes crear una credencial del tipo:

Red Hat Ansible Automation Platform

con:

Red Hat Ansible Automation Platform:
https://aap.midominio.cl

OAuth Token:
************************

o alternativamente usuario/password.

AAP inyecta automáticamente variables como:

CONTROLLER_HOST
CONTROLLER_USERNAME
CONTROLLER_PASSWORD
CONTROLLER_OAUTH_TOKEN

Red Hat documenta específicamente estos injectors para la credencial AAP.

Por eso el playbook puede hacer:

controller_token: "{{ lookup('env', 'CONTROLLER_OAUTH_TOKEN') }}"

sin que el token esté en Git, el playbook o la Survey.

La Survey

En el Job Template activaría Survey y pondría, por ejemplo:

AAP URL

Prompt:
URL de Ansible Automation Platform

Answer variable name:
aap_url

Answer type:
Text

Required:
Yes

Ejemplo ingresado por el operador:

https://aap.cliente.cl

Y:

Hostname

Prompt:
Nombre del host a eliminar de Host Metrics

Answer variable name:
hostname_delete

Answer type:
Text

Required:
Yes

Ejemplo:

server-old-01.example.com

La ejecución quedaría conceptualmente:

              AAP Job Template
                     │
          ┌──────────┴──────────┐
          │                     │
       Survey               Credential
          │                     │
     aap_url              OAuth Token
 hostname_delete          (secreto)
          │                     │
          └──────────┬──────────┘
                     ▼
                  Playbook
                     │
                     ▼
          GET /host_metrics/
                     │
                     ▼
           Busca hostname exacto
                     │
                     ▼
              obtiene ID
                     │
                     ▼
       DELETE /host_metrics/ID/
                     │
                     ▼
               Soft Delete

Hay una mejora que haría para un entorno productivo: no preguntaría la URL por Survey si siempre se va a operar contra el mismo AAP. La propia credencial AAP ya inyecta CONTROLLER_HOST, por lo que podrías obtenerla así:

controller_host: "{{ lookup('env', 'CONTROLLER_HOST') }}"

y entonces la Survey tendría solamente el hostname. Esto evita que alguien escriba accidentalmente otra URL y envíe la credencial almacenada a un servidor incorrecto. La documentación de AAP 2.7 confirma tanto CONTROLLER_HOST como los demás valores inyectados por este credential type.

idealmente no poner 
validate_certs: false en producción. 
Lo ideal es:

validate_certs: true

con la CA correspondiente disponible en el Execution Environment.



