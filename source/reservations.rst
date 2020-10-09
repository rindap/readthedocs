
*********************
Reservations
*********************

Rindap creates a Reservation subresource whenever a Task is reserved for a Worker. Rindap will provide the details 
of this Reservation subresource in the Assignment Callback HTTP request it makes to your application server. You can read more about `Reservations here <https://rindap.com/reservation/>`_

You have multiple options for handling a Reservation:

* Respond to the Assignment Callback with an Assignment Instruction.
* Call the REST API with how to handle it.

You can read more about `Reservations here <https://rindap.com/reservation/>`_


Reservation Properties
-----------------------

.. list-table:: Properties
   :widths: 25 50
   :header-rows: 1

   * - field
     - description
   * - **sid**
     - The unique string that we created to identify the Reservation resource.
   * - **account_sid**
     - The SID of the Account
   * - **workspace_sid**
     - The SID of the Workspace
   * - **worker_sid**
     - The SID of the reserved Worker resource
   * - **task_sid**
     - The SID of the reserved Task resource
   * - **reservation_status**
     - The current status of the reservation. Can be: pending, accepted, rejected.
   * - **date_created**
     - The date and time in GMT when the resource was created, specified in ISO 8601 format.
   * - **date_updated**
     - The date and time in GMT when the resource was last updated, specified in ISO 8601 format.
   * - **url**
     - The absolute URL of the RateLimitProfile resource
   * - **links**
     - The URLs of related resources.


Accessing Reservations
^^^^^^^^^^^^^^^^^^^^^^^^^^^

There are 3 ways to access a Reservation resource:

* For accessing a certain Reservation with Its SID , You can access It through Its Task or Worker.
* For accessing all Reservations of a certain Task, you need to access through the Task
* For accessing all Reservations of a certain Worker, you need to access through the Worker 


Through a Task: Fetching a Reservation
---------------------------------------

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Tasks/{TaskSID}/Reservations/{ReservationSID}

