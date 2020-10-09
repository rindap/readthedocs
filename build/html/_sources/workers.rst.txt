***********
Workers
***********

In order to perform more tasks, perform tasks more precisely and in-time, Workers interact with TaskQueues. 
Workers always listen to multiple TaskQueues or no Queue at all. When a task is feeded to TaskQueue, based on 
eligibility, capability and role of a Worker, a task is assigned to Worker to perform it. For example, a Worker 
can be a person in a call center,  a web service endpoint that listens to instructions, or an expert fixing the 
system failure. 


Worker Properties
------------------


.. list-table:: Properties
   :widths: 25 50
   :header-rows: 1

   * - field
     - description
   * - **sid**
     - The unique string that we created to identify the Worker resource.
   * - **account_sid**
     - The SID of the Account that created the Worker resource.     
   * - **workspace_sid**
     - The SID of the Workspace that contains the Worker
   * - **available**
     - Whether the Worker is available to perform tasks.
   * - **friendly_name**
     - The string that you assigned to describe the resource. Friendly names are case insensitive, and unique within the Rindap Workspace.
   * - **attributes**
     - A JSON String for describing the Worker and its features such as skills and information. This value is used for deciding which TaskQueues the Worker can receive Tasks from
   * - **activity**
     - This Activity determines the Worker's current state in the system, as well as whether the Worker can accept new Task assignments. There are 4 types of Activities a Worker can be set to:
        #. idle: the Worker is availabe and is waiting for Task Assignments 
        #. busy: the Worker is not available. The Worker is set to this Activity when a Reservation for this Worker is accepted.
        #. offline: the Worker is not available.
        #. reserved: This is a system Activity. At this stage, the Worker is paired with a Task and a reservation is created. 

   * - **date_created**
     - The date and time in GMT when the resource was created, specified in ISO 8601 format.
   * - **date_status_changed**
     - The date and time in GMT when the Activity of the Worker was last updated, specified in ISO 8601 format.
   * - **date_updated**
     - The date and time in GMT when the resource was last updated, specified in ISO 8601 format.
   * - **url**
     - The absolute URL of the resource
   * - **links**
     - The URLs of related resources.



Worker Attributes
------------------

A worker's attributes are used to simply and accurately represent a working entity on Rindap. These attributes can 
be skills and information about a person , in the case of a human Worker. Or it can be some identifying data and 
hard-coded parameters for a digital entity like a web service. When populating this field, you should keep in mind that 
the value in some of these fields might be used for creating the TaskQueue relations of this Worker, according to the 
`worker_requirements` field of TaskQueues , which is simply a JsonLogic rule. For example , a worker with attributes such as the one below:


.. code-block:: json

    {
      "name" : "john doe",
      "age": 44,
      "department" : "support",
      "location": "Utah"        
    }




would be receiving Tasks from a TaskQueue with worker_requirements as such:

.. code-block:: json

    {
      "==":[{"var":"department"},"support"]
    }

Because the requirement rules for the TaskQueue would be matching the attributes of the Worker. 


.. note::  **Understanding the "requirements vs attributes" model is fundamental.**

  With this **requirements vs attributes** model,you do not need to point out which TaskQueues a Worker needs to receive Tasks from or which Workers are suitable for the Tasks in a TaskQueue.
  When you create a Worker , the Worker will automatically be receiving Tasks from appropriate TaskQueues. And when attributes of a Worker are updated, the Workers related TaskQueues are updated accordingly. 
  
  The same goes for TaskQueues : When requirements for a TaskQueue are updated , the related Workers for the TaskQueue are updated.


Create A Worker
----------------

.. http:post:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Workers

You can create a Worker by simply providing a FriendlyName

