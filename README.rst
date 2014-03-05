Project Templates
=================

.. contents::


Introduction
------------

Project layout templates for various types of python projects using on the Open
Source PythonPro librarys for speedy development bootstrapping. This uses the
MrBob project behing the scenes.

 * http://mrbob.readthedocs.org/en/latest/


Set up
------

You can pip install the latest code version::

  pip install http://github.com/pythonpro/project-templates/tarball/master#egg=project-templates


Pyramid REST Service
--------------------

The first template type is a Pyramid REST Service template which creates a
backend model, service and client library. The layout is based on PythonPro's
standard.

In the following example I'm going to create a ficticious email service. I will
choose the python namespace "widgetco.email" which will create the top level
project name widgetco-email

Create
~~~~~~

To create the project we use the following command line::

    $ mrbob project.templates:restservice

    Welcome to mr.bob interactive mode. Before we generate directory structure, some questions need to be answered.

    Answer with a question mark to display help.
    Values in square brackets at the end of the questions show the default value if there is no answer.


    --> Top level namespace e.g. 'myproject': widgetco

    --> Python package below project e.g. user would result in namespace project.user.*: email


    Pre Render Variables Out: {'build_root': 'widgetco-email',
     'capital_name': u'WidgetcoEmail',
     'created': '2014-03-05T15:31:32',
     'egg': 'widgetco-email',
     'name': 'widgetco.email',
     'namespace': 'widgetco.email',
     'namespace_packages': ['widgetco'],
     'namespaced_package': 'widgetco.email',
     'package': u'email',
     'project': 'widgetco-email',
     'random_port': 60614,
     'random_string': '5c4fc26bb9dbe51bfcddab3281247bf260bce776',
     'root': '/home/vagrant/src',
     'tcp_port': '60614',
     'top_namespace': u'widgetco'}
    :
    lots of output
    :
    $


Set up
~~~~~~

Setup the project in development mode, pulling down dependancies::

    $ cd widgetco-email
    $ python setup.py develop
    ---> pavement.develop
    Using python: </home/vagrant/.virtualenvs/api/bin/python>
    -- Changing to /home/vagrant/src/widgetco-email/model --
    -- Setting up /home/vagrant/src/widgetco-email/model in development mode --
    Setting up '/home/vagrant/src/widgetco-email/model': (BASKET:'')
    /home/vagrant/.virtualenvs/api/bin/python setup.py develop
    :
    lots and lots of output
    :
    Using /home/vagrant/.virtualenvs/api/lib/python2.7/site-packages/Pygments-1.6-py2.7.egg
    Searching for MarkupSafe==0.18
    Best match: MarkupSafe 0.18
    Processing MarkupSafe-0.18-py2.7-linux-x86_64.egg
    MarkupSafe 0.18 is already the active version in easy-install.pth

    Using /home/vagrant/.virtualenvs/api/lib/python2.7/site-packages/MarkupSafe-0.18-py2.7-linux-x86_64.egg
    Finished processing dependencies for widgetco-email-client==1.0.0dev
    $

Success. The top level directory contains the pavement.py file. This emulates
a normal call to "python setup.py develop". It changes to each of the packages
in the project and sets them up.


Quick Test Check
~~~~~~~~~~~~~~~~

The rendered template has unit and acceptance tests. These are done using py.test
and allow the end user to build on them. The acceptance tests run the service
and use the python client library to exercise the generated interface.

To run the tests do::

    $ py.test
    ========================================================================== test session starts ==========================================================================
    platform linux2 -- Python 2.7.3 -- py-1.4.20 -- pytest-2.5.2
    plugins: pkglib, cov
    collected 2 items

    model/widgetco/email/model/tests/test_model.py .
    service/widgetco/email/service/tests/test_serverapi.py .

    ======================================================================= 2 passed in 20.66 seconds =======================================================================
    $


Run The Service
~~~~~~~~~~~~~~~

To run the newly created REST service you can do::

    $ pserve service/development.ini
    Starting server in PID 23047.
    serving on 0.0.0.0:60614 view at http://127.0.0.1:60614

The template process chooses a random port as the default port for the service.
This can be changed in the configuration.


Curl to Ping
~~~~~~~~~~~~

Once the service is running you can "ping" the service. From another command
line do::

    $ curl -qs http://127.0.0.1:60614/
    {"status": "ok", "version": "1.0.0dev", "name": "widgetco-email-service"}


Notes
~~~~~

This template delivers service, client, etc into one repository. The versions
of the eggs are controlled from "eggs_version.ini" and not directly from
"setup.py".

This uses Paver_ to make the project appear as one 'package' from the top. Paver
is like Fabric_, however it allows you to extend distutils to provide custom
commands. It also doesn't need to be installed to run. This project use this
feature to get it to work out-of-the-box. The "pavement.py" is the equivalent
of the fabfile. Paver's command line handling is better and it allows task
dependancies. I like the remote access in Fabric_.


.. _namespace: http://packages.python.org/distribute/setuptools.html#namespace-packages
.. _templating: http://collective-docs.readthedocs.org/en/latest/misc/paster_templates.html
.. _Paver: http://paver.github.com/paver/
.. _Fabric: http://docs.fabfile.org/en/1.4.3/index.html