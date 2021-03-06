.. doctest-skip-all

.. _astroquery.gaia:

*****************************
Gaia TAP+ (`astroquery.gaia`)
*****************************

Gaia is an ambitious mission to chart a three-dimensional map of our Galaxy,
the Milky Way, in the process revealing the composition, formation and evolution
of the Galaxy. Gaia will provide unprecedented positional and radial velocity
measurements with the accuracies needed to produce a stereoscopic and kinematic
census of about one billion stars in our Galaxy and throughout the Local Group.
This amounts to about 1 per cent of the Galactic stellar population.

If you use public Gaia DR1 data in your paper, please take note of our guide_ on
how to acknowledge and cite Gaia DR1.

.. _guide: http://gaia.esac.esa.int/documentation/GDR1/Miscellaneous/sec_credit_and_citation_instructions.html

This package allows the access to the European Space Agency Gaia Archive (http://archives.esac.esa.int/gaia)

Gaia Archive access is based on a TAP+ REST service. TAP+ is an extension of
Table Access Protocol (TAP: http://www.ivoa.net/documents/TAP/) specified by the
International Virtual Observatory Alliance (IVOA: http://www.ivoa.net).

The TAP query language is Astronomical Data Query Language
(ADQL: http://www.ivoa.net/documents/ADQL/2.0), which is similar
to Structured Query Language (SQL), widely used to query databases.

TAP provides two operation modes: Synchronous and Asynchronous:

* Synchronous: the response to the request will be generated as soon as the
  request received by the server.
  (Do not use this method for queries that generate a big amount of results.)
* Asynchronous: the server will start a job that will execute the request.
  The first response to the request is the required information (a link)
  to obtain the job status.
  Once the job is finished, the results can be retrieved.

Gaia TAP+ server provides two access mode: public and authenticated:

* Public: this is the standard TAP access. A user can execute ADQL queries and
  upload tables to be used in a query 'on-the-fly' (these tables will be removed
  once the query is executed). The results are available to any other user and
  they will remain in the server for a limited space of time.

* Authenticated: some functionalities are restricted to authenticated users only.
  The results are saved in a private user space and they will remain in the server
  for ever (they can be removed by the user).

  * ADQL queries and results are saved in a user private area.

  * Cross-match operations: a catalog cross-match operation can be executed.
    Cross-match operations results are saved in a user private area.

  * Persistence of uploaded tables: a user can upload a table in a private space.
    These tables can be used in queries as well as in cross-matches operations.


This python module provides an Astroquery API access. Nevertheless, only
``query_object`` and ``query_object_async`` are implemented.

The Gaia Archive table used for the methods where no table is specified is
``gaiadr1.gaia_source``

========
Examples
========

---------------------------
1. Non authenticated access
---------------------------

1.1. Query object
~~~~~~~~~~~~~~~~~

.. code-block:: python

  >>> import astropy.units as u
  >>> from astropy.coordinates.sky_coordinate import SkyCoord
  >>> from astropy.units import Quantity
  >>> from astroquery.gaia import Gaia
  >>>
  >>> coord = SkyCoord(ra=280, dec=-60, unit=(u.degree, u.degree), frame='icrs')
  >>> width = Quantity(0.1, u.deg)
  >>> height = Quantity(0.1, u.deg)
  >>> r = Gaia.query_object_async(coordinate=coord, width=width, height=height)
  >>> r.pprint()

           dist             solution_id     ...       ecl_lat
                                            ...      Angle[deg]
  --------------------- ------------------- ... -------------------
  0.0026029414438061079 1635378410781933568 ... -36.779151653783892
  0.0038537557334594502 1635378410781933568 ... -36.773899692008634
  0.0045451702670639632 1635378410781933568 ... -36.772645786277522
  0.0056131312891700424 1635378410781933568 ... -36.781488832325074
  0.0058494547209840585 1635378410781933568 ... -36.770812028764119
  0.0062076788443168303 1635378410781933568 ... -36.780588167751368
  0.008201843586626921 1635378410781933568 ... -36.784730288359086
  0.0083377863521668077 1635378410781933568 ... -36.784848302904727
  0.0084057202175603796 1635378410781933568 ... -36.784556953222634
  0.0092437652172596384 1635378410781933568 ... -36.767784193150469
                  ...                 ... ...                 ...
  0.049586988816560117 1635378410781933568 ... -36.824132319326232
  0.049717306565450765 1635378410781933568 ... -36.823845008396503
  0.049777020825344041 1635378410781933568 ...  -36.72857293240213
  0.050385912463710505 1635378410781933568 ... -36.729880776402624
  0.050826536195428054 1635378410781933568 ... -36.822968947436181
  0.050859645206141363 1635378410781933568 ... -36.823021426398789
  0.051040085912766479 1635378410781933568 ... -36.728589237516161
  0.051211160779507325 1635378410781933568 ... -36.825120633172546
  0.051958453766310551 1635378410781933568 ... -36.725819366872734
  0.053207596589671176 1635378410781933568 ... -36.826600298826662
  Length = 152 rows


1.2. Cone search
~~~~~~~~~~~~~~~~

.. code-block:: python

  >>> import astropy.units as u
  >>> from astropy.coordinates.sky_coordinate import SkyCoord
  >>> from astropy.units import Quantity
  >>> from astroquery.gaia import Gaia
  >>>
  >>> coord = SkyCoord(ra=280, dec=-60, unit=(u.degree, u.degree), frame='icrs')
  >>> radius = Quantity(1.0, u.deg)
  >>> j = Gaia.cone_search_async(coord, radius)
  >>> r = j.get_results()
  >>> r.pprint()

           dist             solution_id     ...       ecl_lat
                                          ...      Angle[deg]
  --------------------- ------------------- ... -------------------
  0.0026029414438061079 1635378410781933568 ... -36.779151653783892
  0.0038537557334594502 1635378410781933568 ... -36.773899692008634
  0.0045451702670639632 1635378410781933568 ... -36.772645786277522
  0.0056131312891700424 1635378410781933568 ... -36.781488832325074
  0.0058494547209840585 1635378410781933568 ... -36.770812028764119
  0.0062076788443168303 1635378410781933568 ... -36.780588167751368
  0.008201843586626921 1635378410781933568 ... -36.784730288359086
  0.0083377863521668077 1635378410781933568 ... -36.784848302904727
  0.0084057202175603796 1635378410781933568 ... -36.784556953222634
  0.0092437652172596384 1635378410781933568 ... -36.767784193150469
                  ...                 ... ...                 ...
  0.14654733241000259 1635378410781933568 ... -36.667789989774818
  0.14657617264211745 1635378410781933568 ... -36.876849099093427
  0.14674748663117593 1635378410781933568 ... -36.734323499168184
  0.14678063354511475 1635378410781933568 ... -36.845214606267504
  0.14679704339818228 1635378410781933568 ... -36.697986781654343
  0.14684048305123779 1635378410781933568 ...   -36.6983554058179
  0.14684061095346052 1635378410781933568 ... -36.854933118845658
  0.14690380253776872 1635378410781933568 ... -36.700207569397797
  0.1469069007730108 1635378410781933568 ...  -36.92092859296757
  0.14690740362559238 1635378410781933568 ... -36.677757522466912
  Length = 2000 rows



1.3 Getting public tables
~~~~~~~~~~~~~~~~~~~~~~~~~

To load only table names (TAP+ capability)

.. code-block:: python

  >>> from astroquery.gaia import Gaia
  >>> tables = Gaia.load_tables(only_names=True)
  >>> for table in (tables):
  >>>   print(table.get_qualified_name())

  public.dual
  public.tycho2
  public.igsl_source
  public.hipparcos
  public.hipparcos_newreduction
  public.hubble_sc
  public.igsl_source_catalog_ids
  tap_schema.tables
  tap_schema.keys
  tap_schema.columns
  tap_schema.schemas
  tap_schema.key_columns
  gaiadr1.phot_variable_time_series_gfov
  gaiadr1.ppmxl_neighbourhood
  gaiadr1.gsc23_neighbourhood
  gaiadr1.ppmxl_best_neighbour
  gaiadr1.sdss_dr9_neighbourhood
  ...
  gaiadr1.tgas_source
  gaiadr1.urat1_original_valid
  gaiadr1.allwise_original_valid

To load table names (TAP compatible)

.. code-block:: python

  >>> from astroquery.gaia import Gaia
  >>> tables = Gaia.load_tables()
  >>> for table in (tables):
  >>>   print(table.get_qualified_name())

  public.dual
  public.tycho2
  public.igsl_source
  public.hipparcos
  public.hipparcos_newreduction
  public.hubble_sc
  public.igsl_source_catalog_ids
  tap_schema.tables
  tap_schema.keys
  tap_schema.columns
  tap_schema.schemas
  tap_schema.key_columns
  gaiadr1.phot_variable_time_series_gfov
  gaiadr1.ppmxl_neighbourhood
  gaiadr1.gsc23_neighbourhood
  gaiadr1.ppmxl_best_neighbour
  gaiadr1.sdss_dr9_neighbourhood
  ...
  gaiadr1.tgas_source
  gaiadr1.urat1_original_valid
  gaiadr1.allwise_original_valid

To load only a table (TAP+ capability)

.. code-block:: python

  >>> from astroquery.gaia import Gaia
  >>> table = Gaia.load_table('gaiadr1.gaia_source')
  >>> print(table)

  Table name: gaiadr1.gaia_source
  Description: This table has an entry for every Gaia observed source as listed in the
  Main Database accumulating catalogue version from which the catalogue
  release has been generated. It contains the basic source parameters,
  that is only final data (no epoch data) and no spectra (neither final
  nor epoch).
  Num. columns: 57


Once a table is loaded, columns can be inspected

.. code-block:: python

  >>> from astroquery.gaia import Gaia
  >>> table = Gaia.load_table('gaiadr1.gaia_source')
  >>> for column in (gaiadr1_table.get_columns()):
  >>>   print(column.get_name())

  solution_id
  source_id
  random_index
  ref_epoch
  ra
  ra_error
  dec
  dec_error
  ...
  ecl_lon
  ecl_lat

1.4 Synchronous query
~~~~~~~~~~~~~~~~~~~~~

A synchronous query will not store the results at server side. These queries must be used when the amount of data to be retrieve is 'small'.

There is a limit of 2000 rows. If you need more than that, you must use asynchronous queries.

The results can be saved in memory (default) or in a file.

Query without saving results in a file:

.. code-block:: python

  >>> from astroquery.gaia import Gaia
  >>>
  >>> job = Gaia.launch_job("select top 100 \
  >>> solution_id,ref_epoch,ra_dec_corr,astrometric_n_obs_al,matched_observations,duplicated_source,phot_variable_flag \
  >>> from gaiadr1.gaia_source order by source_id")
  >>>
  >>> print(job)

  Jobid: None
  Phase: COMPLETED
  Owner: None
  Output file: sync_20170223111452.xml.gz
  Results: None

  >>> r = job.get_results()
  >>> print(r['solution_id'])

    solution_id
  -------------------
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
                ...
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  Length = 100 rows

Query saving results in a file:

.. code-block:: python

  >>> from astroquery.gaia import Gaia
  >>> job = Gaia.launch_job("select top 100 \
  >>> solution_id,ref_epoch,ra_dec_corr,astrometric_n_obs_al,matched_observations,duplicated_source,phot_variable_flag \
  >>> from gaiadr1.gaia_source order by source_id", dump_to_file=True)
  >>>
  >>> print(job)

  Jobid: None
  Phase: COMPLETED
  Owner: None
  Output file: sync_20170223111452.xml.gz
  Results: None

  >>> r = job.get_results()
  >>> print(r['solution_id'])

    solution_id
  -------------------
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
                ...
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  Length = 100 rows


1.5 Synchronous query on an 'on-the-fly' uploaded table
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A table can be uploaded to the server in order to be used in a query.

.. code-block:: python

  from astroquery.gaia import Gaia

  >>> upload_resource = 'my_table.xml'
  >>> j = Gaia.launch_job(query="select * from tap_upload.table_test", upload_resource=upload_resource, \
  >>> upload_table_name="table_test", verbose=True)
  >>> r = j.get_results()
  >>> r.pprint()

  source_id alpha delta
  --------- ----- -----
          a   1.0   2.0
          b   3.0   4.0
          c   5.0   6.0


1.6 Asynchronous query
~~~~~~~~~~~~~~~~~~~~~~

Asynchronous queries save results at server side. These queries can be accessed at any time. For anonymous users, results are kept for three days.

The results can be saved in memory (default) or in a file.

Query without saving results in a file:

.. code-block:: python

  >>> from astroquery.gaia import Gaia
  >>>
  >>> job = Gaia.launch_job_async("select top 100 * from gaiadr1.gaia_source order by source_id")
  >>>
  >>> print(job)

  Jobid: 1487845273526O
  Phase: COMPLETED
  Owner: None
  Output file: async_20170223112113.vot
  Results: None

  >>> r = job.get_results()
  >>> print(r['solution_id'])

    solution_id
  -------------------
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
                ...
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  Length = 100 rows

Query saving results in a file:

.. code-block:: python

  >>> from astroquery.gaia import Gaia
  >>>
  >>> job = Gaia.launch_job_async("select top 100 * from gaiadr1.gaia_source order by source_id", dump_to_file=True)
  >>>
  >>> print(job)

  Jobid: 1487845273526O
  Phase: COMPLETED
  Owner: None
  Output file: async_20170223112113.vot
  Results: None

  >>> r = job.get_results()
  >>> print(r['solution_id'])

    solution_id
  -------------------
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
                ...
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  1635378410781933568
  Length = 100 rows


1.6 Asynchronous job removal
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To remove asynchronous

.. code-block:: python

  >>> from astroquery.gaia import Gaia
  >>> job = Gaia.remove_jobs(["job_id_1","job_id_2",...])


---------------------------
2. Authenticated access
---------------------------

Authenticated users are able to access to TAP+ capabilities (shared tables, persistent jobs, etc.)
In order to authenticate a user, ``login`` or ``login_gui`` methods must be called. After a successful
authentication, the user will be authenticated until ``logout`` method is called.

All previous methods (``query_object``, ``cone_search``, ``load_table``, ``load_tables``, ``launch_job``) explained for
non authenticated users are applicable for authenticated ones.

The main differences are:

* Asynchronous results are kept at server side for ever (until the user decides to remove one of them).
* Users can access to shared tables.


2.1. Login/Logout
~~~~~~~~~~~~~~~~~

Graphic interface


*Note: Tkinter module is required to use login_gui method.*

.. code-block:: python

  >>> from astroquery.gaia import Gaia
  >>> Gaia.login_gui()


Command line


.. code-block:: python

  >>> from astroquery.gaia import Gaia
  >>> Gaia.login(user='userName', password='userPassword')


It is possible to use a file where the credentials are stored:

*The file must containing user and password in two different lines.*

.. code-block:: python

  >>> from astroquery.gaia import Gaia
  >>> Gaia.login(credentials_file='my_credentials_file')



To perform a logout


.. code-block:: python

  >>> from astroquery.gaia import Gaia
  >>> Gaia.logout()



2.2. Listing shared tables
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

  >>> from astroquery.gaia import Gaia
  >>> tables = Gaia.load_tables(only_names=True, include_shared_tables=True)
  >>> for table in (tables):
  >>>   print(table.get_qualified_name())

  public.dual
  public.tycho2
  public.igsl_source
  tap_schema.tables
  tap_schema.keys
  tap_schema.columns
  tap_schema.schemas
  tap_schema.key_columns
  gaiadr1.phot_variable_time_series_gfov
  gaiadr1.ppmxl_neighbourhood
  gaiadr1.gsc23_neighbourhood
  ...
  user_schema_1.table1
  user_schema_2.table1
  ...


Reference/API
=============

.. automodapi:: astroquery.gaia
    :no-inheritance-diagram:
