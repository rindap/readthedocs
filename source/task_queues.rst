
*********************
Task Queues
*********************

TaskQueue helps the process management system to manage and control how tasks are executed and determine 
capable Workers to accomplish those Tasks. Tasks are evaluated through predefined Workflow and based on the 
evaluation results, Tasks are given to TaskQueue to  handle by an appropriate Worker. 

TaskQueue Properties
----------------------


.. list-table:: Properties
   :widths: 25 50
   :header-rows: 1

   * - field
     - description
   * - **sid**
     - The unique string that we created to identify the TaskQueue resource.
   * - **account_sid**
     - The SID of the Account that created the TaskQueue resource.
   * - **workspace_sid**
     - The SID of the Workspace that contains the TaskQueue
   * - **friendly_name**
     - The string that you assigned to describe the resource. 
   * - **worker_requirements**
     - A JSON String for the JsonLogic rule , deciding the Worker relations for the TaskQueue
   * - **date_created**
     - The date and time in GMT when the resource was created, specified in ISO 8601 format.
   * - **date_updated**
     - The date and time in GMT when the resource was last updated, specified in ISO 8601 format.
   * - **url**
     - The absolute URL of the RateLimitProfile resource
   * - **links**
     - The URLs of related resources.



TaskQueue worker requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A TaskQueue's worker requirements is a JsonLogic rule,used to simply and accurately represent the necessary features or skills required for handling a Task in this TaskQueue

For example , a worker with attributes such as the one below:

.. code-block:: json

    {
    "name" : "john doe"
    "age": 44
    "department" : "support"
    "location": "Utah"
    }


would be receiving Tasks from a TaskQueue with worker_requirements as such:

.. code-block:: json

    {
    "==":[{"var":"department"},"support"]
    }

Because the requirement rules for the TaskQueue would be matching the attributes of the Worker. 


.. note::  **Understanding the "requirements vs attributes" model is fundamental.**

    With this **requirements vs attributes** model,you do not need to point out 
    which TaskQueues a Worker needs to receive Tasks from or which Workers are suitable for the Tasks in a TaskQueue.

    When you create a Worker , the Worker will automatically be receiving Tasks from appropriate TaskQueues. And when 
    attributes of a Worker are updated, the Workers related TaskQueues are updated accordingly. 
    
    The same goes for TaskQueues : When requirements for a TaskQueue are updated , the related Workers for the TaskQueue 
    are updated.



Create A TaskQueue
----------------------

.. http:post:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/TaskQueues

You can create a TaskQueue by simply providing a `FriendlyName`

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
   * - **WorkerRequirements**
     - JSON Object
     - {"==":[1,1]}
     - (optional) A URL-encoded JSON string , representing a JsonLogic rule for Worker relation


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X POST https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/TaskQueues \
    --data-urlencode 'FriendlyName=my test task queue' \
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.TaskQueue;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        

        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            TaskQueue tq = TaskQueue.creator("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .setFriendlyName("my test task queue")
                .create();

            System.out.println(tq);
        }
    }


**Python**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Create a task queue
    task_queue = workspace.task_queues.create("My new task queue")

    # Print task queue content
    print("TaskQueue FriendlyName: {}".format(task_queue.friendly_name))
    print("TaskQueue Sid: {}".format(task_queue.sid))


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");


    // Crate a task queue
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .taskQueues
    .create({
        friendlyName: "Friendly Name"
    }, function(err, taskQueue) {
        console.log(taskQueue.sid);
        console.log(taskQueue.friendlyName);
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
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Create a taskQueue
            var taskQueue = TaskQueueResource.Create(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                friendlyName: "New Task Queue"
                );

            // Print taskQueue content
            Console.WriteLine("TaskQueue Sid : " + taskQueue.Sid);
            Console.WriteLine("Friendly Name : " + taskQueue.FriendlyName);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "friendly_name": "my test task queue",
    "date_created": "2020-05-04T09:38:26+03:00",
    "date_updated": "2020-05-04T09:38:26+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/TaskQueues/WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
    }



Get All TaskQueues
--------------------------

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/TaskQueues

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
     - (optional) Human readable friendly name 
   * - **PageSize**
     - Integer
     - 50
     - Page size for paging
   * - **Page**
     - Integer
     - 0
     - Page number for paging



Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X GET https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/TaskQueues \
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.TaskQueue;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        

        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            TaskQueue.Reader reader = TaskQueue.reader("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx");

            for(TaskQueue tq:reader.read())
            System.out.println(tq);
        }
    }


