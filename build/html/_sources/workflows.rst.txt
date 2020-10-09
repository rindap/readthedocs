
***********
Workflows
***********

Workflows manage and control the distribution of tasks to workers based on `Workflows configurations <https://rindap.com/workflow/>`_ and capabilities of 
workers.  Workflows configurations is the most important aspect of a workflow process which specifies the order in which 
tasks must be executed over time by simply described in JSON format and pair tasks into Queues. Each configuration 
includes a set of conditions which control and evaluate tasks attributes. The rules of condition were defined in 
JSONLogic format. Read more about `JSONLogic here <https://rindap.com/json-logic/>`_ .

When a task is added to the workspace, its own workflow is executed in order to control the task to be channeled 
into proper Queues. The workflow managing process will continue until the task is either completed or 
cancelled. Afterwards, based on the evaluation process of Workflows configurations, a task is assigned to a TaskQueue. 
Workers listen to TaskQueues and reserve the task according to workers availability and capabilities. Assignment Callback 
mechanism will be enabled to execute the intended request. Workflow's Assignment Callback contains all the 
information necessary for the worker to perform the task (for example, emailing or texting the Worker or showing a 
Notification If your Worker is logged in some sort of portal or making an IVR call to your Worker and later updating 
the Task with the input from the call).

Main features of Workflow listed as following:

* Condition: Condition of each Workflow is defined in the filters field of Workflow Configuration. Condition determines whether Task execution happens or not.  
* Fork: A Task can be triggered to fork out to create new Tasks for different Workflows, executed in parallel.Read more about use cases and details here at `Task Forking <https://rindap.com/task-forking/>`_ 
* Redirected to: A filter can redirect the flow of the Task to another Filter to run a different course , like a branch.
* Looping Filter : A looping filter that can be validated between time periods, till It turns true or It's validated enough


You can read about these features and more about `Workflows here <https://rindap.com/workflow/>`_ .


Workflow Properties
----------------------



.. list-table:: Properties
   :widths: 25 50
   :header-rows: 1

   * - field
     - description
   * - **sid**
     - The unique string that we created to identify the Workflow resource.
   * - **account_sid**
     - The SID of the Account that created the Workflow resource.
   * - **workspace_sid**
     - The SID of the Workspace that contains the Workflow
   * - **friendly_name**
     - The string that you assigned to describe the resource. Friendly names are case insensitive, and unique within the Rindap Workspace.
   * - **assignment_callback_url**
     - The URL that we call when a task managed by the Workflow is assigned to a Worker.
   * - **fallback_assignment_callback_url**
     - The URL that we call when a call to the AssignmentCallbackUrl fails.
   * - **task_reservation_timeout**
     - How long Rindap will wait for a confirmation response from your application after notifying the ``AssignmentCallbackUrl`` about the reservation.Can be up to ``86,400`` (24* hours) , minimum ``5`` , and the default is ``86,400``..     
   * - **configuration**
     - A URL-encoded JSON string that contains the Workflow's configuration. See `Configuring Workflows <https://rindap.com/workflow/>`_  for more information.
   * - **date_created**
     - The date and time in GMT when the resource was created, specified in ISO 8601 format.
   * - **date_updated**
     - The date and time in GMT when the resource was last updated, specified in ISO 8601 format.
   * - **url**
     - The absolute URL of the resource
   * - **links**
     - The URLs of related resources.



Create A Workflow
----------------------

.. http:post:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Workflows


You can easily design and create your Workflow on our developer Portal with an intuitive drag&drop interface. 
For more information `Workflows <https://rindap.com/workflow/>`_ 



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
     - Human readable friendly name. It can be 512 characters long 
   * - **AssignmentCallbackUrl**
     - URL
     - ""
     - The URL that we call when a task managed by the Workflow is assigned to a Worker.
   * - **FallbackAssignmentCallbackUrl**
     - URL
     - ""
     - The URL that we call when a call to the assignment_callback_url fails.
   * - **Configuration**
     - String
     - ""
     - A URL-encoded JSON string that contains the Workflow's configuration. For more information , see `Workflows <https://rindap.com/workflow/>`_ .
   * - **TaskReservationTimeout**
     - Integer
     - 86400
     - (optional) How long Rindap will wait for a confirmation response from your application after notifying the `assignment_callback_url` about the reservation.


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X POST https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workflows \
    --data-urlencode 'FriendlyName=my test workflow' 
    --data-urlencode 'Configuration={"first_step":"FS111","filters":{"FS111":{"true_queue_sid":"WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx","false_filter_sid":"FS222","name":"Hot","conditions":{">":[{"var":"temp"},"300"]}},"FS222":{"true_queue_sid":"WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx","name":"Mild","conditions":{"!=":["1","1"]}}}}' 
    --data-urlencode 'AssignmentCallbackUrl=https://assignment-callback.my-backbone.mydomain.org' 
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"




