.. _ratelimitprofiles:

*********************
Rate Limit Profiles
*********************

RateLimitProfiles define a limit for the maximum number of Reservations that can be created Hourly. 

When a Worker has a ``rate_limit_profile``  set, you guarantee that the Worker will not be assigned more than 
a certain number of Tasks **hourly**, regardless of the Task's nature , or which Workflow It's processed through 
or which TaskQueue it comes from. You can read more about `Rate Limits and Rate Limit Profiles here <https://rindap.com/rate-limit/>`_ .

Using Rate Limit Profiles helps you group similar Workers together and change their pace of Task consumption from a 
single point of setting. For example , a Call Center may set a rate limit profile for all 
the Workers in the support department, which lets them all have 12 calls per hour in the morning session , and then 
update the Rate Limit Profile and let them have 6 calls per hour in the afternoon session, halving their workload 
and redirecting it to some other department, with a single Rate Limit Profile update.

* With utilizing Rate Limit Profiles , you can :
    * change the pace of Task consumption  or
    * enforce quotas on limited resources and budgets or
    * prevent bottlenecks on your application side 

You can read more about `Rate Limits and Rate Limit Profiles here <https://rindap.com/rate-limit/>`_ 
    

RateLimitProfile Properties
---------------------------------

.. list-table:: Properties
   :widths: 25 50
   :header-rows: 1

   * - field
     - description
   * - **sid**
     - The unique string that we created to identify the Rate Limit Profile resource.
   * - **account_sid**
     - The SID of the Account that created the RateLimitProfile resource.
   * - **workspace_sid**
     - The SID of the Workspace that contains the RateLimitProfile
   * - **friendly_name**
     - The string that you assigned to describe the resource. 
   * - **reservations_per_hour**
     - The maximum number of Reservations that can be created hourly, for a Worker with this RateLimitProfile
   * - **date_created**
     - The date and time in GMT when the resource was created, specified in ISO 8601 format.
   * - **date_updated**
     - The date and time in GMT when the resource was last updated, specified in ISO 8601 format.
   * - **url**
     - The absolute URL of the RateLimitProfile resource
   * - **links**
     - The URLs of related resources.



Create A RateLimitProfile
-----------------------------

.. http:post:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/RateLimitProfiles

You can easily create a RateLimitProfile by providind a friendly name and the number of maximum hourly reservations

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
   * - **ReservationsPerHour**
     - Integer
     - ""
     - The maximum number of Reservations that can be created hourly, for a Worker with this RateLimitProfile




Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X POST https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/RateLimitProfiles \
    --data-urlencode 'FriendlyName=my rate limit profile for 6 reservations an Hour' 
    --data-urlencode 'ReservationsPerHour=6' 
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.RateLimitProfile;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console

        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            RateLimitProfile rl = RateLimitProfile.creator("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                "my rate limit profile for 6 reservations an Hour",
                6
                )            
                .create();

            System.out.println(rl);
        }
    }


**Python**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    workspace = rindap.workspaces.get("WSb9d8cf8597f64f77a45666c4c0263862")
    rtp = workspace.rate_limit_profiles.create("My Rate Limit", reservation_per_hour=7)
    print(rtp)


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Crate a reservation
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .rateLimitProfiles
    .create({
        friendlyName: 'New Rate Limit',
        reservationsPerHour: 4
    }, function(err, rateLimitProfile) {
        console.log(rateLimitProfile.sid);
        console.log(rateLimitProfile.friendlyName);
        console.log(rateLimitProfile.reservationsPerHour);
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

            // Create a RateLimitProfile
            var rateLimitProfile = RateLimitProfileResource.Create(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                friendlyName: "New RateLimitProfile",
                reservationsPerHour: 7
                );

            // Print RateLimitProfile Content
            Console.WriteLine("SID               : " + rateLimitProfile.Sid);
            Console.WriteLine("FriendlyName      : " + rateLimitProfile.FriendlyName);
            Console.WriteLine("ReservationPerHour: " + rateLimitProfile.ReservationPerHour);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "friendly_name": "my rate limit profile for 6 reservations an Hour",
    "reservations_per_hour": 6,
    "date_created": "2020-05-06T16:24:36+03:00",
    "date_updated": "2020-05-06T16:24:36+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/RateLimitProfiles/RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
    }




Get All RateLimitProfiles
----------------------------

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/RateLimitProfiles

This endpoint retrives all RateLimitProfiles

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

    curl -X GET https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/RateLimitProfiles \
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.RateLimitProfile;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        

        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            RateLimitProfile.Reader reader = RateLimitProfile.reader("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx");

            for(RateLimitProfile rl:reader.read())
            System.out.println(rl);
        }
    }


