
***********
Tasks
***********

In process management, a task is an activity that needs to be done, leading to the final deliverable. Tasks are a 
small essential piece of work that must be carried out to progress the whole project. For example, in business process 
systems, a process could consist of several tasks or activities to be completed in a defined period of time. CRM or 
ticketing systems allow organizations to generate tasks such as creating reminders in customer accounts that are synced 
with the calendar. 

Task Attributes
----------------

Task attributes are the core features of Tasks that are evaluated in the Workflow process. Consequently, based on 
the evaluation results, the path a task will follow in the Workflow is determined. Attributes are expressed in JSON data, 
for example:



Task Lifecycle
----------------

A Task has a Lifecycle to manage a task. When a task is created, either manually or automatically by a system, it can 
pass through several states it is completed by its own or is closed manually. A Task’s lifecycle starts when the task 
is generated, then the task is assigned, is processed, is completed and finally is verified. In the process management 
system, a user submits a task to a Workflow. Afterwards, the Task’s attributes are evaluated against the Filters in the 
Workflow , starting from the starting Filter defined in the Workflow until a match is found. When a match is found, the 
Task is pushed to the TaskQueue designated by the matching Filter. Task waits in the queue till a capable and available 
Worker is handled the Task. 


Task Properties
-----------------

.. list-table:: Properties
   :widths: 25 50
   :header-rows: 1

   * - field
     - description
   * - **sid**
     - The unique string that we created to identify the Task resource.
   * - **account_sid**
     - The SID of the Account that created the Task resource.
   * - **workspace_sid**
     - The SID of the Workspace that contains the Task
   * - **workflow_sid**
     - The SID of the Workflow that is controlling the Task
   * - **workflow_friendly_name**
     - The friendly name of the Workflow that is controlling the Task
   * - **initial_attributes**
     - A JSON String for the attributes of the task. This value cannot be changed, for showing the original state of the Task
   * - **attributes**
     - A JSON String for the attributes of the task. This field can be updated throughout the Lifecycle of the Task and will be used for evaluating filters of the Task's Workflow
   * - **assignment_status**
     - The current status of the Task's assignment. Can be: pending, awaiting_reservation,reserved, postponed, accepted, cancelled or completed.
   * - **task_queue_sid**
     - The SID of the TaskQueue that the Task was last sent to
   * - **task_queue_sid_friendly_name**
     - The friendly name of the TaskQueue that the Task was last sent to
   * - **age**
     - The number of seconds since the Task was created.
   * - **step_history**
     - A JSON Array of JSON Objects, showing the steps which were selected for the Task throughout its lifecycle. The JSON Objects hold the Step Id and the Date it was selected for the Task
   * - **task_queue_history**
     - A JSON Array of JSON Objects, showing the Task Queues to which the Task was sent, throughout its lifecyle. The JSON Objects hold the Task Queue SID, Task Queue friendly name and the Date 
   * - **last_charge_date**
     - The date The Task was last charged
   * - **next_charge_date**
     - The date The Task will be charged again, If It has not ended Its lifecycle
   * - **total_cost**
     - The total cost of charges for The Task so far
   * - **forked_from**
     - The SID of the Parent Task , if this Task is forked from another Task. For more information , see `Task Forking <https://rindap.com/task-forking/>`_
   * - **postponed_till**
     - The date and time in GMT, till the task is postponed , valid when The Task AssignmentStatus is `postponed`.
   * - **postponing_reason**
     - The explanation of the reason for postponing , valid when The Task AssignmentStatus is `postponed`.
   * - **loop_retries_left**
     - The number of retries left for the current Loop Filter in the Workflow, valid when The Task AssignmentStatus is `postponed`. For more information, see `Workflow Configuration <https://rindap.com/workflow/>`_ .
   * - **date_created**
     - The date and time in GMT when the resource was created, specified in ISO 8601 format.
   * - **date_updated**
     - The date and time in GMT when the resource was last updated, specified in ISO 8601 format.
   * - **url**
     - The absolute URL of the resource
   * - **links**
     - The URLs of related resources.


Create A Task
---------------

.. http:post:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Tasks

