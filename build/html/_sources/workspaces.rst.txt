
***********
Workspaces
***********


Rindap (Process) Workspace is a customizable place enabling you to create your Tasks, Workers, TaskQueues and Workflows 
elements in it to access and manage your business processes. The elements defined in one Workspace are specific to it 
and cannot be shared with other Workspace.
 
Various tasks with different attributes, specific and modifiable workflow, various skilled workers can be 
defined in one Workspace to orchestrate your business processes efficiently in the lowest time. Business Process Workspace 
helps organizations to improve their internal processes management by providing a single container to manage, control and 
monitor the whole elements needed in one place. With Rindap, managing the inner operational processes of a company 
in a purpose-built Workspace and defining your strategies and policies for the company in a single Workspace can be done. 

Workspace Properties
----------------------


.. list-table:: Properties
   :widths: 25 50
   :header-rows: 1

   * - field
     - description
   * - **sid**
     - The unique string that we created to identify the Workspace resource.
   * - **account_sid**
     - The SID of the Account that created the Workspace resource.
   * - **friendly_name**
     - The string that you assigned to describe the resource. 
   * - **event_callback_url**
     - The URL we call when an event occurs. If provided, the Workspace will publish events to this URL, for example, to collect data for reporting. See Workspace Events for more information.
   * - **event_callback_method**
     - The HTTP Request Method for calling the EventCallbackUrl
   * - **default_activity**
     - The Activity that will be used when new Workers are created in the Workspace.
   * - **timeout_activity**
     - The Activity that will be assigned to a Worker when a Task reservation times out without a response 
   * - **date_created**
     - The date and time in GMT when the resource was created, specified in ISO 8601 format.
   * - **date_updated**
     - The date and time in GMT when the resource was last updated, specified in ISO 8601 format.
   * - **url**
     - The absolute URL of the resource
   * - **links**
     - The URLs of related resources.


Create A Workspace
--------------------

.. http:post:: /v1/rindap-rest-gw/Workspaces/

You can create a Workspace by simply providing a friendly name

.. list-table:: Query Parameters
   :widths: 20 15 5 60
   :header-rows: 1

   * - Parameter
     - Type
     - Default
     - Description
   * - **FriendlyName**
     - String
     - ""
     - A descriptive string that you create to describe the Workspace resource. It can be up to 512 characters long
   * - **EventCallbackUrl**
     - URL
     - ""
     - (Optional) The URL we call when an event occurs. If provided, the Workspace will publish events to this URL, for example, to collect data for reporting. See Workspace Events for more information.
   * - **EventCallbackMethod**
     - String
     - POST
     - (Optional) The HTTP Request Method for calling the EventCallbackUrl
   * - **DefaultActivity**
     - String
     - offline
     - (Optional) The Activity that will be used when new Workers are created in the Workspace.
   * - **TimeoutActivity**
     - String
     - offline
     - (Optional) The Activity that will be assigned to a Worker when a Task reservation times out without a response 


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X POST https://api.rindap.com/v1/rindap-rest-gw/Workspaces \
    --data-urlencode 'FriendlyName=my test workspace' \
    --data-urlencode 'EventCallbackUrl=https://my-backbone.mydomain.org' \
    --data-urlencode 'EventCallMethod=GET' \
    --data-urlencode 'DefaultActivity=busy' \ 
    --data-urlencode 'TimeoutActivity=offline'
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"

**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.Task;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Workspace ws = Workspace.creator("my test workspace")
                .setEventCallbackUrl("https://my-backbone.mydomain.org")
                .setEventCallbackMethod(HttpMethod.GET)
                .setDefaultActivity("busy")
                .setTimeoutActivity("idle)
                .create();

            System.out.println(ws);
        }
    }


