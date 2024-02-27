# Cómo trabajar con varias cuentas de Github desde mi Mac

Para este caso tendremos 2 cuentas de github: una de mi trabajo y una personal. El mismo procedimiento se puede aplicar para 3 o más cuentas de Github.

Cuentas de Github:
``
https://github.com/may-oficina
https://github.com/may-personal
``

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

-C representa un comentario para ayudar a identificar su clave ssh
-f representa el nombre del archivo donde se guardará su clave ssh

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

* Copiar todo el contenido de la clave pública abriendo cada uno de los archivos ".pub" Ejemplo: 

```
  vim ~/.ssh/github-may-oficina.pub
  vim ~/.ssh/github-may-personal.pub
```

* Pegar el contenido copiado de la clave pública en Github:

- Inicia sesión en cada una de las cuentas de Github (asegúrate de hacerlo en dos navegadores web diferentes)
- Dentro de Github, vaya a Configuración > Claves SSH y GPG > Nueva clave SSH
- Pega el contenido de tu clave pública copiada y asígnale un Título de tu elección.

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
     Host github.com-may-oficina
          HostName github.com
          User git
          IdentityFile ~/.ssh/github-may-oficina

     #may-personal account
     Host github.com-may-personal
          HostName github.com
          User git
          IdentityFile ~/.ssh/github-may-personal
```
