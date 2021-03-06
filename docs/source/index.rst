MoSDeF Cassandra
================
|Citing|
|CodeCov|
|Azure|

.. |Citing| image:: https://img.shields.io/badge/cite-mosdef__cassandra-blue
   :target: reference/citing.html
.. |Codecov| image:: https://codecov.io/gh/MaginnGroup/mosdef_cassandra/branch/master/graph/badge.svg
.. |Azure| image:: https://dev.azure.com/MaginnGroup/mosdef_cassandra/_apis/build/status/MaginnGroup.mosdef_cassandra?branchName=master

Overview
~~~~~~~~

**MoSDeF Cassandra** is a Python interface for the **Cassandra** Monte Carlo software.
It offers complete integration with the **MoSDeF** tools and a user-friendly
interface for Cassandra.

.. warning::
  **MoSDeF Cassandra** is still in early development (0.x releases). The API may
  change unexpectedly.

Resources
~~~~~~~~~

* :doc:`Installation guide <getting_started/install>`: Instructions for installing MoSDeF Cassandra
* :ref:`keyconcepts`: How we think about MoSDeF Cassandra
* `GitHub repository <https://github.com/MaginnGroup/mosdef_cassandra>`_: View the source code, contribute, and raise issues
* `Cassandra <https://cassandra.nd.edu>`_: Learn more about the Cassandra Monte Carlo software
* `Cassandra respository <https://github.com/MaginnGroup/Cassandra>`_: View the source of the Cassandra Monte Carlo software
* `MoSDeF tools <https://mosdef.org>`_: A collection of tools for constructing systems and applying forcefield parameters for particle-based simulations

Citation
~~~~~~~~

Please cite **MoSDeF Cassandra**, **Cassandra**, and the **MoSDeF** suite of
tools if you use this tool in your research. See :doc:`here <reference/citing>` for details.

Installation
~~~~~~~~~~~~

Installation instructions are :doc:`here <getting_started/install>`.
A conda installation is available:

.. code-block:: bash

    conda create --name mc mosdef_cassandra foyer -c conda-forge -c mosdef -c omnia

Example
~~~~~~~

MoSDeF Cassandra provides a Python interface to Cassandra. The workflow
consists of first constructing a system and move set. These two objects are
passed to a function that calls Cassandra to perform the Monte Carlo simulation.
The example below demonstrates an NVT Monte Carlo simulation
of OPLS methane. No additional files are required. Everything
required to run the Monte Carlo simulation is contained in the script below.

.. code-block:: python

  import mbuild
  import foyer
  import mosdef_cassandra as mc

  # Create a methane molecule from a SMILES string
  methane = mbuild.load("C", smiles=True)

  # Load the forcefield via foyer
  ff = foyer.forcefields.load_OPLSAA()

  # Apply the forcefield parameters to methane with foyer
  methane_ff = ff.apply(methane)

  # Define an empty simulation box
  box = mbuild.Box([3.0, 3.0, 3.0])

  # Define the boxes, species in the system, molecules in the box
  ensemble = 'nvt'
  box_list = [box]
  species_list = [methane_ff]
  molecules_to_add = [[100]]

  # Create the System
  system = mc.System(box_list, species_list, mols_to_add=molecules_to_add)

  # Create the MoveSet
  moveset = mc.MoveSet(ensemble, species_list)

  # Run a Monte Carlo simulation!
  mc.run(
      system=system,
      moveset=moveset,
      run_type="equilibration",
      run_length=1000,
      temperature=300.0 * u.K
  )

Several additional examples can be found :doc:`here <getting_started/examples>`.

Credits
~~~~~~~

Development of MoSDeF Cassandra was supported by the National Science Foundation
under grant NSF Grant Number 1835874. Any opinions, findings, and conclusions or
recommendations expressed in this material are those of the author(s) and do
not necessarily reflect the views of the National Science Foundation.

See :doc:`here <reference/credits>` for complete credits.

Table of Contents
=================

.. toctree::
   :maxdepth: 2
   :caption: Getting Started

   getting_started/intro
   getting_started/install
   getting_started/quickstart
   getting_started/examples

.. toctree::
   :maxdepth: 2
   :caption: Guides

   guides/philosophy
   guides/unyts
   guides/system
   guides/moveset
   guides/kwargs
   guides/runners

.. toctree::
   :maxdepth: 2
   :caption: API

   api/api.rst

.. toctree::
   :maxdepth: 2
   :caption: Reference

   reference/develop
   reference/citing
   reference/license
   reference/credits

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
