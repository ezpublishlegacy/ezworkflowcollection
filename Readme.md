Extension ezworkflowcollection for eZ publish
=============================================

This extension is a collection of general-purpose workflow event types that could
be useful in different websites.

The list of event types available so far:

Multipublish workflow event:
----------------------------

Will add secondary locations (one or more) when a content is published.

The list of nodes used as parents for secondary location(s) has to be defined for every workflow event (ie. in the admin interface).
It is possible, but not mandatory, to add a class/attribute combination acting as filter, deciding for every given object being published wheter the multipublication will happen or not:
  - If no class/attribute combination is selected, the workflow will apply to every object published (to limit its applicability, chain it to a multiplexer workflow).
  - If a class/attribute combination is selected, the workflow will check the value of the given attribute to decide if multi-publication has to take place; if the attribute is not found in the current class, no multi-publication happens.
  - If multiple class/attribute combinations are selected, the workflow will apply to every object that has set to TRUE at least one of those attributes.
If an edited object is already child of one of the nodes selcted as multi-publication location, the workflow does not add a new secondary location at the same place.
The cronjob never removes any secondary location, nor does it change any location to become primary.


Subtree Multiplexer workflow event:
-----------------------------------

Like a standard multiplexer event, but filters on node subtrees instead


Expire remote caches (ezflow based) workflow event:
---------------------------------------------------

Used to purge reverse proxy caches when an object is edited.
This allows to set a high TTL for all web pages and increase the hit rate of reverse proxies, and at the same time to have the proxy refresh its cached version of a page as soon as the object is edited.

It is based on the eZHTTPCacheManager class and configuration from eZ Flow, which means that:

- eZ Flow is needed to take advantage of this workflow
- for the workflow to do anything, editing squid.ini is necessary
- currently the only CacheManager class available can purge both squid and varnish reverse proxy caches
- it allows to purge the cache correctly even if multiple domains/hostnames are cused (see ezworkflowcollection.ini)


"Approve location" workflow event:
----------------------------------
Allows approval upon adding a new location to an object.
Look at INSTALL file and check troubleshooting section for more info.


"Update object state" workflow event:
-------------------------------------

Allows to move object state from X to Y upon content publishing


"Add url alias" workflow event:
-------------------------------

Allows to add an url alias to an object upon content publishing.
The alias will be formed by an (user-defined) attribute of the object itself;
it can be set to redirect internally or externally, and to be anchored at root or at parent node


"Copy all existing children to new location" workflow event:
------------------------------------------------------------

When adding a new location to an object which already has some children, add those
children as well to the new location. Useful e.g. when you have a content that is
composed of a parent node and some children, and you want to keep them together
at all times, even when using multi-location


"Copy a node to all locations of parent (if parent has many)" workflow event:
------------------------------------------------------------

When publishing a new object, check if parent node has many locations and, if it
has, add the new object as child to all locations of parent object.
Useful e.g. when you have a content that is composed of a parent node and some
children, and you want to keep them together at all times, even when using
multi-location



FUTURE IDEAS
============

'Relate to node' workflow event:
--------------------------------
will add a content as related-object to another node.

'Relate to node attribute' workflow event:
------------------------------------------
will add a content as related-object to another node, making use of a related-object(s) attribute.

Section-based multiplexer
-------------------------

Object-state-based multiplexer
------------------------------

Hide until date workflow event:
-------------------------------
please use the GWUtils extension, that already implements this

Send email workflow event:
--------------------------
similar to notification system, but based on workflows
also with batch emails (eg. 1 x week or 1 x month)

Improved approval workflow event:
---------------------------------
. send out an email in case of rejection, too
. make notification configuration flexible

Log workflow event:
-------------------
a simple tracing mechanism, useful e.g. for auditing purposes or debugging
workflow execution

Halt workflow event:
--------------------

Object-attribute based multiplexer workflow event:
--------------------------------------------------
