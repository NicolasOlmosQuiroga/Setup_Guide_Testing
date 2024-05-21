# Manual de Instalación
Se recomienda a los usuarios de Mac M1 o M2, que esta instalacion se haga en un terminal de Rosetta.

## 1. Homebrew

Primero verifica si tienes _Homebrew_ instalado ejecutando:

```bash
brew -v
```

Si es que no lo tienes instalado, ejecuta:

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 2. Ruby Version Manager

Luego instalermos _Ruby Version Manager_, también conocido como _rvm_. Para eso, ejecuta los siguientes comandos desde la consola:

```bash
brew install gnupg2
gpg --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
curl -sSL https://get.rvm.io | bash -s stable
```

Con el primero comando instalamos GNU Privacy Guard, que es una herramienta de cifrado y firmas digitales. Con el segundo comando instalamos una llave que se utiliza (_automágicamente_) para verificar el paquete a instalar. Con el tercero comando instalamos _rvm_.

Para verificar que _rvm_ se instaló correctamente, reincia tu consola y corre:
```bash
rvm -v
```

## 3. PostgreSQL

Ahora instala PostgreSQL en tu máquina. Para eso, ejecuta el siguiente comando:

```bash
brew install postgresql
```

## 4. Ruby y Rails

Instala Ruby 3.1.3 y Ruby on Rails 7.0.4

```bash
rvm install 3.1.3
gem install rails -v 7.0.4
```

## 5. PostgreSQL: superuser

Ahora lo último antes de iniciar el proyecto! Debemos crear un nuevo `superuser` de PostgreSQL. Para esto primero debemos iniciar un servicio de PostgreSQL, ejecutando el siguiente comando:

```bash
brew services start postgresql
```

Ahora ingresamos a la consola de PostgreSQL:

```bash
psql postgres
```

Luego creamos un nuevo usuario. Para mantener consistencia entre todos y para que nadie tenga problemas relacionados al método de autenticación por default que les viene con la instalación, **vamos a crear un usuario con el mismo nombre que el usuario de tu sistema** (acá tomaremos como ejemplo que mi usuario se llama myuser).

Ahora, es posible que este usuario ya exista. Para verificar esto corre `\du` y revisa si ya está creado. En el caso de que no aparezca, ejecuta:

```bash
CREATE USER myuser WITH SUPERUSER PASSWORD 'mypassword';
```

Ojo con que la contraseña esté entre comillas simples.

Para salir de la consola de postgres corre `\q`.

## 6. Node

Ahora tenemos que instalar Node (un runetime de JavaScript): (las versiones más recientes de RoR necesitan Node).

```bash
brew install node
```

Reinicia tu consola y luego corre

```bash
npm -v
```

## 7. Iniciar Proyecto

Ya tenemos todo para empezar con el proyecto. Para esto, descarga el archivo .zip en canvas, descomprimelo y ahora nos movemos a nuestra carpeta del proyecto con:

```bash
cd Proyecto-Testing-2024-1
```

Desde este punto en adelante, en nuestra consola siempre estaremos en el directorio del proyecto.

Y por último, corremos el siguiente comando que instalará sus dependencias del proyecto:

```bash
bundle install
```

Y por último, corremos el siguiente comando que instalará sus dependencias del proyecto:

```bash
bundle install
```

## 8. .env

Ahora vamos a abrir el proyecto recién creado en nuestro editor favorito. Si tienes VS Code puedes correr `code .`.

Ahora que ya tenemos los archivos del proyecto, necesitamos setear la configuración de la base de datos que ocuparemos. 

Primero crea en la raiz de tu proyecto (directamente adentro de la carpeta `Proyecto-Testing-2024-1`) un archivo llamado `.env`  y adentro escribe las credenciales del usuario que creaste en el paso 7:

```env
# no se usan comillas
DB_USER=myuser
DB_PASSWORD=mypassword
```
Recuerda cambiar myuser y mypassword por tu usuario y contraseña!

## 9. Crear y migrar base de datos

¡Necesitamos una base de datos para el proyecto! El siguiente comando creará una por ti:

```bash
rails db:create
rails db:migrate
```

## 10. Iniciar el servidor

Ahora utilizando tu consola, puedes levantar el servidor correr el siguiente comando, se debería levantar en el localhost puerto ```3000```

```
rails server
```

Usuarios de un sistema operativo distinto a windows pueden necesitar descomentar la linea en el archivo config/puma.rb:

workers ENV.fetch("WEB_CONCURRENCY") { 4 }

Ahora que tienes el servidor corriendo, dirígete a http://localhost:3000/ y debería aparecerte la vista inicial de la pagina.

## 11. Ejecutar tests unitarios y de integracion

Antes de ejecutar los tests correr
```
rails db:migrate RAILS_ENV=test
```

Para ejecutar los tests unitarios y de integracion de MiniTest se puede usar:

```
rails test
```

Lo anterior no incluye los tests de sistema

## 12. Instalar Google Chrome

Para ejecutar los tests de sistema, necesitamos Google Chrome, el cual lo puedes instalar del sitio oficial

Para verificar su instalacion, ejecuta:

```
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --version
```

## 14. Ejecutar tests de sistema

Para ejecutar los tests de rspec que contiene a los de sistema se puede utilizar:

```
rspec
```

¡Ya estas listo!, cualquier duda sobre la instalacion la puedes hacer por las issues del repositorio.