**Java**

.. code-block::  java


    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.Workflow;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console

        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Workflow wf = Workflow.creator("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "my test workflow",
                "https://assignment-callback.my-backbone.mydomain.org",
                "{\"first_step\":\"FS111\",\"filters\":{\"FS111\":{\"true_queue_sid\":\"WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\",\"false_filter_sid\":\"FS222\",\"name\":\"Hot\",\"conditions\":{\">\":[{\"var\":\"temp\"},\"300\"]}},\"FS222\":{\"true_queue_sid\":\"WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\",\"name\":\"Mild\",\"conditions\":{\"!=\":[\"1\",\"1\"]}}}}",
                )            
                .create();

            System.out.println(wf);
        }
    }



**Phyton**

.. code-block::  python


    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # create configuration
    configuration = {
        "first_step": "FS111",
        "filters": {
            "FS111": {
                "true_queue_sid":"WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "false_filter_sid":"FS222",
                "name":"Hot",
                "conditions": {
                    ">":[{"var":"temp"},"300"]
                }
            },
            "FS222":{
                "true_queue_sid":"WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "name":"Mild",
                "conditions": {
                    "!=" : ["1","1"]
                }
            }
        }
    }

    # Create a workflow
    workflow = workspace.workflows.create("My Workflow",
                                        str(configuration),
                                        assignment_callback_url="https://assignment-callback.my-backbone.mydomain.org")

    # Print workflow content
    print("WorkflowSid: {}".format(workflow.sid))
    print("FriendlyName: {}".format(workflow.friendly_name))



**JS**

.. code-block::  javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Workflow configuration
    var configuration = {
        "first_step": "FS111",
        "filters": {
            "FS111": {
                "true_queue_sid":"WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "false_filter_sid":"FS222",
                "name":"Hot",
                "conditions": {
                    ">":[{"var":"temp"},"300"]
                }
            },
            "FS222":{
                "true_queue_sid":"WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "name":"Mild",
                "conditions": {
                    "!=" : ["1","1"]
                }
            }
        }
    }

    // Crate a workflow
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .workflows
    .create({
        friendlyName: "Friendly Name",
        configuration: JSON.stringify(configuration),
        assignmentCallbackUrl: "https://assignment-callback.my-backbone.mydomain.org"
    }, function(err, workflow) {
        console.log(workflow.sid);
        console.log(workflow.friendlyName);
    });



**C#**

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

            // Create a configuration
            var configuration = JsonConvert.SerializeObject(new Dictionary<string, Object>()
            {
                {"first_step", "FS111"},
                {"filters",
                    new Dictionary<string, Object>()
                    {
                        {
                            "FS111", new Dictionary<string, Object>()
                            {
                                { "true_queue_sid", "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"},
                                { "false_filter_sid", "FS222"},
                                { "name", "Hot"},
                                { "conditions",
                                    new Dictionary<string, Object>() {
                                        { ">",
                                            new object [] {
                                                new Dictionary<string, Object>() { { "var", "temp" } }, 300 }
                                        }
                                    }
                                }
                            }
                        },
                        {
                            "FS222", new Dictionary<string, Object>()
                            {
                                { "true_queue_sid", "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" },
                                { "name", "Mild" },
                                { "conditions",
                                    new Dictionary<string, Object>() {
                                        { "!=",
                                            new object [] { "1", "1" }
                                        }
                                    }
                                }
                            }
                        }
                    }
                }
            }, Formatting.None);

            // Create a workflow
            var workflow = WorkflowResource.Create(
                assignmentCallbackUrl: new Uri("https://example.com/"),
                fallbackAssignmentCallbackUrl: new Uri("https://example2.com/"),
                friendlyName: "New Friendly Name",
                configuration: configuration,
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
            );

            // Print workflow content
            Console.WriteLine("WorkflowSID   : " + workflow.Sid);
            Console.WriteLine("Friendly Name : " + workflow.FriendlyName);

        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "friendly_name": "WF1-yesno",
    "configuration": {
        "first_step": "FS111",
        "filters": {
        "FS111": {
            "true_queue_sid": "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "false_filter_sid": "FS222",
            "name": "Hot",
            "conditions": {
            ">": [
                {
                "var": "temp"
                },
                "300"
            ]
            }
        },
        "FS222": {
            "true_queue_sid": "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "name": "Mild",
            "conditions": {
            "!=": [
                "1",
                "1"
            ]
            }
        }
        }
    },
    "task_reservation_timeout": 86400,
    "assignment_callback_url": "https://assignment-callback.my-backbone.mydomain.org",
    "date_created": "2020-05-06T10:13:34+03:00",
    "date_updated": "2020-05-06T10:13:34+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WS2b948ebc07d14f5388eecbc92139e4c7/Workflows/WWc6187bd0d24a4365bc01ee340d3f7010",
    "links": {
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WS2b948ebc07d14f5388eecbc92139e4c7"
    }
    }