.. list-table:: Query Parameters
   :widths: 20 15 5 60
   :header-rows: 1

   * - Parameter
     - Type
     - Default
     - Description
   * - **WorkspaceSID**
     - String
     - ""
     - The SID of the `Workspace`
   * - **FriendlyName**
     - String
     - ""
     - Human readable friendly name. It can be 512 characters long 
   * - **Activity**
     - String
     - ""
     - (optional) Activity for the created ``Worker`` to be assinged to. If not provided , The ``default_activity`` for the ``Workspace`` will be used
   * - **Attributes**
     - String
     - ""
     - A URL-encoded JSON string with the features and skills of a Worker. This value is used for deciding which TaskQueues the Worker can receive ``Task`` s from.
   * - **RateLimitProfileSid**
     - String
     - ""
     - (optional) The SID of the RateLimitProfile for overseeing the maximum rate of reservations this Worker can have hourly. For more information see :ref:`ratelimitprofiles`



Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X POST https://api.rindap.com/v1/rindap-rest-gw/Workspaces/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/Workers \
    --data-urlencode 'FriendlyName=my test worker' 
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"

**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.Worker;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console

        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Worker w = Worker.creator("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .setFriendlyNameAttributes("my test worker")
                .create();

            System.out.println(w);
        }
    }

**Phyton**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Create a worker
    worker = workspace.workers.create("My New Worker", activity="idle",
                                      task_queues=["WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"],
                                      rate_limit_profile_sid="RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Print content of worker
    print("FriendlyName: {}".format(worker.friendly_name))
    print("WorkerSid: {}".format(worker.sid))

**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Crate a worker
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
      .workers
      .create({
        friendlyName: "Friendly Worker Name" ,
        activity: "idle",
        rateLimitProfileSid: "RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        taskQueues: ["WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"]
      }, function(err, worker) {
        console.log(worker.sid);
        console.log(worker.friendlyName);
        console.log(worker.activity);
        console.log(worker.rateLimitProfileSid);
        console.log(worker.taskQueues);
      });

**C#**

.. code-block:: csharp

    using System;
    using System.ComponentModel.DataAnnotations;
    using Rindap;
    using Rindap.Rest.V1;
    using Rindap.Rest.V1.Workspace;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_AUTH_TOKEN", "YOUR_AUTH_TOKEN");

            // Create a worker
            var worker = WorkerResource.Create(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                friendlyName: "New Worker",
                activity: "offline",
                taskQueues: new String[] { "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" },
                rateLimitProfileSid: "RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
                );

            // Print content of worker
            Console.WriteLine("Friendly Name      : " + worker.FriendlyName);
            Console.WriteLine("Activity           : " + worker.Activity);
            Console.WriteLine("TaskQueues         : " + string.Join(",", worker.TaskQueues));
            Console.WriteLine("RateLimitProfileSid: " + worker.RateLimitProfileSid);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

  {
    "sid": "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "queues": [
      "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    ],
    "activity": "offline",
    "available": false,
    "friendly_name": "my test worker",
    "date_created": "2020-05-04T11:03:41+03:00",
    "date_updated": "2020-05-04T11:03:41+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
      "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
  }



Get All Workers
-----------------

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Workers

This endpoint retrives all Workers


.. list-table:: Query Parameters
   :widths: 20 15 5 60
   :header-rows: 1

   * - Parameter
     - Type
     - Default
     - Description
   * - **WorkspaceSID**
     - String
     - ""
     - The SID of the `Workspace`
   * - **FriendlyName**
     - String
     - ""
     - (optional) Human readable friendly name. Can be used for filtering
   * - **PageSize**
     - Integer
     - 50
     - Page size for paging
   * - **Page**
     - Integer
     - 0
     - Page number for paging


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shel**

.. code-block:: shell

  curl -X GET https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers \
  -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
  -H "Content-Type:application/x-www-form-urlencoded"

**Java**

.. code-block:: java

  // Install the Java helper library from rindap.com/docs/java/install

  import com.rindap.Rindap;
  import com.rindap.rest.v1.workspace.Worker;

  public class Example {
      // Find your Account Sid and Token at rindap.com/console
      

      public static void main(String[] args) {

          Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

          Worker.Reader reader = TaskQueue.reader("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx");

          for(Worker w:reader.read())
            System.out.println(w);
      }
  }

**Phyton**

.. code-block:: python

  from rindap.rest import Client
  from rindap.rest import Rindap

  client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
  rindap = Rindap(client)

  workspace = rindap.workspaces.get("WSb9d8cf8597f64f77a45666c4c0263862")
  worker_fetcher = workspace.workers.list(limit=10, page_size=5)
  worker = worker_fetcher.pop()
  print("FriendlyName: {}".format(worker.friendly_name))
  print("WorkerSid: {}".format(worker.sid))

**JS**

.. code-block:: javascript

  var Rindap = require('rindap');

  // Authenticate
  var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

  // Get all workers
  rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .workers
    .list({
      limit: 100,
      pageSize: 100,
    }, function(err, workers) {
      workers.forEach( function(worker) {
        console.log(worker.sid);
        console.log(worker.friendlyName);
        console.log(worker.activity);
        console.log(worker.rateLimitProfileSid);
        console.log(worker.taskQueues);
      });
    });

**C#**

.. code-block:: csharp

  using System;
  using System.ComponentModel.DataAnnotations;
  using Rindap;
  using Rindap.Rest.V1;
  using Rindap.Rest.V1.Workspace;

  class Program
  {
      static void Main(string[] args)
      {
          // Authenticate
          RindapClient.Init("YOUR_AUTH_TOKEN", "YOUR_AUTH_TOKEN");

          // Get all workers
          var workers = WorkerResource.Read(
              pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
              limit: 100,
              pageSize: 100
              );

          foreach (var worker in workers)
          {
              // Print content of worker
              Console.WriteLine("Worker SID         : " + worker.Sid);
              Console.WriteLine("Friendly Name      : " + worker.FriendlyName);
              Console.WriteLine("Activity           : " + worker.Activity);
              Console.WriteLine("TaskQueues         : " + string.Join(",", worker.TaskQueues));
              Console.WriteLine("RateLimitProfileSid: " + worker.RateLimitProfileSid);
          }
      }
  }


**The above commands return JSON structured like this:**

.. code-block:: json

  {
    "meta": {
      "page_size": 50,
      "page": 0,
      "first_page_url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers?Page=0&PageSize=50",
      "previous_page_url": null,
      "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers?Page=0&PageSize=50",
      "key": "workers",
      "next_page_url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers?Page=1&PageSize=50"
    },
    "workers": [
      {
        "sid": "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "queues": [
          "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
        ],
        "activity": "offline",
        "available": false,
        "friendly_name": "my test worker",
        "date_created": "2020-05-04T11:03:41+03:00",
        "date_updated": "2020-05-04T11:03:41+03:00",
        "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "links": {
          "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
        }
      }
    ]
  }

  