**Phyton**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    ws = rindap.workspaces.create("my test workspace",
                                event_callback_url="https://my-backbone.mydomain.org",
                                event_callback_method="GET",
                                default_activity="busy",
                                timeout_activity="idle")

    print("FriendlyName: {}".format(ws.friendly_name))


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Create a workspace
    rindap.workspaces.create({
    friendlyName: "Friendly Name",
    eventCallbackUrl: "https://my-backbone.mydomain.org",
    eventCallbackMethod: "GET",
    defaultActivity: "busy",
    timeoutActivity: "idle"
    }, function(err, workspace) {
    // Print workspace content
    console.log('Workspace Created');
    console.log(workspace.sid);
    console.log(workspace.friendlyName);
    console.log(workspace.eventCallbackUrl);
    console.log(workspace.eventCallbackMethod);
    console.log(workspace.defaultActivity);
    console.log(workspace.timeoutActivity);
    });


**CSharp**

.. code-block:: csharp

    using System;
    using Rindap;
    using Rindap.Rest.V1;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Create a workspace
            WorkspaceResource ws = WorkspaceResource.Create(
                friendlyName: "Very Friendly Name From C#",
                eventCallbackUrl: new Uri("https://my-backbone.mydomain.org"),
                eventCallbackMethod: "GET",
                defaultActivity: "busy",
                timeoutActivity: "offline"
                );

            Console.WriteLine("Workspace Friendly Name        : " + ws.FriendlyName);
            Console.WriteLine("Workspace Sid                  : " + ws.Sid);
            Console.WriteLine("Workspace Default Activity     : " + ws.DefaultActivity);
            Console.WriteLine("Workspace Timeout Activity     : " + ws.TimeoutActivity);
            Console.WriteLine("Workspace Event Calllback Url  : " + ws.EventCallbackUrl);
            Console.WriteLine("Workspace Event callback Method: " + ws.EventCallbackMethod);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
        "sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "friendly_name": "my test workspace",
        "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "default_activity": "busy",
        "timeout_activity": "idle",
        "date_created": "2020-05-04T01:36:02+03:00",
        "date_updated": "2020-05-04T01:36:02+03:00",
        "event_callback_url": "https:my-backbone.mydomain.org",
        "event_callback_method": "GET",
        "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "links": {
            "tasks": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks",
            "workers": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers",
            "workflows": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workflows",
            "task_queues": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/TaskQueues"
        }
    }




Get All Workspaces
----------------------

.. http:get::/v1/rindap-rest-gw/Workspaces

This endpoint retrives all Workspaces

.. list-table:: Query Parameters
   :widths: 20 15 5 60
   :header-rows: 1

   * - Parameter
     - Type
     - Default
     - Description
   * - **FriendlyName**
     - String
     - ""
     - Human readable friendly name
   * - **PageSize**
     - Integer
     - 50
     - Page size for paging
   * - **FriendlyName**
     - Integer
     - 0
     - Page number for paging


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


**CSharp**

.. code-block:: csharp

    using System;
    using Rindap;
    using Rindap.Rest.V1;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Fetch all workspaces
            var workspaces = WorkspaceResource.Read(limit: 100, pageSize: 100);

            // Iterate all workspaces
            foreach (var ws in workspaces)
            {
                // Print workspace content
                Console.WriteLine("Workspace Friendly Name        : " + ws.FriendlyName);
                Console.WriteLine("Workspace Sid                  : " + ws.Sid);
                Console.WriteLine("Workspace Default Activity     : " + ws.DefaultActivity);
                Console.WriteLine("Workspace Timeout Activity     : " + ws.TimeoutActivity);
                Console.WriteLine("Workspace Event Calllback Url  : " + ws.EventCallbackUrl);
                Console.WriteLine("Workspace Event callback Method: " + ws.EventCallbackMethod);
            }
        }
    }


**Phyton**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    lists = rindap.workspaces.list(friendly_name=None, limit=10, page_size=5)
    workspace = lists.pop()
    print(workspace.friendly_name)


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Get all workspaces
    rindap.workspaces.list({
    limit: 100,
    page_size: 100
    }, function(err, result) {
    result.forEach(function(workspace) {

        console.log(workspace.sid);
        console.log(workspace.friendlyName);
        console.log(workspace.eventCallbackUrl);
        console.log(workspace.eventCallbackMethod);
        console.log(workspace.defaultActivity);
        console.log(workspace.timeoutActivity);
    });
    });


