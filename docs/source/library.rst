.. _library:

=======
Library
=======

Powerhose is organized into a simple hierarchy of classes and a few functions:

- :func:`get_cluster` -- a function that create a whole cluster.

- :class:`Job` -- A class that holds a job to be performed.

- :class:`Broker` -- A class that pass the jobs it receives to workers.

- :class:`Worker` -- A class that connects to a broker and pass jobs it receives
  to a Python callable.

- :class:`Heartbeat` and :class:`Stethoscope` -- implements a heartbeat.

- :class:`Client` -- A class that connects to a broker and let you run jobs against
  it.



get_cluster
===========

The :func:`get_cluster` function creates a :class:`Broker` and several
:class:`Worker` instance. It can be run in the background for conveniency.


.. autofunction:: powerhose.get_cluster


Example::

    from powerhose import get_cluster
    from powerhose.client import Client


    cluster = get_cluster('example.echo', background=True)
    cluster.start()

    client = Client()

    for i in range(10):
        print client.execute(str(i))

    cluster.stop()




Job
===


.. autoclass:: powerhose.job.Job
   :members: add_header, serialize, load_from_string


Example::

    >>> from powerhose.job import Job
    >>> job = Job('4*2')
    >>> job.serialize()
    'NONE:::4*2'
    >>> Job.load_from_string('NONE:::4*2')
    <powerhose.job.Job object at 0x107b78c50>
    >>> Job.load_from_string('NONE:::4*2').data
    '4*2'


Broker
======

.. autoclass:: powerhose.broker.Broker
   :members: start,stop


Worker
======

.. autoclass:: powerhose.worker.Worker
   :members: start,stop


Heartbeat
=========

The :class:`Broker` class runs a :class:`Heartbeat` instance that regularly
sends a *BEAT* message on a PUB channel. Each worker has a  :class:`Stethoscope`
instance that subscribes to the channel, to check if the :class:`Broker` is still
around.

.. autoclass:: powerhose.heartbeat.Heartbeat
   :members: stop,start

.. autoclass:: powerhose.heartbeat.Stethoscope
   :members: stop,start


Client
======

.. autoclass:: powerhose.client.Client
   :members: execute, ping


Client Pool
===========

.. autoclass:: powerhose.client.Pool
   :members: execute
