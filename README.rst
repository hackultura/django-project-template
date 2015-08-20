Hackultura Project Template
===========================

.. image:: https://requires.io/github/hackultura/django-project-template/requirements.svg?branch=master
     :target: https://requires.io/github/hackultura/django-project-template/requirements/?branch=master
     :alt: Requirements Status

.. image:: https://travis-ci.org/hackultura/django-project-template.svg?branch=master
    :target: https://travis-ci.org/hackultura/django-project-template
    :alt: Build Status

.. image:: https://badges.gitter.im/Join Chat.svg
   :target: https://gitter.im/hackultura/django-project-template?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge


Template de projetos do Hackultura, usando cookiecutter_.

.. _cookiecutter: https://github.com/audreyr/cookiecutter

Características
---------------

* Django 1.8
* Renderização dos projetos django com 100% de cobertura de testes
* Twitter Bootstrap_ 3
* Testes End-to-end via Hitch_
* AngularJS_
* 12-Factor_ usando django-environ_
* Configurações otimizadas para ambiente de desenvolvimento e produção
* Registro de usuários via django-allauth_
* Compilação para compass e livereload usando Grunt (breve com Gulp)
* Pre configured Celery_ (optional)
* Suporte a docker usando docker-compose_ para desenvolvimento e produção

.. _Hitch: https://github.com/hitchtest/hitchtest
.. _Bootstrap: https://github.com/twbs/bootstrap
.. _AngularJS: https://github.com/angular/angular.js
.. _django-environ: https://github.com/joke2k/django-environ
.. _12-Factor: http://12factor.net/
.. _django-allauth: https://github.com/pennersr/django-allauth
.. _django-avatar: https://github.com/jezdez/django-avatar/
.. _Celery: http://www.celeryproject.org/
.. _docker-compose: https://www.github.com/docker/compose


Constraints
-----------

* Tudo com PostgreSQL (9.0+)
* Variável de ambiente para configuração (Não funciona com Apache/mod_wsgi).


Instalação
----------

Vamos supor que deseja criar um projeto chamado de ``hackultura``.

Primeiro, instale o pacote:

    $ pip install cookiecutter

Agora, execute esse comando para gerar o seu projeto::

    $ cookiecutter https://github.com/pydanny/cookiecutter-django.git

Você será redirecionado para responder algumas perguntas sobre o seu projeto.

Segue um exemplo::

    Cloning into 'cookiecutter-django'...
    remote: Counting objects: 550, done.
    remote: Compressing objects: 100% (310/310), done.
    remote: Total 550 (delta 283), reused 479 (delta 222)
    Receiving objects: 100% (550/550), 127.66 KiB | 58 KiB/s, done.
    Resolving deltas: 100% (283/283), done.
    project_name (default is "project_name")? Reddit Clone
    repo_name (default is "Reddit_Clone")? reddit
    author_name (default is "Your Name")? Daniel Greenfeld
    email (default is "Your email")? pydanny@gmail.com
    description (default is "A short description of the project.")? A reddit clone.
    domain_name (default is "example.com")? myreddit.com
    version (default is "0.1.0")? 0.0.1
    timezone (default is "UTC")?
    use_celery (default is "y")?


Entre no projeto::

    $ cd hackultura/
    $ ls

Crie um repositório github e envie essa estrutura::

    $ git init
    $ git add .
    $ git commit -m "first awesome commit"
    $ git remote add origin git@github.com:hackultura/hackultura.git
    $ git push -u origin master

Getting up and running
----------------------

Os passos abaixo serve para configurar e levantar o projeto localmente, no ambiente de desenvolvimento. Vamos assumir, que possui
as seguintes ferramentas abaixo instaladas:

* pip
* virtualenv
* PostgreSQL

Primeiro, vamos criar e ativar o seu virtualenv_, e com o terminal aberto na raiz do projeto, instale as dependêncidas no ambiente::

    $ pip install -r requirements/local.txt

.. _virtualenv: http://docs.python-guide.org/en/latest/dev/virtualenvs/

Agora, crie o banco no PostgreSQL e adiciona suas configurações usando o padrão do ``dj-database-url``:``postgres://db_owner:password@dbserver_ip:port/db_name`` ou:

* na arquivo de configuraçao ``config.settings.common.py``,
* ou na variável de ambiente ``DATABASE_URL``


Você agora pode executar os comandos padrão::

    $ python manage.py migrate

    $ python manage.py runserver


**Live reloading e compilação do SASS**

If you'd like to take advantage of live reloading and Sass / Compass CSS compilation you can do so with the included Grunt task.

Com o nodejs_ instalado, execute o comando para instalar as dependências::

    $ npm install

.. _nodejs: http://nodejs.org/download/

Agora você precisa rodar::

    $ grunt serve

O projeto agora poderá rodar junto com o usual ``manage.py runserver``, mas com live reloading e compilação Sass habilitado.

To get live reloading to work you'll probably need to install an `appropriate browser extension`_

.. _appropriate browser extension: http://feedback.livereload.com/knowledgebase/articles/86242-how-do-i-install-and-use-the-browser-extensions-

It's time to write the code!!!

Getting up and running using docker
----------------------------------

The steps below will get you up and running with a local development environment. We assume you have the following installed:

* docker
* docker-compose

Open a terminal at the project root and run the following for local development::

    $ docker-compose -f dev.yml up

You can also set the environment variable ``COMPOSE_FILE`` pointing to ``dev.yml`` like this::

    $ export COMPOSE_FILE=dev.yml

And then run::

    $ docker-compose up


To migrate your app and to create a superuser, run::

    $ docker-compose run django python manage.py migrate

    $ docker-compose run django python manage.py createsuperuser


If you are using `boot2docker` to develop on OS X or Windows, you need to create a `/data` partition inside your boot2docker
vm to make all changes persistent. If you don't do that your `/data` directory will get wiped out on every reboot.

To create a persistent folder, log into the `boot2docker` vm by running::

    $ bootdocker ssh

And then::

    $ sudo su
    $ echo 'ln -sfn /mnt/sda1/data /data' >> /var/lib/boot2docker/bootlocal.sh

In case you are wondering why you can't use a host volume to keep the files on your mac: As of `boot2docker` 1.7 you'll
run into permission problems with mounted host volumes if the container creates his own user and `chown`s the directories
on the volume. Postgres is doing that, so we need this quick fix to ensure that all development data persists.