This endpoint fetches a single Reservation with all its details

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
   * - **TaskSID**
     - String
     - -
     - The SID of the Task
   * - **ReservationSID**
     - String
     - -
     - The SID of the Reservation


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X GET https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations/WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    // IMPORTING from com.rindap.rest.v1.workspace.task package
    import com.rindap.rest.v1.workspace.task.Reservation;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Reservation r=Reservation.fetcher(
            "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", 
            "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", // Task SID
            "WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
            .fetch();

            System.out.println(r);
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

    # Get Task with SID
    task = workspace.tasks.get("WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").fetch()

    # Get reservation with SID
    reservation = task.reservations.get("WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").fetch()
    print("Reservation SID: {}".format(reservation.sid))
    print("Reservation Status: {}".format(reservation.reservation_status))


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Get a reservation with task and reservation SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .tasks("WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .reservations("WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .fetch(function(err, reservation) {
        console.log(reservation.sid);
        console.log(reservation.reservationStatus);
    });


**C#**

.. code-block:: csharp

    using System;
    using Rindap;
    using Rindap.Rest.V1.Workspace.Task;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Fetch a reservation with SID
            var reservation = ReservationResource.Fetch(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathTaskSid: "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
                );

            // Print Reservation Content
            Console.WriteLine("Reservation SID   : " + reservation.Sid);
            Console.WriteLine("Reservation Status: " + reservation.ReservationStatus);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "worker_sid": "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "task_sid": "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "reservation_status": "rejected",
    "date_created": "2020-04-23T13:47:15+03:00",
    "date_updated": "2020-04-23T13:49:42+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations/WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
        "task": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "worker": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
    }



Through a Task: Listing All Reservations of A Task
------------------------------------------------------

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Tasks/{TaskSID}/Reservations

This endpoint retrives all Reservations of a Task

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
   * - **TaskSID**
     - String
     - -
     - The SID of the Task
   * - **ReservationStatus**
     - String
     - -
     - (optional) The current status of the reservation. Can be: pending, accepted, rejected, completed. Can be used for filtering
   * - **WorkerSid**
     - String
     - -
     - (optional) The SID of the reserved Worker. Can be used to filter the Reservations by Worker




Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X GET https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    // IMPORTING from com.rindap.rest.v1.workspace.task package
    import com.rindap.rest.v1.workspace.task.Reservation;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        

        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Reservation.Reader reader = Reservation.reader(
            "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
            .setReservationStatus(Reservation.Status.ACCEPTED)
            ;

            for(Reservation r:reader.read())
            System.out.println(r);
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

    # Get Task with SID
    task = workspace.tasks.get("WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").fetch()

    # Get all reservation
    reservations = task.reservations.list(limit=10, page_size=100)
    for reservation in reservations:
        # Print Content of reservation
        print("Reservation SID: {}".format(reservation.sid))
        print("Reservation Status: {}".format(reservation.reservation_status))


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // List all reservations
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .tasks("WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .reservations
    .list({
        limit: 100,
        pageSize: 100
    },function(err, reservations) {
        reservations.forEach(function(reservation) {
        console.log(reservation.sid);
        console.log(reservation.reservationStatus);
        });
    });


**C#**

.. code-block:: csharp

    using System;
    using Rindap;
    using Rindap.Rest.V1.Workspace.Task;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Get all reservations
            var reservations = ReservationResource.Read(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathTaskSid: "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                limit: 100,
                pageSize: 100
                );

            foreach (var reservation in reservations)
            {
                // Print Reservation Content
                Console.WriteLine("Reservation SID   : " + reservation.Sid);
                Console.WriteLine("Reservation Status: " + reservation.ReservationStatus);
            }
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "meta": {
        "page_size": 2147483647,
        "page": 0,
        "first_page_url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations?Page=0&PageSize=2147483647",
        "previous_page_url": null,
        "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations?Page=0&PageSize=2147483647",
        "key": "",
        "next_page_url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations?Page=1&PageSize=2147483647"
    },
    "reservations": [
        {
            "sid": "WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "worker_sid": "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "task_sid": "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "reservation_status": "rejected",
            "date_created": "2020-04-23T13:47:15+03:00",
            "date_updated": "2020-04-23T13:49:42+03:00",
            "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations/WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "links": {
                "task": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "worker": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
            }
        }
    ]
    }



Through a Task: Updating a reservation
----------------------------------------

.. http:put:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Tasks/{TaskSID}/Reservations/{ReservationSID}

.. note:: You can only update a Reservation while It's at the `pending` status

You can update a Reservation by updating Its ``ReservationStatus``. 

* **accepted** - use this for accepting a reservation, which means that the Worker will process the Task
* **rejected** - use this for rejecting a reservation.At this situation, Rindap will reject the Reservation and try to assing the Task to another available Worker
* **completed** - This is a special status setting, a shortcut, for saying that the Reservation is accepted and the Task is completed , or can be considered completed. 
    After receiving this status, 
        * the Reservation will be updated as `accepted` 
        * the Task will be updated as `completed`
        * the Worker will be updated as `idle` and will be availabe for Reservations

    This is useful for Workers with short-lived functions such as a WebService endpoint where your application does whatever needed to be done, instantaneously


.. list-table:: Query Parameters
   :widths: 20 15 5 60
   :header-rows: 1

   * - Parameter
     - Type
     - Default
     - Description
   * - **WorkspaceSID**
     - String
     - -
     - The SID of the `Workspace`
   * - **TaskSID**
     - String
     - -
     - The SID of the Task
   * - **ReservationStatus**
     - String
     - -
     - the status of the reservation. Can be "accepted","rejected" or "completed"
   * - **ReservationSID**
     - String
     - -
     - The SID of the Reservation


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X PUT https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations/WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \  
    --data-urlencode 'ReservationStatus=accepted' \
    --data-urlencode 'AssignmentStatus=pending' \
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"



**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    // IMPORTING from com.rindap.rest.v1.workspace.task package
    import com.rindap.rest.v1.workspace.task.Reservation;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Reservation r=Reservation
            .updater(
            "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", // Task SID
            "WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
            .setReservationStatus(Reservation.Status.ACCEPTED)
            .update();

            System.out.println(r);
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

    # Get Task with SID
    task = workspace.tasks.get("WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").fetch()

    # Get reservation with SID
    reservation = task.reservations.get("WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").fetch()

    updated_reservation = reservation.update(reservation_status='rejected')
    print("Reservation SID: {}".format(updated_reservation.sid))
    print("Reservation Status: {}".format(updated_reservation.reservation_status))


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Update a reservation with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .tasks("WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .reservations("WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .update({
        reservationStatus: 'rejected'
    },function(err, reservation) {
        console.log(err);
        console.log(reservation.sid);
    });


**C#**

.. code-block:: csharp

    using System;
    using Rindap;
    using Rindap.Rest.V1.Workspace.Task;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Fetch a reservation with SID
            var reservation = ReservationResource.Update(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathTaskSid: "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                reservationStatus: ReservationResource.StatusEnum.Rejected
                );

            // Print Reservation Content
            Console.WriteLine("Reservation SID   : " + reservation.Sid);
            Console.WriteLine("Reservation Status: " + reservation.ReservationStatus);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "worker_sid": "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "task_sid": "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "reservation_status": "rejected",
    "date_created": "2020-04-23T13:47:15+03:00",
    "date_updated": "2020-04-23T13:49:42+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations/WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
        "task": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "worker": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
    }


Through a Worker: Fetching a Reservation
--------------------------------------------

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Workers/{WorkerSID}/Reservations/{ReservationSID}

This endpoint fetches a single Reservation with all its details

.. list-table:: Query Parameters
   :widths: 20 15 5 60
   :header-rows: 1

   * - Parameter
     - Type
     - Default
     - Description
   * - **WorkspaceSID**
     - String
     - -
     - The SID of the `Workspace`
   * - **WorkerSID**
     - String
     - -
     - The SID of the Worker
   * - **ReservationSID**
     - String
     - -
     - The SID of the Reservation


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X GET https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations/WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;

    // IMPORTING from com.rindap.rest.v1.workspace.worker package
    import com.rindap.rest.v1.workspace.worker.Reservation;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Reservation r=Reservation.fetcher(
            "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", 
            "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", // Worker SID
            "WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
            .fetch();

            System.out.println(r);
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

    # Get Worker with SID
    worker = workspace.workers.get("WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").fetch()

    # Get reservation with SID
    reservation = worker.reservations.get("WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").fetch()
    print("Reservation SID: {}".format(reservation.sid))
    print("Reservation Status: {}".format(reservation.reservation_status))


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Get a reservation with worker and reservation SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .workers("WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .reservations("WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .fetch(function(err, reservation) {
        console.log(reservation.sid);
    });


**C#**

.. code-block:: csharp

    using System;
    using Rindap;
    using Rindap.Rest.V1.Workspace.Worker;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Fetch a reservation with SID
            var reservation = ReservationResource.Fetch(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathWorkerSid: "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
                );

            // Print Reservation Content
            Console.WriteLine("Reservation SID   : " + reservation.Sid);
            Console.WriteLine("Reservation Status: " + reservation.ReservationStatus);
        }
    }



**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "worker_sid": "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "task_sid": "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "reservation_status": "rejected",
    "date_created": "2020-04-23T13:47:15+03:00",
    "date_updated": "2020-04-23T13:49:42+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations/WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
        "task": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "worker": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
    }



Through a Worker: Listing All Reservations of A Worker
----------------------------------------------------------

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Workers/{WorkerSID}/Reservations

This endpoint retrives all Reservations of a Worker

.. list-table:: Query Parameters
   :widths: 20 15 5 60
   :header-rows: 1

   * - Parameter
     - Type
     - Default
     - Description
   * - **WorkspaceSID**
     - String
     - -
     - The SID of the `Workspace`
   * - **WorkerSID**
     - String
     - -
     - The SID of the Worker
   * - **ReservationStatus**
     - String
     - -
     - (optional) The current status of the reservation. Can be: pending, accepted, rejected. Can be used for filtering


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X GET https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    //IMPORTING Reservation from com.rindap.rest.v1.workspace.worker
    import com.rindap.rest.v1.workspace.worker.Reservation;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        

        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Reservation.Reader reader = Reservation.reader(
            "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx");

            for(Reservation r:reader.read())
            System.out.println(r);
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

    # Get Worker with SID
    worker = workspace.workers.get("WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").fetch()

    # Get all reservation
    reservations = worker.reservations.list(limit=10, page_size=100)
    for reservation in reservations:
        # Print Content of reservation
        print("Reservation SID: {}".format(reservation.sid))
        print("Reservation Status: {}".format(reservation.reservation_status))


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // List all reservations
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .workers("WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .reservations
    .list({
        limit: 100,
        pageSize: 100
    },function(err, reservations) {
        reservations.forEach(function(reservation) {
        console.log(reservation.sid);
        console.log(reservation.reservationStatus);
        });
    });


**C#**

.. code-block:: csharp

    using System;
    using Rindap;
    using Rindap.Rest.V1.Workspace.Worker;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // List all reservations
            var reservations = ReservationResource.Read(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathWorkerSid: "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                limit: 100,
                pageSize: 100
                );

            foreach (var reservation in reservations)
            {
                // Print Reservation Content
                Console.WriteLine("Reservation SID   : " + reservation.Sid);
                Console.WriteLine("Reservation Status: " + reservation.ReservationStatus);
            }
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "meta": {
        "page_size": 2147483647,
        "page": 0,
        "first_page_url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations?Page=0&PageSize=2147483647",
        "previous_page_url": null,
        "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations?Page=0&PageSize=2147483647",
        "key": "",
        "next_page_url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations?Page=1&PageSize=2147483647"
    },
    "reservations": [
        {
            "sid": "WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "worker_sid": "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "task_sid": "WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "reservation_status": "rejected",
            "date_created": "2020-04-23T13:47:15+03:00",
            "date_updated": "2020-04-23T13:49:42+03:00",
            "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations/WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "links": {
                "task": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Tasks/WTxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "worker": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
            }
        }
    ]
    }



