Installing iDempiere for Execution
=====================

In order to run iDempiere you need to have a JDK (not JRE) version of java, and a proper database (PostgreSQL and Oracle are supported).

The examples on this guide are using the following versions:
- Ubuntu 18.04.2 64 bits
- PostgreSQL 9.6.12
- PostgreSQL contrib (for UUID support) - already included after postgresql 10
- OpenJDK 11.0.2

  .. note::
    This guide can be used in other systems (even Windows) taking care of installing the corresponding packages and using corresponding commands.__ 

Install Ubuntu
---------------------------------------------------------
Please refer to `Ubuntu installation guide <http://www.ubuntu.com/download>`

Downloaded and installed Ubuntu Server 18.04.2 LTS

Install PostgreSQL 9.6.12
---------------------------------------------------------

Install
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
iDempiere can also run with Oracle 11G or 12C, and also with PostgreSQL >= 9.6, for this tutorial we use postgresql 9.6.10 - see http://www.postgresql.org/download/linux/ubuntu/ for details

.. code-block:: text

  echo "deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main" | sudo tee /etc/apt/sources.list.d/pgdg.list
  wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
  sudo apt-get update
  # NOTE: alternatively you can install postgresql 10 11 or 12 (it includes contrib)
  # sudo apt-get install postgresql-12
  sudo apt-get install postgresql-9.6 postgresql-contrib-9.6
  sudo service postgresql start
  sudo update-rc.d postgresql defaults
  
Assign a password to user postgres
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In order to create the database the installer needs to know the password of user postgres, by default this user doesn't have a password in ubuntu (windows installer asks for a password).

Please take note of the password you assign here as it will be required in the setup process:

Steps are (replace your_chosen_password by your preferred):

.. code-block:: text

  sudo su - postgres
  psql -U postgres -c "alter user postgres password 'your_chosen_password'"
  logout

Configure pg_hba.conf
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
After installing postgres you must check the correct configuration of /etc/postgresql/9.6/main/pg_hba.conf (or /etc/postgresql/11/main/pg_hba.conf in case you installed postgresql 11).

The following line requires change of the authentication method:

Please NOTE that some guides suggest configuring trust instead of md5 - but that creates a security issue on your postgres server.

Alternatively, if you installed postgresql 10 or 11, it's better recommended to use the authentication method scram-sha-256, this requires some additional configuration and checking of your client libraries in all systems accessing your postgresql server, for more information see IDEMPIERE-3959 and postgresql auth-password documentation.