You can create a task by simply providing the Task Attributes and the Workflow for the task

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
     - The SID of the Workspace in which the Task is created
   * - **WorkflowSID**
     - String
     - ""
     - The SID of the Workflow that you would like Task to be handled by 
   * - **Attributes**
     - String
     - ""
     - A URL-encoded JSON string with the attributes of the new task. This value is passed to the Workflow's ``assignment_callback_url`` when the Task is assigned to a Worker.   
   * - **Timeout**
     - Integer
     - ""
     - (Optional) (Max=2073600) (Min=60) Timeout value in seconds, for the Task to be closed after, if the Task assignment_status is not `completed` or `cancelled` at the time of control.Task will be "cancelled" with reason "Task TTL Exceeded"


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block::  shell

    curl -X POST https://api.rindap.com/v1/rindap-rest-gw/Workspaces/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/Tasks \
    --data-urlencode 'WorkflowSid=WWb898bae42dec49e792f37f9639bf625c' 
    --data-urlencode 'Attributes={"some_integer": 55,"some_text": "hello world","some_boolean" : true}' \
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block::  java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.Task;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        

        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Task t = Task.creator("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .setAttributes("{\"some_integer\": 55, \"some_text\": \"hello world\",\"some_boolean\" : true}")
                .setWorkflowSid("WWXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX")
                .create();

            System.out.println(t);
        }
    }

**Python**

.. code-block::  python

    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Create a task
    task = workspace.tasks.create(workflow_sid="WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                                attributes='{"some_integer": 55, "some_text": "hello world","some_boolean" : true}')

    # Print content of task
    print("TaskSid: {}".format(task.sid))

**JS**

.. code-block::  javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Crate a task
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .tasks
    .create({
        attributes: '{"some_integer": 55, "some_text": "hello world","some_boolean" : true}',
        workflowSid: 'WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
    }, function(err, task) {
        console.log(task.sid);
        console.log(task.attributes);
        console.log(task.workflowSid);
    });

**CSharp**

.. code-block::  csharp

    using System;
    using System.Collections.Generic;
    using Newtonsoft.Json;
    using Rindap;
    using Rindap.Rest.V1.Workspace;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Create an attributes
            var attributes = JsonConvert.SerializeObject(new Dictionary<string, Object>()
            {
                {"type", "support"}
            }, Formatting.Indented);

            // Create a task
            var task = TaskResource.Create(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                workflowSid: "WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                attributes: attributes,
                timeout: 100
                );

            // Print task content
            Console.WriteLine("Workspace Sid : " + task.Sid);
            foreach (var attribute in task.Attributes)
            {
                Console.WriteLine("Attributes: ");
                Console.WriteLine("\tKey   :" + attribute.Key);
                Console.WriteLine("\tValue :" + attribute.Value);
            }
            Console.WriteLine("Timeout       : " + task.Timeout);
            Console.WriteLine("WorkflowSid   : " + task.WorkflowSid);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "WT9a69287d97fe4110a76ef9db8cf728f8",
    "account_sid": "ACXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workflow_sid": "WWXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    "workflow_friendly_name": "my first workflow",
    "initial_attributes": {
        "some_integer": 55,
        "some_text": "hello world"
        "some_boolean" : true    
    },
    "attributes": {
        "some_integer": 55,
        "some_text": "hello world"
        "some_boolean" : true    
    },
    "assignment_status": "pending",
    "step": -1,
    "reason": null,
    "date_created": "2020-01-01T16:11:35+03:00",
    "date_updated": "2020-01-01T16:11:35+03:00",
    "last_charge_date": "2020-01-01T16:11:35+03:00",
    "next_charge_date": "2020-02-01T16:11:35+03:00",
    "total_cost": "0.01",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks/WT9a69287d97fe4110a76ef9db8cf728f8",
    "links": {
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "workflow": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workflows/WWXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    },
    "age": 0
    }


Get All Tasks
-----------------

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Tasks


This endpoint retrives all Tasks

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
     - The SID of the Workspace in which the Task is created
   * - **FriendlyName**
     - String
     - ""
     - Human readable friendly name 
   * - **PageSize**
     - Integer
     - 50
     - Page size for paging
   * - **Page**
     - Integer
     - 0
     - Page number for paging


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**C#**

