# Instalação

- [instalação](#installation)
    - [Requerimentos do Servidor](#server-requirements)
    - [Instalando o Laravel](#installing-laravel)
    - [Configuração](#configuration)
- [Configuração do Webserver](#web-server-configuration)
    - [Configuração de Diretório](#directory-configuration)
    - [Url´s Amigaveis](#pretty-urls)

<a name="installation"></a>
## Installation

<a name="server-requirements"></a>
### Requerimentos de Servidor

O framework Laravel tem alguns requisitos de sistema. Todos esses requisitos são atendidos pela máquina virtual Laravel Homestead, então é altamente recomendável que você use Homestead como seu ambiente de desenvolvimento Laravel local.

No entanto, se você não estiver usando Homestead, você precisará certificar-se de que seu servidor atenda aos seguintes requisitos:

<div class="content-list" markdown="1">
- PHP >= 7.3
- BCMath PHP Extension
- Ctype PHP Extension
- Fileinfo PHP Extension
- JSON PHP Extension
- Mbstring PHP Extension
- OpenSSL PHP Extension
- PDO PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension
</div>

<a name="installing-laravel"></a>
### Instalando laravel

Laravel utiliza [Composer](https://getcomposer.org) para gerenciar suas dependências. Portanto, antes de usar o Laravel, certifique-se de ter o Composer instalado em sua máquina.

<a name="via-laravel-installer"></a>
#### Via Laravel Installer

Primeiro, baixe o instalador do Laravel usando o Composer:

    composer global require laravel/installer

Make sure to place Composer's system-wide vendor bin directory in your `$PATH` so the laravel executable can be located by your system. This directory exists in different locations based on your operating system; however, some common locations include:

<div class="content-list" markdown="1">
- macOS: `$HOME/.composer/vendor/bin`
- Windows: `%USERPROFILE%\AppData\Roaming\Composer\vendor\bin`
- GNU / Linux Distributions: `$HOME/.config/composer/vendor/bin` or `$HOME/.composer/vendor/bin`
</div>

You could also find the composer's global installation path by running `composer global about` and looking up from the first line.

Once installed, the `laravel new` command will create a fresh Laravel installation in the directory you specify. For instance, `laravel new blog` will create a directory named `blog` containing a fresh Laravel installation with all of Laravel's dependencies already installed:

    laravel new blog

> {tip} Want to create a Laravel project with login, registration, and more features already built for you? Check out [Laravel Jetstream](https://jetstream.laravel.com).

<a name="via-composer-create-project"></a>
#### Via Composer Create-Project

Alternatively, you may also install Laravel by issuing the Composer `create-project` command in your terminal:

    composer create-project --prefer-dist laravel/laravel blog

<a name="local-development-server"></a>
#### Local Development Server

If you have PHP installed locally and you would like to use PHP's built-in development server to serve your application, you may use the `serve` Artisan command. This command will start a development server at `http://localhost:8000`:

    php artisan serve

More robust local development options are available via [Homestead](/docs/{{version}}/homestead) and [Valet](/docs/{{version}}/valet).

<a name="configuration"></a>
### Configuration

<a name="public-directory"></a>
#### Public Directory

After installing Laravel, you should configure your web server's document / web root to be the `public` directory. The `index.php` in this directory serves as the front controller for all HTTP requests entering your application.

<a name="configuration-files"></a>
#### Configuration Files

All of the configuration files for the Laravel framework are stored in the `config` directory. Each option is documented, so feel free to look through the files and get familiar with the options available to you.

<a name="directory-permissions"></a>
#### Directory Permissions

After installing Laravel, you may need to configure some permissions. Directories within the `storage` and the `bootstrap/cache` directories should be writable by your web server or Laravel will not run. If you are using the [Homestead](/docs/{{version}}/homestead) virtual machine, these permissions should already be set.

<a name="application-key"></a>
#### Application Key

The next thing you should do after installing Laravel is set your application key to a random string. If you installed Laravel via Composer or the Laravel installer, this key has already been set for you by the `php artisan key:generate` command.

Typically, this string should be 32 characters long. The key can be set in the `.env` environment file. If you have not copied the `.env.example` file to a new file named `.env`, you should do that now. **If the application key is not set, your user sessions and other encrypted data will not be secure!**

<a name="additional-configuration"></a>
#### Additional Configuration

Laravel needs almost no other configuration out of the box. You are free to get started developing! However, you may wish to review the `config/app.php` file and its documentation. It contains several options such as `timezone` and `locale` that you may wish to change according to your application.

You may also want to configure a few additional components of Laravel, such as:

<div class="content-list" markdown="1">
- [Cache](/docs/{{version}}/cache#configuration)
- [Database](/docs/{{version}}/database#configuration)
- [Session](/docs/{{version}}/session#configuration)
</div>

<a name="web-server-configuration"></a>
## Web Server Configuration

<a name="directory-configuration"></a>
### Directory Configuration

Laravel should always be served out of the root of the "web directory" configured for your web server. You should not attempt to serve a Laravel application out of a subdirectory of the "web directory". Attempting to do so could expose sensitive files present within your application.

<a name="pretty-urls"></a>
### Pretty URLs

<a name="apache"></a>
#### Apache

Laravel includes a `public/.htaccess` file that is used to provide URLs without the `index.php` front controller in the path. Before serving Laravel with Apache, be sure to enable the `mod_rewrite` module so the `.htaccess` file will be honored by the server.

If the `.htaccess` file that ships with Laravel does not work with your Apache installation, try this alternative:

    Options +FollowSymLinks -Indexes
    RewriteEngine On

    RewriteCond %{HTTP:Authorization} .
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]

    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ index.php [L]

<a name="nginx"></a>
#### Nginx

If you are using Nginx, the following directive in your site configuration will direct all requests to the `index.php` front controller:

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

When using [Homestead](/docs/{{version}}/homestead) or [Valet](/docs/{{version}}/valet), pretty URLs will be automatically configured.
