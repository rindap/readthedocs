.. tryingrtfd documentation master file, created by
   sphinx-quickstart on Tue Sep  8 18:39:16 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Rindap - REST API for BPM
======================================


############
Introduction
############

Welcome to the Rindap API documentation! Rindap helps companies to increase their work efficiency by offering developers a low-code BPM platform with RESTful API in order to automate business processes based on the requirements set by the management.

The reason Rindap was developed was the reason to help developers manage to automate business processes by using APIs without the need for high-cost out-of-the-box programs or complex custom developments.

Developers and business analysts can cooperate to automate business processes on Rindap with ease. Developers can design workflows via a user-friendly drag&drop visual modeller, easily set business rules by filters that use visual code blocks and utilize Rindap’s RESTful API to connect to any platform, device or system without any boundaries. 

By using Rindap’s RESTful API you can achieve end-to-end digital process automation across people, platforms and devices without being limited by company or department data silos. Rindap is fully compatible with any legacy software and can be used without any problems across the entire enterprise technology stack. 

You can use our API documentation to get familiarized with Rindap API endpoints, which can are related to Tasks, Workspaces, Workflows, Workers and etc. 




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

