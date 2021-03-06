###########
Using Eigen
###########

.. note::

   Changes to this document must be approved by the System Architect (`RFC-24 <https://jira.lsstcorp.org/browse/RFC-24>`_).
   To request changes to these policies, please file an :doc:`RFC </communications/rfc>`.


.. _cpp_using_eigen:

The `Eigen`_ C++ template library for linear algebra is distributed with the LSST software stack.

It is permitted to use any of the standard modules included with Eigen in LSST code without restrictions beyond the :ref:`usual coding guidelines <part-guides>`.

Eigen also includes some **unsupported** modules which are located in the ``include/unsupported`` directory. For details, refer to the `relevant Eigen documentation`_.

These unsupported modules are included for the convenience of other projects which build upon the LSST codebase. Their use in LSST code is currently forbidden.

.. _Eigen: http://eigen.tuxfamily.org/
.. _relevant Eigen documentation: http://eigen.tuxfamily.org/dox/unsupported/index.html
