# Manual de Instalación

## 1. Instalar WSL

**¿Estás usando Windows?** 

Para que no tengas problemas en este y próximos cursos deberás descargar [WSL o WSL2](https://es.wikipedia.org/wiki/Windows_Subsystem_for_Linux) (Windows Subsystem for Linux). Para esto existen diferentes guías:
* [Guía oficial](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
* [Tutorial de Youtube](https://www.youtube.com/watch?v=n-J9438Mv-s)

La diferencia entre WSL y WSL2 es la compatibilidad con la versión de Windows.

Luego, les recomendamos que instalen, desde Microsoft Store, algún lanzamiento de Ubuntu desde la versión 18.04 para utilizarlo como consola, [esta por ejemplo](https://www.microsoft.com/store/productId/9N6SVWS3RX71).

De ahora en adelante, al referirnos a "la consola" o "el terminal", aquellos utilizando Windows deben utilizar la consola de Ubuntu.

## 2. Dependencias

Instalar las dependencias necesarias. Para eso, desde la consola, se deben ejecutar los siguientes comandos:

```bash
sudo apt-get update
sudo apt-get install build-essential patch liblzma-dev libpq-dev
```

## 3. Ruby Version Manager

Instalar _Ruby Version Manager_, también conocido como _rvm_. Para eso, ejecutar los siguientes comandos desde la consola:

```bash
sudo apt install gnupg2
gpg --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
curl -sSL https://get.rvm.io | bash -s stable
```

Con el primero comando instalamos GNU Privacy Guard, que es una herramienta de cifrado y firmas digitales. Con el segundo comando instalamos una llave que se utiliza (_automágicamente_) para verificar el paquete a instalar. Con el segundo comando instalamos _rvm_.

Para verificar que _rvm_ se instaló correctamente, **reinicia tu consola** y corre:
```bash
rvm -v
```

## 4. PostgresSQL

Instalar PostgreSQL en tu máquina. Para eso, ejecutaremos el siguiente comando:

```bash
sudo apt-get install postgresql postgresql-contrib
```

## 5. Ruby y Rails

Instalar Ruby 3.1.3 e instalar Ruby on Rails 7.0.4

```bash
rvm install 3.1.3
gem install rails -v 7.0.4
```

## 6. Node

Ahora tenemos que instalar Node (un [_runetime_](https://en.wikipedia.org/wiki/Runtime_library) de JavaScript), para lo que utilizaremos _Node Version Manager_ o _nvm_ (las versiones más recientes de RoR necesitan Node).

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
```

Reinicia tu consola y luego corre

```bash
nvm install --lts
```


## 7. PostgreSQL: superuser

Ahora lo último antes de iniciar el proyecto! Debemos crear un nuevo `superuser` de PostgreSQL. Para esto primero debemos iniciar un servicio de PostgreSQL, ejecutando el siguiente comando:

```bash
sudo service postgresql start
```

Ahora ingresamos a la consola de PostgreSQL como usuario `postgres` (que viene creado junto con la instalación):

```bash
sudo -u postgres psql
```

Luego, creamos un nuevo usuario. Para mantener consistencia entre todos y para que nadie tenga problemas relacionados al método de autenticación por default que les viene con la instalación, **vamos a crear un usuario con el mismo nombre que el usuario de tu sistema** (acá tomaremos como ejemplo que mi usuario se llama myuser):

```bash
CREATE USER myuser WITH SUPERUSER PASSWORD 'mypassword';
```

Ojo con que la contraseña esté entre comillas simples.

Para salir de la consola de postgres corre `\q`.

## 8. Iniciar Proyecto

Ya tenemos todo para empezar con el proyecto. Para esto, descarga el archivo .zip en canvas, muevelo a tu directorio en Ubuntu, puedes hacerlo facilmente yendo a la consola y colocando el comando `explorer.exe .` y moviendo la carpeta dentro del directorio con tu usuario, ahora nos movemos a nuestra carpeta del proyecto con:

```bash
cd Proyecto-Testing-2024-1
```

Desde este punto en adelante, en nuestra consola siempre estaremos en el directorio del proyecto.

Si aparecen archivos del tipo Zone.Identifier, los puedes eliminar de la siguiente manera:

```
find . -name "*:Zone.Identifier" -type f -delete
```

Y por último, corremos el siguiente comando que instalará sus dependencias del proyecto:

```bash
bundle install
```

## 9. .env

Ahora vamos a abrir el proyecto recién creado en nuestro editor favorito. Si tienes VS Code puedes correr `code .`.

Ahora que ya tenemos los archivos del proyecto, necesitamos setear la configuración de la base de datos que ocuparemos. 

Primero crea en la raiz de tu proyecto (directamente adentro de la carpeta `Proyecto-Testing-2024-1`) un archivo llamado `.env`  y adentro escribe las credenciales del usuario que creaste en el paso 7:

```env
# no se usan comillas
DB_USER=myuser
DB_PASSWORD=mypassword
```

¡Recuerda cambiar myuser y mypassword por tu usuario y contraseña!

## 10. Crear y migrar base de datos

¡Necesitamos una base de datos para el proyecto! El siguiente comando creará una por ti:

```bash
rails db:create
rails db:migrate
```

## 11. Iniciar el servidor

Ahora utilizando tu consola, puedes levantar el servidor correr el siguiente comando, se debería levantar en el localhost puerto ```3000```

```
rails server
```

Usuarios de un sistema operativo distinto a windows pueden necesitar descomentar la linea en el archivo config/puma.rb:

workers ENV.fetch("WEB_CONCURRENCY") { 4 }

Ahora que tienes el servidor corriendo, dirígete a http://localhost:3000/ y debería aparecerte la vista inicial de la pagina.

## 12. Ejecutar tests unitarios y de integracion

Antes de ejecutar los tests correr
```
rails db:migrate RAILS_ENV=test
```

Para ejecutar los tests unitarios y de integracion de MiniTest se puede usar:

```
rails test
```

Lo anterior no incluye los tests de sistema

## 13. Instalar Google Chrome

Para ejecutar los tests de sistema, necesitamos un navegador, empezaremos instalando Google Chrome:
```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install ./google-chrome-stable_current_amd64.deb
```

Para verificar su instalacion, ejecuta:

```
google-chrome --version
```

## 14. Ejecutar tests de sistema

Para ejecutar los tests de rspec que contiene a los de sistema se puede utilizar:

```
rspec
```

¡Ya estas listo!, cualquier duda sobre la instalacion la puedes hacer por las issues del repositorio.