.. code-block::  csharp

    using System;
    using Rindap;
    using Rindap.Rest.V1.Workspace;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Fetch all tasks
            var tasks = TaskResource.Read(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                limit: 100,
                pageSize: 100
                );

            // Iterate on all tasks
            foreach (var task in tasks)
            {
                // Print task content
                Console.WriteLine("Workspace Sid : " + task.Sid);
                foreach (var attribute in task.Attributes)
                {
                    Console.WriteLine("Attributes: ");
                    Console.WriteLine("\tKey   :" + attribute.Key);
                    Console.WriteLine("\tValue :" + attribute.Value);
                }
                Console.WriteLine("Timeout       : " + task.Timeout);
                Console.WriteLine("WorkflowSid   : " + task.WorkflowSid);
            }
        }
    }

**Python**

.. code-block::  python

    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Fetch all tasks
    task_fetcher = workspace.tasks.list(limit=10, page_size=5)
    for task in task_fetcher:
        # Print content of task
        print("TaskSid: {}".format(task.sid))

**JS**

.. code-block::  javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");


    // List all tasks
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .tasks
    .list({
        limit: 100,
        pageSize: 100
    }, function(err, tasks) {
        tasks.forEach(function(task){
        console.log(task.sid);
        console.log(task.attributes);
        console.log(task.workflowSid);
        })
    });


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "meta": {
        "page_size": 50,
        "page": 0,
        "first_page_url": "string",
        "previous_page_url": "string",
        "url": "string",
        "key": "string",
        "next_page_url": "string"
    },
    "workspaces": [
        {
        "sid": "string",
        "friendly_name": "string",
        "event_callback_url": "string",
        "account_sid": "string",
        "date_created": "2020-04-02T12:30:58.083Z",
        "date_updated": "2020-04-02T12:30:58.083Z",
        "url": "string",
        "links": {
            "tasks": "string",
            "workers": "string",
            "workflows": "string",
            "task_queues": "string"
        }
        }
    ]
    }



Fetch a Task
-----------------

This endpoint fetches a single Task with all its details

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Tasks/{TaskSID}


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
     - The SID of the Workspace in which the Task is created
   * - **TaskSID**
     - String
     - ""
     - TaskSID (Secure Identifier) - a unique ID for the Task


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block::  shell

    curl -X GET https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"

**Java**

.. code-block::  java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.Task;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Task t = Task
                .fetcher("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .fetch();

            System.out.println(t);
        }
    }

**Python**

.. code-block::  python

    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Get a task with sid
    task = workspace.tasks.get("WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").fetch()

    # Print content of task
    print("TaskSid: {}".format(task.sid))

**JS**

.. code-block::  javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");


    // Get a tasks with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .tasks("WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .fetch(function(err, task) {
        console.log(task.sid);
        console.log(task.attributes);
        console.log(task.workflowSid);
    });

**C#**

.. code-block::  csharp

    using System;
    using Rindap;
    using Rindap.Rest.V1.Workspace;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Get a tasks with SID
            var task = TaskResource.Fetch(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
                );

            // Print task content
            Console.WriteLine("Workspace Sid : " + task.Sid);
            foreach (var attribute in task.Attributes)
            {
                Console.WriteLine("Attributes: ");
                Console.WriteLine("\tKey   :" + attribute.Key);
                Console.WriteLine("\tValue :" + attribute.Value);
            }
            Console.WriteLine("Timeout       : " + task.Timeout);
            Console.WriteLine("WorkflowSid   : " + task.WorkflowSid);
        }
    }


**The above command returns JSON structured like this:**


.. code-block:: json

    {
    "sid": "WT9a69287d97fe4110a76ef9db8cf728f8",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workflow_sid": "WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workflow_friendly_name": "my first workflow",
    "initial_attributes": {
        "some_integer": 55,
        "some_text": "hello world"
        "some_boolean" : true    
    },
    "attributes": {
        "some_integer": 55,
        "some_text": "hello world"
        "some_boolean" : true    
    },
    "assignment_status": "pending",
    "step": -1,
    "reason": null,
    "date_created": "2020-01-01T16:11:35+03:00",
    "date_updated": "2020-01-01T16:11:35+03:00",
    "last_charge_date": "2020-01-01T16:11:35+03:00",
    "next_charge_date": "2020-02-01T16:11:35+03:00",
    "total_cost": "0.01",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "workflow": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workflows/WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    },
    "age": 0
    }





Update a Task
-----------------

