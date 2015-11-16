.. include:: /_shared/rc1-notice.txt

Console Application to Existing Database (Database First)
=========================================================

In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework. You will use reverse engineering to create an Entity Framework model based on an existing database.

.. contents:: `In this article:`
    :depth: 2
    :local:

.. include:: /_shared/sample.txt
.. _sample: https://github.com/aspnet/EntityFramework.Docs/tree/master/docs/getting-started/full-dotnet/sample/EFGetStarted.ConsoleApp.ExistingDb

Prerequisites
-------------

The following prerequisites are needed to complete this walkthrough:

* Visual Studio 2013 or Visual Studio 2015
* `Latest version of NuGet Package Manager`_
* `Blogging database`_

Latest version of NuGet Package Manager
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. include:: /_shared/ensure-vs-nuget-version.txt

Blogging database
^^^^^^^^^^^^^^^^^

This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.

.. include:: /getting-started/_shared/create-blogging-database-vs.txt

Create a new project
--------------------

* Open Visual Studio (this walkthrough uses 2015 but you can use any version from 2013 onwards)
* :menuselection:`File --> New --> Project...`
* From the left menu select :menuselection:`Templates --> Visual C# --> Windows`
* Select the **Console Application** project template
* Ensure you are targeting **.NET Framework 4.5.1** or later
* Give the project a name and click **OK**

Install Entity Framework
------------------------

To use EF7 you install the package for the database provider(s) you want to target. This walkthrough uses SQL Server. For a list of available providers see :doc:`/providers/index`.

* :menuselection:`Tools --> NuGet Package Manager --> Package Manager Console`
* Run ``Install-Package EntityFramework.MicrosoftSqlServer –Pre``

To enable reverse engineering from an existing database we need to install a couple of other packages too.

* Run ``Install-Package EntityFramework.Commands –Pre``
* Run ``Install-Package EntityFramework.MicrosoftSqlServer.Design –Pre``

Reverse engineer your model
---------------------------

Now it's time to create the EF model based on your existing database.

* :menuselection:`Tools –> NuGet Package Manager –> Package Manager Console`
* Run the following command to create a model from the existing database

.. literalinclude:: sample/EFGetStarted.ConsoleApp.ExistingDb/ScaffoldCommand.txt

The reverse engineer process created entity classes and a derived context based on the schema of the existing database.

Entity Classes
^^^^^^^^^^^^^^

The entity classes are simple C# objects that represent the data you will be querying and saving.

.. literalinclude:: sample/EFGetStarted.ConsoleApp.ExistingDb/Blog.cs
        :language: c#
        :linenos:

Derived Context
^^^^^^^^^^^^^^^

The context represents a session with the database and allows you to query and save instances of the entity classes.

.. note::
    Notice the ``OnConfiguring`` method (new in EF7) that is used to specify the provider to use and, optionally, other configuration too.

.. literalinclude:: sample/EFGetStarted.ConsoleApp.ExistingDb/BloggingContext.cs
        :language: c#
        :linenos:

Use your model
--------------

You can now use your model to perform data access.

* Open *Program.cs*
* Replace the contents of the file with the following code

.. literalinclude:: sample/EFGetStarted.ConsoleApp.ExistingDb/Program.cs
        :language: c#
        :linenos:

* :menuselection:`Debug --> Start Without Debugging`

You will see that one blog is saved to the database and then the details of all blogs are printed to the console.

.. image:: _static/output-existing-db.png