Through a Worker: Updating a reservation
-----------------------------------------------

.. http:put:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/Workers/{WorkerSID}/Reservations/{ReservationSID}

.. note:: You can only update a Reservation while It's at the ``pending`` status

You can update a Reservation by updating Its ``ReservationStatus``. 

* **accepted** - use this for accepting a reservation, which means that the Worker will process the Task
* **rejected** - use this for rejecting a reservation.At this situation, Rindap will reject the Reservation and try to assing the Task to another available Worker
* **completed** - This is a special status setting, a shortcut, for saying that the Reservation is accepted and the Task is completed , or can be considered completed. 
    After receiving this status, 
        * the Reservation will be updated as `accepted` 
        * the Task will be updated as `completed`
        * the Worker will be updated as `idle` and will be availabe for Reservations

    This is useful for Workers with short-lived functions such as a WebService endpoint where your application does whatever needed to be done, instantaneously


.. list-table:: Query Parameters
   :widths: 20 15 5 60
   :header-rows: 1

   * - Parameter
     - Type
     - Default
     - Description
   * - **WorkspaceSID**
     - String
     - -
     - The SID of the `Workspace`
   * - **WorkerSID**
     - String
     - -
     - The SID of the Worker
   * - **ReservationStatus**
     - String
     - -
     - the status of the reservation. Can be "accepted","rejected" or "completed"
   * - **ReservationSID**
     - String
     - -
     - The SID of the Reservation


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X PUT https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /Workers/WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/Reservations/WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \  
    --data-urlencode 'ReservationStatus=accepted' \
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"