Fetch A Worker
-----------------

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Workers/{WorkerSID}

This endpoint fetches a single Worker with all Its details

.. list-table:: Query Parameters
   :widths: 20 15 5 60
   :header-rows: 1

   * - Parameter
     - Type
     - Default
     - Description
   * - **WorkspaceSID**
     - String
     - ""
     - The SID of the `Workspace`
   * - **WorkerSid**
     - String
     - ""
     - The SID of the Worker


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

  curl -X GET https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
  -H "Content-Type:application/x-www-form-urlencoded"

**Java**

.. code-block:: java

  // Install the Java helper library from rindap.com/docs/java/install

  import com.rindap.Rindap;
  import com.rindap.rest.v1.workspace.Worker;

  public class Example {
      // Find your Account Sid and Token at rindap.com/console
      
      public static void main(String[] args) {

          Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

          Worker w = Worker
              .fetcher("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
              .fetch();

          System.out.println(w);
      }
  }

**Phyton**

.. code-block:: python

  from rindap.rest import Client
  from rindap.rest import Rindap

  client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
  rindap = Rindap(client)

  workspace = rindap.workspaces.get("WSb9d8cf8597f64f77a45666c4c0263862")
  worker = workspace.workers.get("WKdc8c97f1e1c24410ae08605ff6e5d946").fetch()
  print("FriendlyName: {}".format(worker.friendly_name))
  print("WorkerSid: {}".format(worker.sid))

**JS**

.. code-block:: javascript

  var Rindap = require('rindap');

  // Authenticate
  var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

  // Get a workers with sid
  rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .workers("WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").fetch(
      function(err, worker) {
        console.log(worker.sid);
        console.log(worker.friendlyName);
        console.log(worker.activity);
        console.log(worker.rateLimitProfileSid);
        console.log(worker.taskQueues);
      });

**C#**

.. code-block:: csharp

  using System;
  using Rindap;
  using Rindap.Rest.V1.Workspace;

  class Program
  {
      static void Main(string[] args)
      {
          // Authenticate
          RindapClient.Init("YOUR_AUTH_TOKEN", "YOUR_AUTH_TOKEN");

          // Get a worker with SID
          var worker = WorkerResource.Fetch(
              pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
              pathSid: "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
              );

          // Print content of worker
          Console.WriteLine("Worker SID         : " + worker.Sid);
          Console.WriteLine("Friendly Name      : " + worker.FriendlyName);
          Console.WriteLine("Activity           : " + worker.Activity);
          Console.WriteLine("TaskQueues         : " + string.Join(",", worker.TaskQueues));
          Console.WriteLine("RateLimitProfileSid: " + worker.RateLimitProfileSid);
      }
  }


**The above command returns JSON structured like this:**

.. code-block:: json

  {
    "sid": "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "queues": [
      "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    ],
    "activity": "offline",
    "available": false,
    "friendly_name": "my test worker",
    "date_created": "2020-05-04T11:03:41+03:00",
    "date_updated": "2020-05-04T11:03:41+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
      "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
  }
  


Update A Worker
-------------------

.. http:put:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Workers/{WorkerSID}


.. list-table:: Query Parameters
   :widths: 20 15 5 60
   :header-rows: 1

   * - Parameter
     - Type
     - Default
     - Description
   * - **WorkspaceSID**
     - String
     - ""
     - The SID of the `Workspace`
   * - **WorkerSid**
     - String
     - ""
     - The SID of the Workspace with the Worker to update
   * - **FriendlyName**
     - String
     - ""
     - (optional) Human readable friendly name. It can be 512 characters long 
   * - **Activity**
     - String
     - ""
     - (optional) Activity for `Worker`. A worker needs to be in `idle` activity to receive a suitable `Task`
   * - **Attributes**
     - String
     - ""
     - A URL-encoded JSON string with the features and skills of a Worker. This value is used for deciding which TaskQueues the Worker can receive Tasks from. When you update this value , the related TaskQueues of this Worker are updated
   * - **RateLimitProfileSid**
     - String
     - ""
     - (optional) The SID of the new `RateLimitProfile` for the `Worker`

Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

  curl -X PUT https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \  
  --data-urlencode 'Activity=idle' \
  --data-urlencode 'TaskQueues=WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx,WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx,WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx' \
  --data-urlencode 'AssignmentStatus=pending' \
  -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
  -H "Content-Type:application/x-www-form-urlencoded"

**Java**

.. code-block:: java

  // Install the Java helper library from rindap.com/docs/java/install

  import com.rindap.Rindap;
  import com.rindap.rest.v1.workspace.Worker;

  public class Example {
      // Find your Account Sid and Token at rindap.com/console
      
      public static void main(String[] args) {

          Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

          Worker w = Worker
              .updater("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
              .setActivity("idle")
              .addTaskQueue("WQ11xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
              .addTaskQueue("WQ22xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
              .update();

          System.out.println(w);
      }
  }

**Phyton**

.. code-block:: python

  from rindap.rest import Client
  from rindap.rest import Rindap

  client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
  rindap = Rindap(client)

  workspace = rindap.workspaces.get("WSb9d8cf8597f64f77a45666c4c0263862")
  worker = workspace.workers.get("WKdc8c97f1e1c24410ae08605ff6e5d946")
  wk = worker.update(friendly_name="New Worker Name", activity="idle")
  print("FriendlyName: {}".format(wk.friendly_name))
  print("WorkerSid: {}".format(wk.sid))

**JS**

.. code-block:: javascript

  var Rindap = require('rindap');

  // Authenticate
  var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

  // Update a workers
  rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .workers("WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").update({
      friendlyName: "New Friendly Worker Name" ,
      activity: "offline",
      rateLimitProfileSid: "RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
      taskQueues: ["WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"]
    } ,function(err, worker) {
      console.log(worker.sid);
      console.log(worker.friendlyName);
      console.log(worker.activity);
      console.log(worker.rateLimitProfileSid);
      console.log(worker.taskQueues);
    });

**C#**

.. code-block:: csharp

  using System;
  using Rindap;
  using Rindap.Rest.V1.Workspace;

  class Program
  {
      static void Main(string[] args)
      {
          // Authenticate
          RindapClient.Init("YOUR_AUTH_TOKEN", "YOUR_AUTH_TOKEN");

          // Update a worker with SID
          var worker = WorkerResource.Update(
              pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
              pathSid: "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
              rateLimitProfileSid: "RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
              friendlyName: "New Friendly Name",
              taskQueues: new string[] { "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" }
              );

          // Print content of worker
          Console.WriteLine("Worker SID         : " + worker.Sid);
          Console.WriteLine("Friendly Name      : " + worker.FriendlyName);
          Console.WriteLine("Activity           : " + worker.Activity);
          Console.WriteLine("TaskQueues         : " + string.Join(",", worker.TaskQueues));
          Console.WriteLine("RateLimitProfileSid: " + worker.RateLimitProfileSid);
      }
  }


**The above command returns JSON structured like this:**

.. code-block:: json

  {
    "sid": "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "queues": [
      "WQ00xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
      "WQ11xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
      "WQ22xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    ],
    "activity": "idle",
    "available": true,
    "friendly_name": "my test worker",
    "date_created": "2020-05-04T11:03:41+03:00",
    "date_updated": "2020-05-04T11:03:41+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
      "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
  }


Delete A Worker
------------------

.. http:delete:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Workers/{WorkerSID}

.. list-table:: Query Parameters
   :widths: 20 15 5 60
   :header-rows: 1

   * - Parameter
     - Type
     - Default
     - Description
   * - **WorkspaceSID**
     - String
     - ""
     - The SID of the `Workspace`
   * - **WorkerSID**
     - String
     - ""
     - The SID of the Worker

Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

  curl -X DEL https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
  -H "Content-Type:application/x-www-form-urlencoded"

**Java**

.. code-block:: java

  // Install the Java helper library from rindap.com/docs/java/install

  import com.rindap.Rindap;
  import com.rindap.rest.v1.workspace.Worker;

  public class Example {
      // Find your Account Sid and Token at rindap.com/console
      
      public static void main(String[] args) {

          Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

          Worker.deleter("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
              .delete();
      }
  }

**Phyton**

.. code-block:: python

  from rindap.rest import Client
  from rindap.rest import Rindap

  client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
  rindap = Rindap(client)

  workspace = rindap.workspaces.get("WSb9d8cf8597f64f77a45666c4c0263862")
  if workspace.workers.get("WK369cfb99700446dea18fcabbd9f1a68c").delete():
      print("Worker has been deleted")

**JS**

.. code-block:: javascript

  var Rindap = require('rindap');

  // Authenticate
  var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

  // Delete a workers with sid
  rindap.workspaces('WS70cdccad050a41619162db0f32b7fc43')
    .workers("WK2b9748b550ee40a2bd010f06c78511e3").remove();

**C#**

.. code-block:: csharp

  using System;
  using Rindap;
  using Rindap.Rest.V1.Workspace;

  class Program
  {
      static void Main(string[] args)
      {
          // Authenticate
          RindapClient.Init("YOUR_AUTH_TOKEN", "YOUR_AUTH_TOKEN");

          // Delete a worker with SID
          var isDeleted = WorkerResource.Delete(
              pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
              pathSid: "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
              );

          if (isDeleted)
          {
              Console.WriteLine("Worker has been deleted!");
          }
      }
  }