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
6. En el apartado de "Par de claves (inicio de sesión)" , para conectarse a una máquina que crea en AWS, necesitamos un par de claves, con la clave privada en tu máquina y la clave pública en AWS

### Comprobación del resultado

En el [ panel de EC2 ] (https://us-east-2.console.aws.amazon.com/ec2/v2/home?region=us-east-2), ahora debería ver una instancia en ejecución y 2 grupos de seguridad.
Al hacer click en "Instancias en ejecución" , aparecerá una lista de todas las instancias de EC2.
Copie la IP pública de la columna de la derecha en una nueva pestaña del navegador y mire su nuevo sitio web.
