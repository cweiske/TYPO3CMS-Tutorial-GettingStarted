.. include:: /Includes.rst.txt

.. index:: installation, deployment, requirements

.. _install:

================
Installing TYPO3
================

Welcome to the TYPO3 installation guide. This guide covers each of the steps required 
to install TYPO3 using the dependency manager Composer.

Installation and deployment
---------------------------

It is recommended to not use Composer on a productive system to update and install 
additional packages. Such changes should be made on a development system (locally in
DDEV, on a non-productive part of the server, ...)

This way it can be prevented to install unwanted versions of packages. The files 
:file:`composer.json` and :file:`composer.lock` can be kept under version controll
and specify exactly what versions should be installed.

Deployment to the productive system can be achieved by deploying these two 
:file:`composer.*` files and calling 

.. code-block:: sh
   
   composer install --no-dev
 
.. warning:: 
   Always use the :sh:`--no-dev` parameter to prevent installing packages marked as
   "development only" to be deployed on the productive server.
   
Or by executing the above command on a development system or in a Docker container 
in a deployment pipeline and then just transfer the following folders:

.. code-block:: none

   $ TYPO3 root directory
   ├── webroot/typo3
   ├── webroot/typo3conf
   └── vendor

Pre-installation Checklist
--------------------------

- Command line (CLI) access with the ability to create directories and symbolic links.
- Access to `Composer <https://getcomposer.org>`__ via the CLI (for local development)
- Access to the web server's root directory
- Database with appropriate credentials

Execute Composer Create-Project
-------------------------------

.. tabs::

   .. group-tab:: bash

       .. code-block:: bash

         composer create-project typo3/cms-base-distribution:^11 example-project-directory

   .. group-tab:: powershell

       .. code-block:: powershell

         composer create-project "typo3/cms-base-distribution:^11" example-project-directory

   .. group-tab:: ddev

       .. code-block:: bash

           # Create a directory for your project
           mkdir example-project-directory

           # Go into that directory
           cd example-project-directory

           # Tell DDEV to create a new project of type "typo3"
           ddev config  --project-type=typo3 --docroot=public --create-docroot

           # Start the ddev instance
           ddev start

           # Fetch a basic TYPO3 installation and its' dependencies
           ddev composer create "typo3/cms-base-distribution:^11"


This command pulls down the latest release of TYPO3 and places it in the :file:`example-project-directory`.

After this command has finished running, :file:`example-project-directory` will contain the following structure:

.. code-block:: none

    .
    ├── .gitignore
    ├── composer.json
    ├── composer.lock
    ├── LICENSE
    ├── public
    ├── README.md
    ├── var
    └── vendor


Verify Installation
-------------------

Create an empty file called `FIRST_INSTALL` in the `/public` directory:

.. tabs::

   .. group-tab:: bash

       .. code-block:: bash

         touch example-project-directory/public/FIRST_INSTALL

   .. group-tab:: powershell

       .. code-block:: powershell

         echo $null >> public/FIRST_INSTALL

   .. group-tab:: ddev

       .. code-block:: bash

         ddev exec touch public/FIRST_INSTALL

.. code-block:: none

    .
    ├── .gitignore
    ├── composer.json
    ├── composer.lock
    ├── LICENSE
    ├── public
        ├── FIRST_INSTALL
    ├── README.md
    ├── var
    └── vendor

Access TYPO3 via a web browser
------------------------------

TYPO3 can now be accessed via a web browser. When accessing a new site for the first time, TYPO3 automatically
redirects all requests to `/typo3/install.php` to complete the installation process.

.. tip::

   When accessing the page via HTTPS, a "Privacy error" or similar warning is likely to occur.
   In a local environment it is safe to ignore this warning by forcing the browser to ignore similar
   exceptions for this domain.

   The warning is due to the fact that self-signed certificates are being used.

   If there is a `trustedHostsPattern` error on initial access, accessing TYPO3 without HTTPS (`http://`) is also an option.


Scan Environment
----------------

TYPO3 will now scan the host environment. During the scan TYPO3 will check the host system for the following:

- Minimum required version of PHP is installed.
- Required PHP extensions are loaded.
- php.ini is configured.
- TYPO3 is able to create and delete files and directories within the installation's root directory.

If no issues are detected, the installation process can continue.

In the event that certain criteria are not met, TYPO3 will display a list of issues it has detected accompanied by a resolution for each issue.

Once changes have been made, TYPO3 can re-scan the host environment by reloading the page `https://example-project-site.local/typo3/install.php`.

.. include:: ../Images/AutomaticScreenshots/QuickInstall/Step1SystemEnvironment.rst.txt

Select A Database
-----------------

Select a database connection driver and enter the credentials for the database.

.. include:: ../Images/AutomaticScreenshots/QuickInstall/Step2DatabaseConnection.rst.txt

TYPO3 can either connect to an existing empty database or create a new one.

The list of databases available is dependent on which database drivers are installed on the host.


For example, if an instance of TYPO3 is intended to be used with a MySQL database then the PHP extension `pdo_mysql` is required.
Once it is installed, :guilabel:`MySQL Database` will be an available option.


.. include:: ../Images/AutomaticScreenshots/QuickInstall/Step3ChooseDb.rst.txt

Create Administrative User & Set Site Name
------------------------------------------

An administrator account needs to be created to gain access to TYPO3's backend.

An email address for this user can also be specified and a name can also be given.

.. include:: ../Images/AutomaticScreenshots/QuickInstall/Step4AdminUserSitename.rst.txt


Initialise
-----------

TYPO3 offers two options for initialisation: Creating an empty starting page or
it can go directly to the backend administrative interface.
Beginners should select the first option and allow TYPO3 to create an empty starting page,
this will also generate a site configuration file.

.. include:: ../Images/AutomaticScreenshots/QuickInstall/Step5LastStep.rst.txt

Next Steps
----------

Now that the installation is complete, TYPO3 can be :ref:`configured <setup>`.
