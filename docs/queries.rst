==========================
Querying with ElasticUtils
==========================

ElasticUtils makes querying and filtering and collecting facets from
ElasticSearch simple ::


    q = (S(product='firefox').filter(version='4.0', platform='all')
                             .facet('product', global_=True).facet('version')
                             .facet('platform').facet('type'))

Search All
----------

By default ``S()`` will do a ``match_all`` query in ElasticSearch.


Search Query
------------

``S('taco trucks')`` will do a query for "taco trucks".


Filters
-------

``S('taco trucks').filter(style='korean')`` will do a query for "taco trucks"
filtering on the attribute ``style``.  This is how we find Korean Taco Trucks.

*Note*: Repeat calls to ``.filter`` will reset the filters.


Multiple Filters
~~~~~~~~~~~~~~~~

``S('taco trucks').filter(style='korean', price='FREE')``
will do a query for "taco trucks" that are
"korean" style and have a price of
"FREE".

This is however equivalent to the more succinct::

S('taco trucks', style='korean', price='FREE')


Complicate Filtering
~~~~~~~~~~~~~~~~~~~~

Sometimes you want something complicated.  For that we have the ``F`` (filter)
object.

``S('taco trucks').filter(F(style='korean')|F(style='thai'))``
will find you "thai" or "korean" style taco trucks.

Let's say you only want "korean" tacos if you can get it for "FREE" or "thai"
tacos at any price::

S('taco trucks').filter(F(style='korean', price='FREE')|F(style='thai'))


Facets
------

``S('taco trucks').facet('style').facet('location')`` will do a query for
"taco trucks" and return facets for the ``style`` and ``location`` fields.

Facets can also be scripted_::

    S('taco trucks').facet('style', script='term == korean ? true : false')

.. _scripted: http://www.elasticsearch.org/guide/reference/api/search/facets/terms-facet.html

Results
-------

Results are accessible via ``S('taco trucks').get_results()``.

Total hits can be found by doing::

    r = S('taco trucks').get_results()
    r.total