**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    //IMPORTING Reservation from package com.rindap.rest.v1.workspace.worker
    import com.rindap.rest.v1.workspace.worker.Reservation;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            Reservation r=Reservation
            .updater(
            "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", // Worker SID
            "WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
            .setReservationStatus(Reservation.Status.ACCEPTED)
            .update();

            System.out.println(r);
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

    # Get Worker with SID
    worker = workspace.workers.get("WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").fetch()

    # Get reservation with SID
    reservation = worker.reservations.get("WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx").fetch()

    # Update Reservation
    updated_reservation = reservation.update(reservation_status='rejected')
    print("Reservation SID: {}".format(updated_reservation.sid))
    print("Reservation Status: {}".format(updated_reservation.reservation_status))


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // UPdate a reservation with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .workers("WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .reservations("WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .update({
        reservationStatus: 'rejected'
    },function(err, reservation) {
        console.log(err);
        console.log(reservation.sid);
    });


**C#**

.. code-block:: csharp

    using System;
    using Rindap;
    using Rindap.Rest.V1.Workspace.Worker;

    class Program
    {
        static void Main(string[] args)
        {
            // Authenticate
            RindapClient.Init("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

            // Update a reservation with SID
            var reservation = ReservationResource.Update(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathWorkerSid: "WKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "WRxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                reservationStatus: ReservationResource.StatusEnum.Completed
                );

            // Print Reservation Content
            Console.WriteLine("Reservation SID   : " + reservation.Sid);
            Console.WriteLine("Reservation Status: " + reservation.ReservationStatus);
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


