==================
The Client object
==================

.. currentmodule:: zeep

The :class:`Client` is the main interface for interacting with a SOAP server.
It provides a ``service`` attribute which references the default binding of
the client (via a ServiceProxy object). The default binding can be specified
when initating the client by passing the ``service_name`` and ``port_name``.
Otherwise the first service and first port within that service are used as the
default.


The ServiceProxy object
-----------------------

The ServiceProxy object is a simple object which will check if an operation
exists for attribute or item requested.  If the operation exists then it will
return an OperationProxy object (callable) which is responsible for calling the
operation on the binding. 


.. code-block:: python

    from zeep import Client
    from zeep import xsd

    client = Client('http://my-endpoint.com/production.svc?wsdl')

    # service is a ServiceProxy object.  It will check if there
    # is an operation with the name `X` defined in the binding
    # and if that is the case it will return an OperationProxy
    client.service.X()  

    # The operation can also be called via an __getitem__ call. 
    # This is usefull if the operation name is not a valid 
    # python attribute name.
    client.service['X-Y']()  


Using non-default bindings
--------------------------
As mentioned by default Zeep picks the first binding in the wsdl as the
default. This binding is availble via ``client.service``. To use a specific
binding you can use the ``bind()`` method on the client object:


.. code-block:: python

    from zeep import Client
    from zeep import xsd

    client = Client('http://my-endpoint.com/production.svc?wsdl')

    service2 = client.bind('SecondService', 'Port12')
    service2.someOperation(myArg=1)


Creating new ServiceProxy objects
---------------------------------
There are situations where you either need to change the SOAP address from the
one which is defined within the WSDL or the WSDL doesn't define any service
elements. This can be done by creating a new ServiceProxy using the 
``Client.create_service()`` method.

.. code-block:: python

    from zeep import Client
    from zeep import xsd

    client = Client('http://my-endpoint.com/production.svc?wsdl')
    service = client.create_service(
        '{http://my-target-namespace-here}myBinding',
        'http://my-endpoint.com/acceptance/')

    service.submit('something') 