**The above command returns JSON structured like this:**


.. code-block:: json

    {
    "meta": {
        "page_size": 50,
        "page": 0,
        "first_page_url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/?Page=0&PageSize=50",
        "previous_page_url": null,
        "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/?Page=0&PageSize=50",
        "key": "workspaces",
        "next_page_url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/?Page=1&PageSize=50"
    },
    "workspaces": [
        {
        "sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "friendly_name": "my test workspace",
        "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "default_activity": "busy",
        "timeout_activity": "idle",
        "date_created": "2020-05-04T01:36:02+03:00",
        "date_updated": "2020-05-04T01:36:02+03:00",
        "event_callback_url": "https:my-backbone.mydomain.org",
        "event_callback_method": "GET",
        "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "links": {
            "tasks": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks",
            "workers": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers",
            "workflows": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workflows",
            "task_queues": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/TaskQueues"
        }
        }    
    ]
    }




Fetch A Workspace
----------------------

This endpoint fetches a single Workspace with all Its details

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}

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


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell


    curl -X GET https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxx
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.Task;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        

        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Workspace ws = Workspace.fetcher("WSxxxxxxxxxxxxxxxxxxxxxxxx")
                .fetch();

            System.out.println(ws);
        }
    }


**Phyton**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    ws_fetcher = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    workspace = ws_fetcher.fetch()
    print(workspace.friendly_name)


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Get a workspaces with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx').fetch(
    function(err, workspace) {
        console.log(err);
        console.log(workspace.sid);
        console.log(workspace.friendlyName);
        console.log(workspace.eventCallbackUrl);
        console.log(workspace.eventCallbackMethod);
        console.log(workspace.defaultActivity);
        console.log(workspace.timeoutActivity);
    });


**CSharp**

.. code-block:: csharp

    using System;
    using Rindap;
    using Rindap.Rest.V1;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Get a workspace with SID
            var ws = WorkspaceResource.Fetch(
                pathSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
                );

            // Print workspace content
            Console.WriteLine("Workspace Friendly Name        : " + ws.FriendlyName);
            Console.WriteLine("Workspace Sid                  : " + ws.Sid);
            Console.WriteLine("Workspace Default Activity     : " + ws.DefaultActivity);
            Console.WriteLine("Workspace Timeout Activity     : " + ws.TimeoutActivity);
            Console.WriteLine("Workspace Event Calllback Url  : " + ws.EventCallbackUrl);
            Console.WriteLine("Workspace Event callback Method: " + ws.EventCallbackMethod);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "friendly_name": "my test workspace",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "default_activity": "busy",
    "timeout_activity": "idle",
    "date_created": "2020-05-04T01:36:02+03:00",
    "date_updated": "2020-05-04T01:36:02+03:00",
    "event_callback_url": "https:my-backbone.mydomain.org",
    "event_callback_method": "GET",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
        "tasks": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks",
        "workers": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers",
        "workflows": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workflows",
        "task_queues": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/TaskQueues"
    }
    }





Update a Workspace
---------------------

.. http:put:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}


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
     - (Optional) A descriptive string that you create to describe the Workspace resource. It can be up to 512 characters long
   * - **EventCallbackUrl**
     - URL
     - ""
     - (Optional) The URL we call when an event occurs. If provided, the Workspace will publish events to this URL, for example, to collect data for reporting. See Workspace Events for more information.
   * - **EventCallbackMethod**
     - String
     - POST
     - (Optional) The HTTP Request Method for calling the EventCallbackUrl
   * - **DefaultActivity**
     - String
     - ""
     - (Optional) The Activity that will be used when new Workers are created in the Workspace.
   * - **TimeoutActivity**
     - String
     - ""
     - (Optional) The Activity that will be assigned to a Worker when a Task reservation times out without a response 


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X PUT https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    --data-urlencode 'FriendlyName=my test workspace updated' \
    --data-urlencode 'EventCallbackUrl=https://my-backbone.my-new-domain.org' \
    --data-urlencode 'EventCallMethod=POST' \
    --data-urlencode 'DefaultActivity=idle' \ 
    --data-urlencode 'TimeoutActivity=busy'
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"