**Python**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    workspace = rindap.workspaces.get("WSb9d8cf8597f64f77a45666c4c0263862")
    rate_limit_profiles = workspace.rate_limit_profiles.list(limit=10, page_size=5)
    for rate_limit_profile in rate_limit_profiles:
        print("RateLimitProfileSid: {}".format(rate_limit_profile.sid))
        print("FriendlyName: {}".format(rate_limit_profile.friendly_name))


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Get all ratelimits
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .rateLimitProfiles
    .list({
        limit: 100,
        pageSize: 100
    }, function(err, rateLimitProfiles) {
        rateLimitProfiles.forEach(function(rateLimitProfile){
        console.log(rateLimitProfile.sid);
        console.log(rateLimitProfile.friendlyName);
        console.log(rateLimitProfile.reservationsPerHour);
        });
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

            // List All RateLimitProfiles
            var rateLimitProfiles = RateLimitProfileResource.Read(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                limit: 100,
                pageSize: 100
                );

            foreach (var rateLimitProfile in rateLimitProfiles)
            {
                // Print RateLimitProfile Content
                Console.WriteLine("SID               : " + rateLimitProfile.Sid);
                Console.WriteLine("FriendlyName      : " + rateLimitProfile.FriendlyName);
                Console.WriteLine("ReservationPerHour: " + rateLimitProfile.ReservationPerHour);
            }
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "meta": {
        "page_size": 50,
        "page": 0,
        "first_page_url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/RateLimitProfiles?Page=0&PageSize=50",
        "previous_page_url": null,
        "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/RateLimitProfiles?Page=0&PageSize=50",
        "key": "rate_limit_profiles",
        "next_page_url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/RateLimitProfiles?Page=1&PageSize=50"
    },
    "rate_limit_profiles": [
        {
        "sid": "RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "friendly_name": "my rate limit profile for 6 reservations an Hour",
        "reservations_per_hour": 6,
        "date_created": "2020-05-06T16:24:36+03:00",
        "date_updated": "2020-05-06T16:24:36+03:00",
        "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/RateLimitProfiles/RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "links": {
            "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
        }
        }
    ]
    }



Fetch a RateLimitProfile
----------------------------

.. http:get:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSid}/RateLimitProfiles/{RateLimitProfileSID}

This endpoint fetches a single RateLimitProfile with all Its details

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
   * - **RateLimitProfileSID**
     - String
     - ""
     - The SID of the RateLimitProfile


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X GET https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /RateLimitProfiles/RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"


**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.RateLimitProfile;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            RateLimitProfiles rl = RateLimitProfile
                .fetcher("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .fetch();

            System.out.println(rl);
        }
    }


**Python**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    rate_limit_profile = workspace.rate_limit_profiles.get('RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx').fetch()
    print("RateLimitProfileSid: {}".format(rate_limit_profile.sid))
    print("FriendlyName: {}".format(rate_limit_profile.friendly_name))


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Get a ratelimit with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .rateLimitProfiles("RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .fetch(function(err, rateLimitProfile) {
        console.log(rateLimitProfile.sid);
        console.log(rateLimitProfile.friendlyName);
        console.log(rateLimitProfile.reservationsPerHour);
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

            // Get a RateLimitProfile with SID
            var rateLimitProfile = RateLimitProfileResource.Fetch(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
                );

            // Print RateLimitProfile Content
            Console.WriteLine("SID               : " + rateLimitProfile.Sid);
            Console.WriteLine("FriendlyName      : " + rateLimitProfile.FriendlyName);
            Console.WriteLine("ReservationPerHour: " + rateLimitProfile.ReservationPerHour);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "friendly_name": "my rate limit profile for 6 reservations an Hour",
    "reservations_per_hour": 6,
    "date_created": "2020-05-06T16:24:36+03:00",
    "date_updated": "2020-05-06T16:24:36+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/RateLimitProfiles/RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
    }




Update a RateLimitProfile
-----------------------------

.. http:put:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/RateLimitProfiles/{RateLimitProfileSID}

.. warning:: When you update the `reservations_per_hour` field , all the Workers with this RateLimitProfile will be affected immediately, Thus receiving Reservations according to this new value.


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
     - The SID of the Workspace with the Rate Limit Profile to update
   * - **RateLimitProfileSID**
     - String
     - ""
     - The SID of the Rate Limit Profile to be updated
   * - **FriendlyName**
     - String
     - ""
     - (optional) Human readable friendly name. It can be 512 characters long 
   * - **ReservationsPerHour**
     - Integer
     - ""
     - (optional) The maximum number of Reservations that can be created hourly, for a Worker with this RateLimitProfile


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X PUT https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /RateLimitProfiles/RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \  
    --data-urlencode 'FriendlyName=my newly named rate limit profile' \
    --data-urlencode 'ReservationsPerHour=30' \
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"



