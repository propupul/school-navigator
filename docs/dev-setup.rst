Development Setup
=================

Follow these steps to contribute to the School Navigator project!


.. _project-architecture:

Project Architecture
--------------------

The project is divided into two components, the frontend and backend:

* :ref:`Frontend <frontend-setup>`: front facing user interface. Built in AngularJS, HTML, and CSS.
* :ref:`Backend <backend-setup>`: backend API that powers the frontend. Built in Python/Django, PostgreSQL, and PostGIS.

Generally, unless you're working on the REST API, you'll develop the
:ref:`Frontend <frontend-setup>`.


.. _clone-the-repository:

Clone the Repository
--------------------

To get started, you'll first need to clone the GitHub repository so you can
work on the project locally. In a terminal, run:

.. code-block:: bash

    git clone git@github.com:codefordurham/school-navigator.git
    cd school-navigator


.. _frontend-setup:

Frontend Setup
--------------

Once you've cloned the project, open the ``frontend`` directory::

    cd frontend/

Next run a basic HTTP server with Python:

.. code-block:: bash

    # Python <= 2.7
    python -m SimpleHTTPServer
    # Python >= 3.0
    python3 -m http.server

Now visit http://localhost:8000/ in your browser.


.. _backend-setup:

Backend Setup
-------------

Below you will find basic setup instructions for the school_inspector
project. To begin you should have the following applications installed on your
local development system:

- Python >= 3.4 (3.4 recommended)
- `pip >= 1.5 <http://www.pip-installer.org/>`_
- `virtualenv >= 1.11 <http://www.virtualenv.org/>`_
- `virtualenvwrapper >= 3.0 <http://pypi.python.org/pypi/virtualenvwrapper>`_
- Postgres >= 9.1
- git >= 1.7

The deployment uses SSH with agent forwarding so you'll need to enable agent
forwarding if it is not already by adding ``ForwardAgent yes`` to your SSH config.


Getting Started
~~~~~~~~~~~~~~~
*Note*: The following instructions use the apt package manager for Debian/Ubuntu
Linux. If apt is not available for your system, use your preferred package manager
(i.e. `Homebrew <http://brew.sh>`_ for Mac OS X) to install the required dependencies.

If you need Python 3.4 installed, you can use this PPA::

    sudo add-apt-repository ppa:fkrull/deadsnakes
    sudo apt-get update
    sudo apt-get install python3.4-dev

The tool that we use to deploy code is called `Fabric
<http://docs.fabfile.org/>`_, which is not yet Python3 compatible. So,
we need to install that globally in our Python2 environment::

    sudo pip install fabric==1.8.1

To setup your local environment you should create a virtualenv and install the
necessary requirements::

    # Check that you have python3.4 installed
    $ which python3.4
    $ mkvirtualenv school_navigator -p `which python3.4`
    (school_navigator)$ $VIRTUAL_ENV/bin/pip install -r $PWD/requirements/dev.txt

Then create a local settings file and set your ``DJANGO_SETTINGS_MODULE`` to use it::

    (school_navigator)$ cp school_navigator/settings/local.example.py school_navigator/settings/local.py
    (school_navigator)$ echo "DJANGO_SETTINGS_MODULE=school_navigator.settings.local" > .env

Exit the virtualenv and reactivate it to activate the settings just changed::

    deactivate
    workon school_inspector

If you're on Ubuntu 12.04, to get get postgis you need to set up a few more
packages before you can create the db and set up the postgis extension::

   sudo apt-add-repository ppa:ubuntugis/ppa
   sudo aptitude update && sudo aptitude install postgis postgresql-9.1-postgis-2.0 postgresql-9.1-postgis-2.0-scripts

Now, create the Postgres database and run the initial syncdb/migrate::

    (school_navigator)$ createdb -E UTF-8 school_navigator
    (school_navigator)$ python manage.py migrate

You should now be able to run the development server::

    python manage.py runserver 8001

If you're serving the frontend locally (via `frontend-setup`_) in a separate
terminal, append ``?env=dev/`` to your browser URI to make the frontend hit the
locally running API on port 8001: http://localhost:8000/?env=dev/
