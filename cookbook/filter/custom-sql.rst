.. _rst_cookbook_filter_custom-sql:

Eigenes SQL
===========

Die ersten Hinweise für die Möglichkeiten der Filterregel
"Eigenes SQL" sind über die |img_about| Hilfe zu finden.

Nochmal der Hinweis: Auch mit der Filterregel "Eigenes SQL"
werden nur IDs zur nächsten Filterregel bzw. zum Filterset
weiter gereicht. Es können keine "Attributwerte" hinzugefügt
werden, auch wenn das per SQL z.B. durch JOINs möglich wäre.

Folgend einige SQL-Queries als "Zutat" für das eigene "SQL-Menü":


**"LIKE"-Abfrage mit Defaultwert**

"Suche Items für die Attribut 'name' wenn GET-Parameter 'name' 
gesetzt ist oder gebe alle Items aus (keine Filterung)."

.. code-block:: php
   :linenos:
   
   SELECT id 
   FROM {{table}} 
   WHERE name LIKE (CONCAT('%',{{param::get?name=name&default=%%}},'%')) 

**Filterung nach Datum**

"Suche Items für die Attribut 'date_start' größer oder gleich dem 
heutigen Datum ist - also in der Zukunft liegt"

.. code-block:: php
   :linenos:
   
   SELECT id 
   FROM {{table}} 
   WHERE FROM_UNIXTIME(`date_start`) >= CURDATE()

**Filterung nach Kind-Elementen eines Eltern-Elements**

"Suche alle Kind-Elemente für ein gegebens Eltern-Element über den Alias-Parameter
- z.B. um auf einer Detailseite alle zugehörigen 'Kind-Elemente' auszugeben."

.. code-block:: php
   :linenos:
   
   SELECT id 
   FROM mm_child
   WHERE pid = (
     SELECT id 
     FROM mm_parent
     WHERE
     parent_alias={{param::get?name=auto_item}}
   )  


.. |img_about| image:: /_img/icons/about.png
