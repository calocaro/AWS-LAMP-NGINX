# AWS-LAMP-NGINX
En este repositorio exlicaré cómo configurar AWS.

### Configuración de AWS

### Cuenta de AWS

Para comenzar, necesitamos meternos en el siguiente enlace, https://aws.amazon.com/es/, y crear una cuenta en AWS. Hay que tener en cuenta que en el registro de dicha cuenta se nos pedirá una tarjeta bancaria para poder finalizar el registro. Realmente no es necesario tener mucho dinero, solo te cobrarán sino céntimos o un euro. Aunque también depende de las funciones que quieras utilizar.
En nuestro caso, como todas las funciones que vamos a utilizar serán de forma gratuita, no nos hará falta tener mucho dinero en dicha tarjeta.
Entonces, el primer paso ya lo tenemos, que sería entrar en el enlace anterior; el segundo paso sería hacer click en la burbujita naranja que pone "Comience de forma gratuita", donde se nos mandará a la página de registro, donde tendremos que poner nuestro correo electrónico y el nombre de la cuenta. De forma que cuando hayamos rellenado esos dos campos, le daremos a "Verificar la dirección de correo electrónico". Procederemos entonces a ir a nuestro correo electrónico y cogerr el código de verficación y lo pondremos en la página de confirmación de AWS.

### Instancia EC2

La instancia EC2 está configurada con ubuntu 22.04 como imagen de máquina de Amazon (ami).

El aprovisionador remoto garantiza que se establezca una conexión ssh y que se instale python.

El aprovisionador ocal ejecuta el libro de jugadas ansible que instala e inicia un servidor web nginx y aloja un html simple.

Ahora bien, para poder lanzar una instancia, lo que es la creación de ella, sería siguiendo los pasos que a continuación se explican:

1. Primero vamos a "Servicios" y buscamos EC2.
2. Luego le daremos a "Lanzar una instancia".
3. Una vez que entramos en "Lanzar una instancia" , le pondremos nombre a la instancia. En nuestro caso le pondremos "nginx".
4. Lo siguiente será en el apartado de "Imágenes de aplicaciones y sistemas operativos (Amazon Machine Image), donde escogeremos la imagen de "Amazon Linux" que nos ofrece AWS.
5. El tipo de instancia será "t2.micro" , que es el apto para la capa gratuita.
6. En el apartado de "Par de claves (inicio de sesión)" , para conectarse a una máquina que crea en AWS, necesitamos un par de claves, con la clave privada en tu máquina y la clave pública en AWS.
7. Luego, en el apartado de "Configuraciones de red, creamos un grupo de seguridad de forma que, permitimos el tráfico de SSH desde (donde elijas) puede ser desde cualquier lugar, como desde un lugar en concreto; premitimos el tráfico de HTTPS desde Internet; y permitimos también el tráfico de HTTP desde Internet.
8. El apartado de la configuración de almacenamiento lo dejamos tal cual se nos presenta.
9. Por último escogemos el número de instancias que queremos hacer y  finalizamos lanzando la instancia.

Luego solo tendríamos que volver a "Instancias" y escoger la nueva que hemos lanzado y conectarla. En cuanto se conecte nos saldrá una ventana donde tendremos el apartado de "Cliente SSH".

### Comprobación del resultado

En el [ panel de EC2 ] (https://us-east-2.console.aws.amazon.com/ec2/v2/home?region=us-east-2), ahora debería ver una instancia en ejecución y 2 grupos de seguridad.
Al hacer click en "Instancias en ejecución" , aparecerá una lista de todas las instancias de EC2.
Copie la IP pública de la columna de la derecha en una nueva pestaña del navegador y mire su nuevo sitio web.

### Cliente SSH

1. Abrimos el terminal y  creamos un nuevo directorio *mkdir ProyAWS* 
2. Nos metemos en dicho directorio *cd ProyAWS*
3. Localizamos el archivo de clave privada. La clave utilizada para lanzar esta instancia es, en nuestro caso, llave.pem
4. Dentro de */ProyAWS* hacemos un *ls*
5. Y dentro, ejecutamos *chmod 400 llave.pem* para garantizar que la clave se pueda ver públicamente
6. Luego tendremos que instalar nginx mediante los siguientes comandos:
    - *sudo amazon-linux-extras list | grep nginx*
    - *sudo amazon-linux-extras enable nginx1*
    ...Ahora puedes instalar...
    - *sudo yum clean metadata*
    - *sudo yum -y install nginx*
    - *nginx -v*
    - *sudo amazon-linux-extras install -y nginx1*
    - *sudo systemctl start nginx*
    - *sudo systemctl enable nginx*
### Conexión a través de Visual Studio Code

Si queremos conectarnos por ssh con VSC tenemos que editar el documento situado en "authorized_keys" en la carpeta "/root/.ssh/authorized_keys" una vez dentro tenemos que borrar todo lo escrito antes de ssh.rsa.
Y luego entraríamos en VSC y procedemos a instalar la extensión de Remote-ssh para poder hacer la conexión. Cuando ésta se instale, lo que haremos será pulsar la tecla F1 para que salga el buscador y así nosotros poder buscar *"Agregar un nuevo host"* 

## Bloqueo de acceso de IP concretas

Para ello debemos crear un fichero llamado [```blacklist.conf```]() donde añadimos las políticas restrictivas sobre las IPS que querramos indicar.

```yml

deny 192.168.1.0/24;
allow all;

```
Para ello debemos ir al fichero de configuración [```nginx.conf```] y en el apartado server añadir un include del fichero de de la blacklist creada previamente.

```yml

    server {
        listen       80;
        listen       [::]:80;
        server_name  _;
        #root         /usr/share/nginx/html;
        root /home/ec2-user/html;
        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/blacklist.conf

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }
```

Posteriormente debemos dar permisos al usuario de poder leer ficho fichero para ello podemos cambiarle la propiedad del mismo con el comando:

`$ chown ec2-user /etc/nginx/blacklist.conf`

Seguidamente de ejecutar el comando reiniciamos el servicio de nginx y ya estaría activo el bloqueo de las IPS indicadas, cada vez que modifiquemos el archivo debemos reiniciar el servicio.