Get All Workflows
---------------------

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Workflows`

This endpoint retrives all Workflows

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
     - The SID of the Workspace 
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
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell


    curl -X GET https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workflows \
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"



**Java**

.. code-block::  java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.Workflow;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        

        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Workflow.Reader reader = Workflow.reader("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx");

            for(Workflow wf:reader.read())
            System.out.println(wf);
        }
    }



**Phyton**

.. code-block::  python


    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Fetch all workflow and print content
    workflow_fetcher = workspace.workflows.list(limit=10, page_size=5)
    for workflow in workflow_fetcher:
        print("WorkflowSid: {}".format(workflow.sid))
        print("FriendlyName: {}".format(workflow.friendly_name))



**JS**

.. code-block::  javascript


    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // List all workflows
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .workflows
    .list({
        limit: 100,
        pageSize: 100
    }, function(err, workflows) {
        workflows.forEach(function(workflow) {
        console.log(workflow.sid);
        console.log(workflow.friendlyName);
        })
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

            // Fetch all workflows
            var workflows = WorkflowResource.Read(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                limit: 100,
                pageSize: 100
            );

            // Iterate on workflows
            foreach (var workflow in workflows)
            {
                // Print workflow content
                Console.WriteLine("WorkflowSID   : " + workflow.Sid);
                Console.WriteLine("Friendly Name : " + workflow.FriendlyName);
            }
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "meta": {
        "page_size": 50,
        "page": 0,
        "first_page_url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workflows?Page=0&PageSize=50",
        "previous_page_url": null,
        "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workflows?Page=0&PageSize=50",
        "key": "workflows",
        "next_page_url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workflows?Page=1&PageSize=50"
    },
    "workflows": [
        {
        "sid": "WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "friendly_name": "my test workflow",
        "configuration": {
            "first_step": "FS111",
            "filters": {
            "FS111": {
                "true_queue_sid": "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "false_filter_sid": "FS222",
                "name": "Hot",
                "conditions": {
                ">": [
                    {
                    "var": "temp"
                    },
                    "300"
                ]
                }
            },
            "FS222": {
                "true_queue_sid": "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "name": "Mild",
                "conditions": {
                "!=": [
                    "1",
                    "1"
                ]
                }
            }
            }
        },
        "task_reservation_timeout": 86400,
        "assignment_callback_url": "https://assignment-callback.my-backbone.mydomain.org",
        "date_created": "2020-05-06T10:13:34+03:00",
        "date_updated": "2020-05-06T10:13:34+03:00",
        "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WS2b948ebc07d14f5388eecbc92139e4c7/Workflows/WWc6187bd0d24a4365bc01ee340d3f7010",
        "links": {
            "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WS2b948ebc07d14f5388eecbc92139e4c7"
        }
        }
    ]
    }




Fetch a Workflow
--------------------

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSid}/Workflows/{WorkflowSID}

This endpoint fetches a single Workflow with all Its details


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
     - The SID of the Workspace 
   * - **WorkflowSID**
     - String
     - ""
     - The SID of the Workflow



Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X GET https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Workflows/WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"




**Java**

.. code-block::  java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.Workflow;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Workflow wf = Workflow
                .fetcher("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .fetch();

            System.out.println(wf);
        }
    }



**Phyton**

.. code-block::  python

    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Get workflow with sid and print content
    workflow = workspace.workflows.get("WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").fetch()
    print("WorkflowSid: {}".format(workflow.sid))
    print("FriendlyName: {}".format(workflow.friendly_name))



**JS**

.. code-block::  javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Get a workflows with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .workflows("WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .fetch(function(err, workflow) {
        console.log(workflow.sid);
        console.log(workflow.friendlyName);
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

            // Get a workflow with SID
            var workflow = WorkflowResource.Fetch(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
            );

            // Print workflow content
            Console.WriteLine("WorkflowSID   : " + workflow.Sid);
            Console.WriteLine("Friendly Name : " + workflow.FriendlyName);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "friendly_name": "my test workflow",
    "configuration": {
        "first_step": "FS111",
        "filters": {
        "FS111": {
            "true_queue_sid": "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "false_filter_sid": "FS222",
            "name": "Hot",
            "conditions": {
            ">": [
                {
                "var": "temp"
                },
                "300"
            ]
            }
        },
        "FS222": {
            "true_queue_sid": "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "name": "Mild",
            "conditions": {
            "!=": [
                "1",
                "1"
            ]
            }
        }
        }
    },
    "task_reservation_timeout": 86400,
    "assignment_callback_url": "https://assignment-callback.my-backbone.mydomain.org",
    "date_created": "2020-05-06T10:13:34+03:00",
    "date_updated": "2020-05-06T10:13:34+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workflows/WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
    }




Update a Workflow
----------------------

.. http:put:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Workflows/{WorkflowSID}

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
     - The SID of the Workspace 
   * - **WorkflowSID**
     - String
     - ""
     - The SID of the Workflow
   * - **FriendlyName**
     - String
     - ""
     - (optional) Human readable friendly name. It can be 512 characters long 
   * - **AssignmentCallbackUrl**
     - URL
     - ""
     - (optional) The URL that we call when a task managed by the Workflow is assigned to a Worker.
   * - **FallbackAssignmentCallbackUrl**
     - URL
     - ""
     - (optional) The URL that we call when a call to the assignment_callback_url fails.
   * - **Configuration**
     - String
     - ""
     - (optional) A URL-encoded JSON string that contains the Workflow's configuration. For more information , see `Workflows <https://rindap.com/workflow/>`_ .
   * - **TaskReservationTimeout**
     - Integer
     - -
     - (optional) How long Rindap will wait for a confirmation response from your application after notifying the `assignment_callback_url` about the reservation.


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X PUT https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Workflows/WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \  
    --data-urlencode 'FriendlyName=my newly named workflow' \
    --data-urlencode 'TaskReservationTimeout=3600' \
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"