**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.Task;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Workspace ws = Workspace
                .updater("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .setFriendlyName("my test workspace updated")
                .setEventCallbackUrl("https://my-backbone.my-new-domain.org")
                .setEventCallbackMethod("POST")
                .setDefaultActivity("idle")
                .setTimeoutActivity("offline")            
                .update();

            System.out.println(ws);
        }
    }


**Phyton**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    ws_fetcher = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    ws_fetcher.update(friendly_name="New Friendly Name", default_activity="offline",
                    timeout_activity="offline", event_callback_url="https://domain.com",
                    event_callback_method="POST")


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Update a workspaces with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx').update({
    friendlyName: "New Friendly Name",
    eventCallbackUrl: "https://my-backbone.mydomain.com",
    defaultActivity: "offline",
    timeoutActivity: "offline"
    }, function(err, workspace) {
    console.log(workspace.sid);
    console.log(workspace.friendlyName);
    console.log(workspace.eventCallbackUrl);
    console.log(workspace.eventCallbackMethod);
    console.log(workspace.defaultActivity);
    console.log(workspace.timeoutActivity);
    });


**CSharp**

.. code-block:: csharp

    using System;
    using System.ComponentModel.DataAnnotations;
    using Rindap;
    using Rindap.Rest.V1;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Update a workspace with SID
            var ws = WorkspaceResource.Update(
                pathSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                friendlyName: "New Workspace Name",
                eventCallbackUrl: new Uri("https://my-backbone.mydomain.com"),
                eventCallbackMethod: "POST",
                defaultActivity: "busy",
                timeoutActivity: "busy"
                );

            // Print workspace content
            Console.WriteLine("Workspace Friendly Name        : " + ws.FriendlyName);
            Console.WriteLine("Workspace Sid                  : " + ws.Sid);
            Console.WriteLine("Workspace Default Activity     : " + ws.DefaultActivity);
            Console.WriteLine("Workspace Timeout Activity     : " + ws.TimeoutActivity);
            Console.WriteLine("Workspace Event Calllback Url  : " + ws.EventCallbackUrl);
            Console.WriteLine("Workspace Event callback Method: " + ws.EventCallbackMethod);
        }
    }


**The above command returns JSON structured like this:**


.. code-block:: json

    {
    "sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "friendly_name": "my test workspace updated",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "default_activity": "idle",
    "timeout_activity": "offline",
    "date_created": "2020-05-04T01:36:02+03:00",
    "date_updated": "2020-05-04T01:36:02+03:00",
    "event_callback_url": "https:my-backbone.my-new-domain.org",
    "event_callback_method": "POST",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "links": {
            "tasks": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks",
            "workers": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers",
            "workflows": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workflows",
            "task_queues": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/TaskQueues"
        }
    }


Delete a Workspace
-----------------------

.. http:delete:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}

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


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X DEL https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.Task;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Workspace.deleter("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").delete();
        }
    }


**Phyton**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    ww_fetcher = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    if ww_fetcher.delete():
        print("Workspaces have been deleted!")


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Delete a workspaces with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx').remove();


**CSharp**

.. code-block:: csharp

    using System;
    using System.ComponentModel.DataAnnotations;
    using Rindap;
    using Rindap.Rest.V1;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Delete a workspace with SID
            var isDeleted = WorkspaceResource.Delete(
                pathSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
                );

            if (isDeleted)
            {
                Console.WriteLine("Workspace has been deleted!");
            }
        }
    }


