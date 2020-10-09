.. tryingrtfd documentation master file, created by
   sphinx-quickstart on Tue Sep  8 18:39:16 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to tryingrtfd's documentation!
======================================



############
Introduction
############

Welcome to the Rindap API! You can use our API to access Rindap API endpoints, which can get information on Tasks, Taskrouters, Workflows, Workers etc. in our database.

We have language bindings in Java, CSharp, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.


##############
Authentication
##############

Rindap uses API keys to allow access to the API. You can register a new Rindap API key at our developer portal.

Rindap expects for the API key to be included in all API requests to the server in a header that looks like the following:

``Authorization:Bearer [Account SID].[Auth token]``


.. note:: You must replace **[Account SID]** and **[Auth token]** with your personal API credentials found on rindap.com developer console.


.. toctree::
  :maxdepth: 2
  :caption: Contents:

  workspaces 
  workflows
  workers
  tasks
  rate_limit_profiles
  task_queues
  reservations



.. .. include:: ./workers.rst

.. .. include:: ./tasks.rst

