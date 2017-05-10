# HOW-TO-CONFIG-SSH-GIT

En este tutorial vamos a configurar nuestra cuenta de github para poder utilizar la autenticación vía ssh.

**SSH**

Hemos hablado de configurar SSH pero, ¿qué es SSH?

SSH es un protocolo utilizado para acceder a máquinas virtuales. Frente a otros protocolos de acceso como Telnet SSH nos ofrece una mayor seguridad al utilizar cifrado en los mensajes, dificultando mucho la tarea de extraer información en el paso de información.

Para asegurar esta seguridad valga la redundacia se utiliza un método criptográfico mediante el cual el propietarioa genera un par de claves, una privada y una pública. Ofrecerá su clave pública para que los remitentes de los mensajes puedan cifrar el mensaje con ella y el propietario utilizará su clave privada secreta solo conocida por él para decodificar el mensaje. Si el proceso del envío del mensaje ocurre a la inversa, es decir, el usuario propietario cifra su mensaje con su clave privada, cualquier con clave pública podrá descifrarlo. Aunque con esto último no se consigue mucha seguridad si que se consigue identificación y autentificación del emisor.

Para lograr esta seguridad con las claves se utilizan números primos muy grandes siendo impracticable el descomponer correctamente el mensaje sin conocer la clave privada.

**CONFIGURACIÓN DE LA CONEXIÓN**

A continuación veremos los pasos que hay que realizar para poder conectarnos vía SSH a nuestro respositorio GitHub:


En primer lugar veremos si tenemos creada la carpeta donde tenemos nuestras claves ssh (~/.ssh). Si no existe la creamos:

```sh
mkdir ~/.ssh
```


Una vez tengamos la carpeta ssh creada entramos en ella:

```sh
cd ~/.ssh
```



Ahora utilizaremos *ssh-keygen* para la generación de nuestro par de claves. Cuando nos pida introducir la frase de acceso poder pulsar la tecla *Enter* para no tener que introducir ninguna:

```sh
ssh-keygen -t rsa -C "tuemail@domain.com" -f github
```

Enter passphrase (empty for no passphrase):
Enter same passphrase again:

Nos deberá salir una salida parecida a esta:

Your identification has been saved in github.
Your public key has been saved in github.pub.
The key fingerprint is:
17:87:16:d4:49:e6:50:bb:1b:9e:40:8d:5d:25:c0:5b tuemail@domain.com
The key's randomart image is:
```
+--[ RSA 2048]----+
|         .+==oo..|
|           X+o . |
|          * *.   |
|         o oE.   |
|        S o.o    |
|         . o -   |
|            +    |
|                 |
|                 |
+-----------------+
```

Podemos ver como ahora tenermos dos archivos nuevos creados. **github.pub** es nuestro archivo que contiene la parte pública que compartiremos con github y **github** es nuestra clave privada, la cual no debemos compartir con nadie.

A continuación copiamos nuestra clave pública para introducirla en nuestra cuenta de github:

```sh
cat github.pub
```

ssh-rsa AAAAB3Nza2EAAAADAQABAAABAQDbSjrdVdjkfzZM2yfdOI2sOW9fXF5qKPZFGJl21FRbOcFmw+HZ5sa7rJknmDsGQBL+AHnc/kBJIfDD3eN1YY2tFZFu2tin6f2PrY7wovxAKC3nZQaX28v4YPR4OMENBL37wowsvH+XvEQVJYogHoq9O8OAus6rnFJeolWd/0rka8XOHv7KMX+i4k985n40iz60ivoznWIRrJbTySspIyo3qYubEGItJ+4uulZiKH79DNZfquqpF/CFQ4EDBIXXpUOZ4nZJlNaWOWy4djE0chk8VVg7QAknvc/ajQnJOIx2M34zR+0TawTDl/J+0r4IMLykTqwBk6KhzYyPFo/Y5QuX tuemail@domain.com

Ingresamos en nuestra cuenta de github y en *settings* accedemos al apartado situado en el lado izquierdo de la pantalla llamado *SSH and GPG keys*.
Una vez allí pulsamos en el botón situado en el lado superior derecho de la pantalla *New SSH key* y pegamos en la caja de texto de abajo nuestra clave pública de antes. Como título debemos viguilar que no contenga cierto carácteres raros o que sea muy larga porque podría arrojar algunos fallos.

De cara a tener más claves para el futuro es una buena idea crear un archivo de configuración en el que indicar donde se encuentra cada una. 
Para ello creamos un nuevo archivo:

```sh
touch ~/.ssh/config
```

Dentro del archivo introducimos lo siguiente cambiando la ruta del apartado *IdentityFile* por la ruta de nuestra clave privada:

```
Host github.com
HostName github.com
IdentityFile /Users/tuusuario/.ssh/github
IdentitiesOnly yes
```

Ahora cambiamos los permisos de ese archivo, ya que es posible que por defecto tenga permisos de lectura y escritura por más personas que nuestro usuario y por lo tanto nos arrojará un error en el futuro.

```sh
chmod 600 /home/dsc/.ssh/config
```

Para finalizar probamos que todo ha salido bien probando a conectarnos con github haciendo lo siguiente:

```sh
ssh -T git@github.com
```

Para más información de los comandos utilizados en este tutorial se pueden ver los enlaces situados al final de la página.


**Referencias**
1. https://es.wikipedia.org/wiki/Criptograf%C3%ADa_asim%C3%A9trica
2. https://www.ecured.cu/Criptograf%C3%ADa_asim%C3%A9trica
3. http://blog.elhui2.info/2015-01/conexion-a-un-repositorio-github-por-ssh/
4. http://man.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man1/ssh-keygen.1?query=ssh-keygen%26sec=1
5. https://superuser.com/questions/916917/bitbucket-git-ssh-key-error-bad-owner-or-permissions-on-home-centos-ssh-conf