**Python**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Get all task queues
    task_queue_fetcher = workspace.task_queues.list(limit=10, page_size=5)
    for task_queue in task_queue_fetcher:
        # Print task queue content
        print("TaskQueue FriendlyName: {}".format(task_queue.friendly_name))
        print("TaskQueue Sid: {}".format(task_queue.sid))


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");


    // List all task queues
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .taskQueues
    .list({
        limit: 100,
        pageSize: 100
    }, function(err, taskQueues) {
        taskQueues.forEach(function(taskQueue) {
        console.log(taskQueue.sid);
        console.log(taskQueue.friendlyName);
        })
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
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // List all taskQueues
            var taskQueues = TaskQueueResource.Read(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                limit: 100,
                pageSize: 100
                );

            foreach (var taskQueue in taskQueues)
            {
                // Print taskQueue content
                Console.WriteLine("TaskQueue Sid : " + taskQueue.Sid);
                Console.WriteLine("Friendly Name : " + taskQueue.FriendlyName);
            }
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "meta": {
        "page_size": 50,
        "page": 0,
        "first_page_url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/TaskQueues?Page=0&PageSize=50",
        "previous_page_url": null,
        "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/TaskQueues?Page=0&PageSize=50",
        "key": "task_queues",
        "next_page_url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/TaskQueues?Page=1&PageSize=50"
    },
    "task_queues": [
        {
        "sid": "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "friendly_name": "my test task queue",
        "date_created": "2020-05-04T09:38:26+03:00",
        "date_updated": "2020-05-04T09:38:26+03:00",
        "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/TaskQueues/WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "links": {
            "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
        }
        }
    ]
    }



Fetch a TaskQueue
--------------------

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/TaskQueues/{TaskQueueSID}

This endpoint fetches a single TaskQueue with all its details

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
   * - **TaskQueueSID**
     - String
     - ""
     - The SID of the TaskQueue


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X GET https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /TaskQueues/WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"



**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.TaskQueue;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            TaskQueue tq = TaskQueue
                .fetcher("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .fetch();

            System.out.println(tq);
        }
    }


**Python**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Get task with sid
    task_queue = workspace.task_queues.get("WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").fetch()

    # Print task queue content
    print("TaskQueue FriendlyName: {}".format(task_queue.friendly_name))
    print("TaskQueue Sid: {}".format(task_queue.sid))



**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Get a task queues with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .taskQueues("WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .fetch(function(err, taskQueue) {
        console.log(taskQueue.sid);
        console.log(taskQueue.friendlyName);
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
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Get a taskQueue with SID
            var taskQueue = TaskQueueResource.Fetch(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
                );

            // Print taskQueue content
            Console.WriteLine("TaskQueue Sid : " + taskQueue.Sid);
            Console.WriteLine("Friendly Name : " + taskQueue.FriendlyName);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "friendly_name": "my test task queue",
    "date_created": "2020-05-04T09:38:26+03:00",
    "date_updated": "2020-05-04T09:38:26+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/TaskQueues/WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
    }



Update a TaskQueue
-----------------------

.. http:put:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/TaskQueues/{TaskQueueSID}

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
   * - **TaskQueueSID**
     - String
     - ""
     - The SID of the TaskQueue
   * - **FriendlyName**
     - String
     - ""
     - Human readable friendly name. It can be 512 characters long 
   * - **WorkerRequirements**
     - JSON Object
     - {"==":[1,1]}
     - (optional) A URL-encoded JSON string , representing a JsonLogic rule for deciding the Worker relations for the TaskQueue


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X PUT https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /TaskQueues/WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    --data-urlencode 'FriendlyName=my new name of task queue' \
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"



**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.TaskQueue;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            TaskQueue tq = TaskQueue
                .updater("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .setFriendlyName("my new name of task queue")
                .update();

            System.out.println(tq);
        }
    }


**Python**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Get task with sid
    task_queue = workspace.task_queues.get("WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    updated_task_queue = task_queue.update(friendly_name="My Task Queue New Name")

    # Print task queue content
    print("TaskQueue FriendlyName: {}".format(updated_task_queue.friendly_name))
    print("TaskQueue Sid: {}".format(updated_task_queue.sid))


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");


    // Update a task queues with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .taskQueues("WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .update({
        friendlyName: "New Friendly Name"
    }, function(err, taskQueue) {
        console.log(taskQueue.sid);
        console.log(taskQueue.friendlyName);
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
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Update a taskQueue with SID
            var taskQueue = TaskQueueResource.Update(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                friendlyName: "New Friendly Name"
                );

            // Print taskQueue content
            Console.WriteLine("TaskQueue Sid : " + taskQueue.Sid);
            Console.WriteLine("Friendly Name : " + taskQueue.FriendlyName);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "friendly_name": "my new name of task queue",
    "date_created": "2020-05-04T09:38:26+03:00",
    "date_updated": "2020-05-04T09:38:26+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/TaskQueues/WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
    }


Delete a TaskQueue
--------------------

.. http:delete:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/TaskQueues/{TaskQueueSID}


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
   * - **TaskQueueSID**
     - String
     - ""
     - The SID of the TaskQueue


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X DEL https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /TaskQueues/WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.TaskQueue;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            TaskQueue.deleter("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .delete();
        }
    }


**Python**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Get task with sid and delete it
    if workspace.task_queues.get("WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").delete():
        print("TaskQueue has been deleted!")


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Delete a task queues with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .taskQueues("WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .remove();


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
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Delete a taskQueue with SID
            var isDeleted = TaskQueueResource.Delete(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
                );

            if (isDeleted)
            {
                Console.WriteLine("TaskQueue has been deleted!");
            }
        }
    }


