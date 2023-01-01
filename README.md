About epc
=========

Home: https://github.com/tkf/python-epc

Package license: GPL3

Feedstock license: BSD 3-Clause

Summary: EPC (RPC stack for Emacs Lisp) implementation in Python

EPC (RPC stack for Emacs Lisp) for Python
=========================================

Links:

* `Documentation <http://python-epc.readthedocs.org/>`_ (at Read the Docs)
* `Repository <https://github.com/tkf/python-epc>`_ (at GitHub)
* `Issue tracker <https://github.com/tkf/python-epc/issues>`_ (at GitHub)
* `PyPI <http://pypi.python.org/pypi/epc>`_
* `Travis CI <https://travis-ci.org/#!/tkf/python-epc>`_ |build-status|

Other resources:

* `kiwanami/emacs-epc <https://github.com/kiwanami/emacs-epc>`_
  (Client and server implementation in Emacs Lisp and Perl.)
* `tkf/emacs-jedi <https://github.com/tkf/emacs-jedi>`_
  (Python completion for Emacs using EPC server.)

.. |build-status|
   image:: https://secure.travis-ci.org/tkf/python-epc.png
           ?branch=master
   :target: http://travis-ci.org/tkf/python-epc
   :alt: Build Status


What is this?
-------------

EPC is an RPC stack for Emacs Lisp and Python-EPC is its server side
and client side implementation in Python.  Using Python-EPC, you can
easily call Emacs Lisp functions from Python and Python functions from
Emacs.  For example, you can use Python GUI module to build widgets
for Emacs (see `examples/gtk/server.py`_ for example).

Python-EPC is tested against Python 2.6, 2.7 and 3.2.

Install
-------

To install Python-EPC and its dependency sexpdata_, run the following
command.::

   pip install epc

.. _sexpdata: https://github.com/tkf/sexpdata


Usage
-----

Save the following code as ``my-server.py``.
(You can find functionally the same code in `examples/echo/server.py`_)::

   from epc.server import EPCServer

   server = EPCServer(('localhost', 0))

   @server.register_function
   def echo(*a):
       return a

   server.print_port()
   server.serve_forever()


And then run the following code from Emacs.
This is a stripped version of `examples/echo/client.el`_ included in
Python-EPC repository_.::

   (require 'epc)

   (defvar my-epc (epc:start-epc "python" '("my-server.py")))

   (deferred:$
     (epc:call-deferred my-epc 'echo '(10))
     (deferred:nextc it
       (lambda (x) (message "Return : %S" x))))

   (message "Return : %S" (epc:call-sync my-epc 'echo '(10 40)))


.. _examples/echo/server.py:
   https://github.com/tkf/python-epc/blob/master/examples/echo/server.py
.. _examples/echo/client.el:
   https://github.com/tkf/python-epc/blob/master/examples/echo/client.el

If you have carton_ installed, you can run the above sample by
simply typing the following commands::

   make elpa        # install EPC in a separated environment
   make run-sample  # run examples/echo/client.el

.. _carton: https://github.com/rejeep/carton


For example of bidirectional communication and integration with GTK,
see `examples/gtk/server.py`_.  You can run this example by::

   make elpa
   make run-gtk-sample  # run examples/gtk/client.el

.. _examples/gtk/server.py:
   https://github.com/tkf/python-epc/blob/master/examples/gtk/server.py


License
-------

Python-EPC is licensed under GPL v3.
See COPYING for details.


Current build status
====================

All platforms:
[![noarch](https://img.shields.io/circleci/project/github/conda-forge/epc-feedstock/master.svg?label=noarch)](https://circleci.com/gh/conda-forge/epc-feedstock)

Current release info
====================

| Name | Downloads | Version | Platforms |
| --- | --- | --- | --- |
| [![Conda Recipe](https://img.shields.io/badge/recipe-epc-green.svg)](https://anaconda.org/conda-forge/epc) | [![Conda Downloads](https://img.shields.io/conda/dn/conda-forge/epc.svg)](https://anaconda.org/conda-forge/epc) | [![Conda Version](https://img.shields.io/conda/vn/conda-forge/epc.svg)](https://anaconda.org/conda-forge/epc) | [![Conda Platforms](https://img.shields.io/conda/pn/conda-forge/epc.svg)](https://anaconda.org/conda-forge/epc) |

Installing epc
==============

Installing `epc` from the `conda-forge` channel can be achieved by adding `conda-forge` to your channels with:

```
conda config --add channels conda-forge
```

Once the `conda-forge` channel has been enabled, `epc` can be installed with:

```
conda install epc
```

It is possible to list all of the versions of `epc` available on your platform with:

```
conda search epc --channel conda-forge
```


About conda-forge
=================

conda-forge is a community-led conda channel of installable packages.
In order to provide high-quality builds, the process has been automated into the
conda-forge GitHub organization. The conda-forge organization contains one repository
for each of the installable packages. Such a repository is known as a *feedstock*.

A feedstock is made up of a conda recipe (the instructions on what and how to build
the package) and the necessary configurations for automatic building using freely
available continuous integration services. Thanks to the awesome service provided by
[CircleCI](https://circleci.com/), [AppVeyor](https://www.appveyor.com/)
and [TravisCI](https://travis-ci.org/) it is possible to build and upload installable
packages to the [conda-forge](https://anaconda.org/conda-forge)
[Anaconda-Cloud](https://anaconda.org/) channel for Linux, Windows and OSX respectively.

To manage the continuous integration and simplify feedstock maintenance
[conda-smithy](https://github.com/conda-forge/conda-smithy) has been developed.
Using the ``conda-forge.yml`` within this repository, it is possible to re-render all of
this feedstock's supporting files (e.g. the CI configuration files) with ``conda smithy rerender``.

For more information please check the [conda-forge documentation](https://conda-forge.org/docs/).

Terminology
===========

**feedstock** - the conda recipe (raw material), supporting scripts and CI configuration.

**conda-smithy** - the tool which helps orchestrate the feedstock.
                   Its primary use is in the construction of the CI ``.yml`` files
                   and simplify the management of *many* feedstocks.

**conda-forge** - the place where the feedstock and smithy live and work to
                  produce the finished article (built conda distributions)


Updating epc-feedstock
======================

If you would like to improve the epc recipe or build a new
package version, please fork this repository and submit a PR. Upon submission,
your changes will be run on the appropriate platforms to give the reviewer an
opportunity to confirm that the changes result in a successful build. Once
merged, the recipe will be re-built and uploaded automatically to the
`conda-forge` channel, whereupon the built conda packages will be available for
everybody to install and use from the `conda-forge` channel.
Note that all branches in the conda-forge/epc-feedstock are
immediately built and any created packages are uploaded, so PRs should be based
on branches in forks and branches in the main repository should only be used to
build distinct package versions.

In order to produce a uniquely identifiable distribution:
 * If the version of a package **is not** being increased, please add or increase
   the [``build/number``](https://conda.io/docs/user-guide/tasks/build-packages/define-metadata.html#build-number-and-string).
 * If the version of a package **is** being increased, please remember to return
   the [``build/number``](https://conda.io/docs/user-guide/tasks/build-packages/define-metadata.html#build-number-and-string)
   back to 0.
test