.. http:put:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Tasks/{TaskSID}

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
     - The SID of the Workspace in which the Task is created
   * - **TaskSID**
     - String
     - ""
     - TaskSID (Secure Identifier) - a unique ID for the Task
   * - **Attributes**
     - String
     - ""
     - (Optional) A URL-encoded JSON string that will replace the `Attributes` field of your Task. You should use this parameter for updating the data related to this Task.
   * - **AssignmentStatus**
     - String
     - ""
     - (Optional) you can update the assignment status of your task by one of these options: `pending`,`completed`,`cancelled`. If you set your task's `assignment_status` to `pending` , it will be processed by Its Workflow once again , from the Workflow Step that's shown at the `step` field of the Task. If you set this field to `cancelled` , you can also send the `Reason` parameter for telling what reason the Task is cancelled for. 
   * - **Reason**
     - String
     - ""
     - (Optional) Can only be sent when the `AssignmentStatus` parameter is set as `cancelled` as well.


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block::  shell

    curl -X PUT https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    --data-urlencode 'Attributes={"result_of_some_query": 55,"some_text": "new related info"}' \
    --data-urlencode 'AssignmentStatus=pending' \
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"

**Java**

.. code-block::  java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.Task;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Task t = Task
                .updater("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .setAttributes("{\"result_of_some_query\": 55, \"some_text\": \"new related info\"}")
                .setAssignmentStatus("pending")
                .update();

            System.out.println(t);
        }
    }

**Python**

.. code-block::  python

    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Get a task with sid
    task = workspace.tasks.get("WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Update task
    updated_task = task.update(assignment_status="completed")

    # Print content of task
    print("TaskSid: {}".format(updated_task.sid))

**JS**

.. code-block::  javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Update a tasks
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .tasks("WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .update({
        assignmentStatus: "cancelled"
    },function(err, task) {
        console.log(err);
        console.log(task.sid);
        console.log(task.attributes);
        console.log(task.workflowSid);
    });

**C#**

.. code-block::  csharp

    using System;
    using Rindap;
    using Rindap.Rest.V1.Workspace;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Update a tasks with SID
            var task = TaskResource.Update(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                assignmentStatus: "cancelled"
                );

            // Print task content
            Console.WriteLine("Workspace Sid : " + task.Sid);
            foreach (var attribute in task.Attributes)
            {
                Console.WriteLine("Attributes: ");
                Console.WriteLine("\tKey   :" + attribute.Key);
                Console.WriteLine("\tValue :" + attribute.Value);
            }
            Console.WriteLine("Timeout       : " + task.Timeout);
            Console.WriteLine("WorkflowSid   : " + task.WorkflowSid);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "WT9a69287d97fe4110a76ef9db8cf728f8",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workflow_sid": "WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workflow_friendly_name": "my first workflow",
    "initial_attributes": {
        "some_integer": 55,
        "some_text": "hello world"
        "some_boolean" : true    
    },
    "attributes": {
        "result_of_some_query": 55,
        "some_text": "new related info"
    },
    "assignment_status": "pending",
    "step": -1,
    "reason": null,
    "date_created": "2020-01-01T16:11:35+03:00",
    "date_updated": "2020-01-01T16:11:35+03:00",
    "last_charge_date": "2020-01-01T16:11:35+03:00",
    "next_charge_date": "2020-02-01T16:11:35+03:00",
    "total_cost": "0.01",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "workflow": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workflows/WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    },
    "age": 0
    }


Delete a Task
----------------

.. http:delete:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Tasks/{TaskSID}


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
     - The SID of the Workspace in which the Task is created
   * - **TaskSID**
     - String
     - ""
     - TaskSID (Secure Identifier) - a unique ID for the Task





Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block::  shell

    curl -X DEL https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block::  java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.Task;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Task.deleter("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .delete();
        }
    }

**Python**

.. code-block::  python

    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Get a task with sid and delete it
    if workspace.tasks.get("WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").delete():
        print("Task has been deleted!")

**JS**

.. code-block::  javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");


    // Get a tasks with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .tasks("WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .remove();

**C#**

.. code-block::  csharp

    using System;
    using Rindap;
    using Rindap.Rest.V1.Workspace;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Get a tasks with SID
            var isDeleted = TaskResource.Delete(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
                );

            if (isDeleted)
            {
                Console.WriteLine("Task has been deleted!");
            }
        }
    }