**Java**

.. code-block::  java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.Workflow;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Workflow wf = Workflow
                .updater("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .setFriendlyName("my newly named workflow")
                .setTaskReservationTimeout(3600)            
                .update();

            System.out.println(w);
        }
    }



**Phyton**

.. code-block::  python

    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Update Workflow
    updated_workflow = workflow.update("New Workflow Name", task_reservation_timeout=3600)

    print("WorkflowSid: {}".format(updated_workflow.sid))
    print("FriendlyName: {}".format(updated_workflow.friendly_name))



**JS**

.. code-block::  javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Update a workflows with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .workflows("WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .update({
        friendlyName: "New Workflow Name",
        taskReservationTimeout: 3600
    },function(err, workflow) {
        console.log(workflow.sid);
        console.log(workflow.friendlyName);
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

            // Update a workflow
            var workflow = WorkflowResource.Update(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                assignmentCallbackUrl: new Uri("https://example.org"),
                friendlyName: "New workflow name"
            );

            // Print workflow content
            Console.WriteLine("WorkflowSID   : " + workflow.Sid);
            Console.WriteLine("Friendly Name : " + workflow.FriendlyName);
            Console.WriteLine("Callback URI  : " + workflow.AssignmentCallbackUrl);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "friendly_name": "my newly named workflow",
    "configuration": {
        "first_step": "FS111",
        "filters": {
        "FS111": {
            "true_queue_sid": "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "false_filter_sid": "FS222",
            "name": "Hot",
            "conditions": {
            ">": [
                {
                "var": "temp"
                },
                "300"
            ]
            }
        },
        "FS222": {
            "true_queue_sid": "WQxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "name": "Mild",
            "conditions": {
            "!=": [
                "1",
                "1"
            ]
            }
        }
        }
    },
    "task_reservation_timeout": 3600,
    "assignment_callback_url": "https://assignment-callback.my-backbone.mydomain.org",
    "date_created": "2020-05-06T10:13:34+03:00",
    "date_updated": "2020-05-06T10:13:34+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workflows/WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
    }





Delete a Workflow
---------------------

.. http:delete::  /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Workflows/{WorkflowSID}

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
     - The SID of the Workspace 
   * - **WorkflowSID**
     - String
     - ""
     - The SID of the Workflow


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X DEL https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Workflows/WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block::  java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.Workflow;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Workflow.deleter("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .delete();
        }
    }


**Phyton**

.. code-block::  python

    from rindap.rest import Client
    from rindap.rest import Rindap

    # Authenticate
    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    # Get Workspace with workspace_sid
    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")

    # Delete workflow with sid
    if workspace.workflows.get("WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").delete():
        print("Workflow has been deleted!")


**JS**

.. code-block::  javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Delete a workflows with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .workflows("WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
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

            // Update a workflows workflow
            var isDeleted = WorkflowResource.Delete(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
            );

            if (isDeleted)
            {
                Console.WriteLine("Workflow has been deleted!");
            }
        }
    }


