# Cómo trabajar con múltiples cuentas de Github desde mi Mac

Para este caso tendremos 2 cuentas de github: una de mi trabajo y una personal. 

Cuentas de Github:

```
https://github.com/may-oficina

https://github.com/may-personal
```


## Paso 1. Crear las claves SSH para cada una de las cuentas

* Dirigirse al directorio ".ssh"

```
 $ cd ~/.ssh
```

* Digitar el siguiente comando por cada una de las cuentas:

```
ssh-keygen -t rsa -C "your-email-address" -f "github-username"
``` 
donde:

```
-C representa un comentario para ayudar a identificar su clave ssh

-f representa el nombre del archivo donde se guardará su clave ssh
```

Generando clave SSH para cada una de mis cuentas:

```
ssh-keygen -t rsa -C "email_oficina@oficina.com" -f "github-may-oficina"

ssh-keygen -t rsa -C "email_personal@gmail.com" -f "github-may-personal"
```

Nota: Durante la generación de la clave le solicitarán que ingrese una frase para la contraseña, puede dejarla vacía y continuar.

Finalmente aparecerá el mensaje de confirmación de generación de la clave SSH:

````
Your identification has been saved in github-may-personal
Your public key has been saved in github-may-personal.pub
The key fingerprint is:
SHA256:6TxudHv8E4L6Kuw1P8ouABoLn9fT5nLLq3TYc9QARDy email_personal@gmail.com
The key's randomart image is:
+---[RSA 3072]----+
|            B.   |
|           O .   |
|            + .  |
|       . o . o . |
|        B = O O  |
|         o + oo=.|
|      .  bO +O*+O|
|       o.o-*.+=O*|
|      .. +++++oB*|
+----[SHA256]-----+
````

Asegúrese de haber generado la clave SSH para las dos cuentas de Github. Digitando el siguiente comando:

```
ls -l

-rw-------  1 test  staff  2610 Feb 26 20:21 github-may-personal
-rw-r--r--  1 test  staff   575 Feb 26 20:21 github-may-personal.pub
-rw-------  1 test  staff  2622 Feb 26 20:34 github-may-work
-rw-r--r--  1 test  staff   586 Feb 26 20:34 github-may-work.pub
```



## Paso 2. Agregar claves SSH al Agente SSH

Digite el siguiente comando:

```
 ssh-add -K ~/.ssh/github-may-oficina
 ssh-add -K ~/.ssh/github-may-personal
```

Mensaje de confirmación - claves agregadas al agente SSH:

```
Identity added: github-may-personal (email_personal@gmail.com)
```



## Paso 3. Agregar la clave pública SSH a Github

Debemos agregar nuestra clave pública (que generamos en el paso 1) y agregarla a cada una de las cuentas de github correspondientes.

* Copiar todo el contenido de la clave pública abriendo cada uno de los archivos ".pub": 

```
  vim ~/.ssh/github-may-oficina.pub
  vim ~/.ssh/github-may-personal.pub
```

* Pegar el contenido copiado de la clave pública en Github:

```
1. Inicia sesión en cada una de las cuentas de Github (asegúrate de hacerlo en dos navegadores web diferentes)
2. Dentro de Github, vaya a Configuración > Claves SSH y GPG > Nueva clave SSH
3. Pega el contenido de tu clave pública copiada y asígnale un Título de tu elección.

```

Nota 1: Asegúrate de realizar este mismo procedimiento en tus dos cuentas de Github.

Nota 2: Asegúrate de haber copiado el contenido de tu CLAVE PÚBLICA únicamente.


## Paso 4. Crear un archivo de configuración y registrar las entradas de host

* Asegúrate de estar ubicada en el directorio: ~/.ssh

```
 $ cd ~/.ssh
```

* Si el archivo de configuración aún no existe, cree uno con el siguiente comando:

```
 touch config
```

* Agregue estas líneas al archivo de configuración (cada bloque corresponde a cada cuenta de Github):

```
     #may-oficina account
     Host github-may-oficina
          HostName github.com
          User git
          IdentityFile ~/.ssh/github-may-oficina

     #may-personal account
     Host github-may-personal
          HostName github.com
          User git
          IdentityFile ~/.ssh/github-may-personal
```



## Paso 5. Clonar repositorios de GitHub

Clonaremos un repositorio usando una de las cuentas Github que hemos agregado.

* Desde la consola crea un nuevo directorio:

```
 mkdir REPOSITORIOS_TRABAJO
```

* Nos ubicamos en el directorio creado:

```
cd REPOSITORIOS_TRABAJO
```

* Clonamos un repositorio perteneciente a la organización/cuenta de Github de nuestro trabajo. Ejemplo:

Nombre repositorio: "TestRepo"
Cuenta/Organización de Github de nuestro trabajo: "SATORIOS"

Digitamos el siguiente comando:

```
 git clone git@{host-del-archivo-config}:{propietario-cuenta}/{the-repo-name}.git

 [e.g.] git clone git@github-may-oficina:SATORIOS/TestRepo.git
```

* De ahora en adelante, para garantizar que nuestras confirmaciones y envíos desde cada repositorio del sistema utilicen el usuario de GitHub correcto, tendremos que configurar nombre de usuario y correo en cada repositorio recién clonado.

Para hacer esto use los siguientes comandos dentro del directorio de cada repositorio clonado.

```
     git config user.email "email_oficina@oficina.com"
     git config user.name "May"
```

* Para pull o push debemos agregar el origen remoto al proyecto.

```
git remote add origin git@github-may-oficina:SATORIOS/TestRepo.git
```

* Para validar que se ha agregado el repositorio remoto:

```
 git remote -v
```


````
Nota: Si le ha resultado útil considere dejar su estrellita!!!
````


