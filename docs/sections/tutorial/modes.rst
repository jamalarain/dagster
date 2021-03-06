Pipeline modes
--------------

By attaching different sets of resources with the same APIs to different modes, we can support
running pipelines -- with unchanged business logic -- in different environments. So you might have
a "unittest" mode that runs against an in-memory SQLite database, a "dev" mode that runs against
Postgres, and a "prod" mode that runs against Snowflake.

Separating the resource definition from the business logic makes pipelines testable. As long as the
APIs of the resources agree, and the fundamental operations they expose are tested in each
environment, we can test business logic independent of environments that may be very costly or
difficult to test against.

.. literalinclude:: ../../../examples/dagster_examples/intro_tutorial/modes.py
   :lines: 75-84
   :linenos:
   :lineno-start: 75
   :caption: modes.py
   :language: python

Even if you're not familiar with SQLAlchemy, it's enough to note that this is a very different
implementation of the ``warehouse`` resource. To make this implementation available to Dagster, we
attach it to a :py:class:`ModeDefinition <dagster.ModeDefinition>`.

.. literalinclude:: ../../../examples/dagster_examples/intro_tutorial/modes.py
   :lines: 127-142
   :linenos:
   :lineno-start: 127
   :caption: modes.py
   :language: python

Each of the ways we can invoke a Dagster pipeline lets us select which mode we'd like to run it in.

From the command line, we can set ``-d`` or ``--mode`` and select the name of the mode:

.. code-block:: shell

    $ dagster pipeline execute -f modes.py -n modes_pipeline -d dev

From the Python API, we can use the :py:class:`RunConfig <dagster.RunConfig>`:

.. literalinclude:: ../../../examples/dagster_examples/intro_tutorial/modes.py
   :lines: 152-156
   :linenos:
   :lineno-start: 152
   :emphasize-lines: 4
   :dedent: 4
   :caption: modes.py
   :language: python

And in Dagit, we can use the "Mode" selector to pick the mode in which we'd like to execute.

.. thumbnail:: modes.png

The config editor is Dagit is mode-aware, so when you switch modes and introduce a resource that
requires additional config, the editor will prompt you.