**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.RateLimitProfile;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            RateLimitProfile rl = RateLimitProfile
                .updater("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .setFriendlyName("my newly named rate limit profile")
                .setReservationsPerHour(30)            
                .update();

            System.out.println(rl);
        }
    }


**Python**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    workspace = rindap.workspaces.get("WSb9d8cf8597f64f77a45666c4c0263862")
    rate_limit_profile_fetcher = workspace.rate_limit_profiles.get('RL8e22315c14294c749db7eee2d9d7bc27')
    rate_limit_profile = rate_limit_profile_fetcher.update(friendly_name="New New Rate Limit", reservation_per_hour=9)
    print("RateLimitProfileSid: {}".format(rate_limit_profile.sid))
    print("FriendlyName: {}".format(rate_limit_profile.friendly_name))


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Update a ratelimit with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .rateLimitProfiles("RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    .update({
        friendlyName: "New Friendly Name",
        reservationsPerHour: 7
    },function(err, rateLimitProfile) {
        console.log(rateLimitProfile.sid);
        console.log(rateLimitProfile.friendlyName);
        console.log(rateLimitProfile.reservationsPerHour);
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

            // Update RateLimitProfile
            var rateLimitProfile = RateLimitProfileResource.Update(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                friendlyName: "New Friendly Name",
                reservationsPerHour: 6
                );

            // Print RateLimitProfile Content
            Console.WriteLine("SID               : " + rateLimitProfile.Sid);
            Console.WriteLine("FriendlyName      : " + rateLimitProfile.FriendlyName);
            Console.WriteLine("ReservationPerHour: " + rateLimitProfile.ReservationPerHour);
        }
    }


**The above command returns JSON structured like this:**

.. code-block:: json

    {
    "sid": "RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "account_sid": "ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "workspace_sid": "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "friendly_name": "my newly named rate limit profile",
    "reservations_per_hour": 30,
    "date_created": "2020-05-06T16:24:36+03:00",
    "date_updated": "2020-05-06T16:24:36+03:00",
    "url": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/RateLimitProfiles/RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "links": {
        "workspace": "https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    }
    }



Delete a RateLimitProfile
-------------------------------

.. http:delete:: /v1/rindap-rest-gw/Workspaces/{WorkspaceSID}/RateLimitProfiles/{RateLimitProfileSID}

.. warning:: When you DELETE a RateLimitProfile , all the Workers with this RateLimitProfile will be affected immediately, Thus receiving Reservations with NO LIMITS

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
     - The SID of the Workspace with the Rate Limit Profile
   * - **RateLimitProfileSID**
     - String
     - ""
     - The SID of the Rate Limit Profile


Example code pieces using SDKs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Shell**

.. code-block:: shell

    curl -X DEL https://api.rindap.com/v1/rindap-rest-gw/Workspaces/WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx \
    /RateLimitProfiles/RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    -H "Authorization: Bearer {YOUR_ACCOUNT_SID}.{YOUR_AUTH_TOKEN}" \
    -H "Content-Type:application/x-www-form-urlencoded"



**Java**

.. code-block:: java

    // Install the Java helper library from rindap.com/docs/java/install

    import com.rindap.Rindap;
    import com.rindap.rest.v1.workspace.RateLimitProfile;

    public class Example {
        // Find your Account Sid and Token at rindap.com/console
        
        public static void main(String[] args) {

            Rindap.init("YOUR_ACCOUNT_SID","YOUR_AUTH_TOKEN");

            RateLimitProfile.deleter("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx", "RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
                .delete();
        }
    }


**Python**

.. code-block:: python

    from rindap.rest import Client
    from rindap.rest import Rindap

    client = Client("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN")
    rindap = Rindap(client)

    workspace = rindap.workspaces.get("WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
    if workspace.rate_limit_profiles.get('RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx').delete():
        print("RateLimitProfile has been deleted!")


**JS**

.. code-block:: javascript

    var Rindap = require('rindap');

    // Authenticate
    var rindap = new Rindap("YOUR_ACCOUNT_SID", "YOUR_AUTH_TOKEN");

    // Delete a ratelimit with SID
    rindap.workspaces('WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx')
    .rateLimitProfiles("RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx")
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

            // Update RateLimitProfile
            var isDeleted = RateLimitProfileResource.Delete(
                pathWorkspaceSid: "WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
                pathSid: "RLxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
                );

            if (isDeleted)
            {
                Console.WriteLine("RateLimitProfile has been deleted!");
            }
        }
    }
