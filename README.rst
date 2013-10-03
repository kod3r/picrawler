PiCrawler
=========

.. image:: https://badge.fury.io/py/picrawler.png
    :target: http://badge.fury.io/py/picrawler

.. image:: https://travis-ci.org/studio-ousia/picrawler.png?branch=master
    :target: https://travis-ci.org/studio-ousia/picrawler

PiCrawler is a distributed web crawler using PiCloud.

Using PiCrawler, you can easily implement a distributed web crawler within a few lines of code.

.. code-block:: python

    >>> from picrawler import PiCloudConnection
    >>>
    >>> with PiCloudConnection() as conn:
    ...     response = conn.send(['http://en.wikipedia.org/wiki/Star_Wars',
    ...                           'http://en.wikipedia.org/wiki/Darth_Vader'])
    ...     print 'status code:', response[0].status_code
    ...     print 'content:', response[0].content[:15]
    status code: 200
    content: <!DOCTYPE html>


Installation
------------

To install PiCrawler, simply:

.. code-block:: bash

    $ pip install picrawler

Alternatively,

.. code-block:: bash

    $ easy_install picrawler


PiCloud Setup
-------------

Before using PiCrawler, it is neccessary to configure an API key of PiCloud.

.. code-block:: python

    >>> import cloud
    >>> cloud.setkey(API_KEY, API_SECRETKEY)

You can obtain an API key by signing up on `PiCloud <http://www.picloud.com/>`_.


Using Real-time Cores
---------------------

PiCloud enables you to reserve your exclusive computational resources by requesting `real-time cores <http://docs.picloud.com/realtime_cores.html>`_.

PiCrawler provides a thin wrapper class for requesting the cores.

NOTE: *s1 core* is the most suitable for crawling tasks, because PiCloud ensures that each *s1 core* has a unique IP address.


.. code-block:: python

    >>> from picrawler import RTCoreRequest
    >>>
    >>> with RTCoreRequest(core_type='s1', num_cores=10):
    ...     pass


Customizing Requests
--------------------

You can easily customize the request headers and other internal behaviors by using Request instances instead of raw URL strings.
Since PiCrawler internally uses `Python requests <http://docs.python-requests.org/en/latest/>`_, it supports all arguments that are supported in Python requests.

.. code-block:: python

    >>> from picrawler import PiCloudConnection
    >>> from picrawler.request import Request
    >>>
    >>> req = Request('http://en.wikipedia.org/wiki/Star_Wars',
    ...               'GET',
    ...               headers={'User-Agent': 'MyCrawler'},
    ...               args={'timeout': 5})
    >>>
    >>> with PiCloudConnection() as conn:
    ...     response = conn.send([req])


Defining Callbacks
------------------

You can also define callbacks to the request.

.. code-block:: python

    >>> import logging
    >>>
    >>> from picrawler import PiCloudConnection
    >>> from picrawler.request import Request
    >>>
    >>> req = Request('http://en.wikipedia.org/wiki/Star_Wars', 'GET',
    ...               success_callback=lambda resp: logging.info(resp.content),
    ...               error_callback=lambda resp: logging.exception(resp.exception))
    >>>
    >>> with PiCloudConnection() as conn:
    ...     response = conn.send([req])


Documentation
-------------

Documentation is available at http://picrawler.readthedocs.org/.
