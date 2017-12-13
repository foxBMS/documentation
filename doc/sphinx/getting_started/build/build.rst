.. include:: ../../macros.rst

.. _getting_started_build:

==============================================
Building the foxBMS Software and Documentation
==============================================


Requirements
------------

The build process of |foxbms| heavily depends on Python, for example, for code generation purposes. |foxbms| hence comes with its own Python distribution, called |foxconda|, powered by `Anaconda <https://continuum.io>`_. These build instructions assume that |foxconda| was successfully installed and that the ``PATH`` environment has been adjusted accordingly. For further information refer to the |foxconda| documentation (:ref:`getting_started_foxconda`).

Obtaining the Sources
---------------------

Obtaining the sources can be achieved in two ways, one involving the commandline and one manually downloading the repositories form the |foxbms| GitHub page at `github.com/foxBMS/ <https://github.com/foxBMS/>`_.

The command line version is prefered, as this is more easy.

    #. Using the commandline: There are two possibilties

        #.  The repository ``foxbms-setup`` has not been cloned yet:
        
            #.  Open ``fbterminal`` from ``path/to/foxconda/Scripts/fbterminal.exe``
            #.  ``cd`` to the directory the |foxbms| project should be setup
            #.  Run ``ghloader -u https://github.com/foxBMS/foxBMS-setup.git --script bootstrap.py``
            #.  The ``foxbms-setup`` repository is cloned onto the harddrive and the bootsrap script is run. This will clone all needed repositories, including the setup repository itself to the correct location and build  the documentation. The general |foxbms| documentation can then be found at ``foxbms-setup\build\sphinx\documentation\doc\sphinx\html\index.html`` 
        
        #.  The repository ``foxbms-setup`` was already cloned somewhere to the local harddrive:
    
            #.  Open ``fbterminal`` from ``path/to/foxconda/Scripts/fbterminal.exe``
            #.  ``cd`` to the directory where ``foxbms-setup`` was cloned to
            #.  Run ``python bootstrap.py``. This will clone all further needed repositories to the correct location and build  the documentation. The general |foxbms| documentation can then be found at ``foxbms-setup\build\sphinx\documentation\doc\sphinx\html\index.html`` 


    #.  Downloading the repositories manually
    
        #.  Goto `github.com/foxBMS/ <https://github.com/foxBMS/>`_ and download the following repositories, either using a git client or as ``zip``-files. If they were downloaded as ``zip``-files, it will not be possible to update of the repositories. Additionally the repositories inside the ``zip``-files will have a naming like ``{{repositoryname}}-master``. These must be renamed to ``{{repositoryname}}``.
        
            #.  `github.com/foxBMS/foxbms-setup             <https://github.com/foxBMS/foxbms-setup>`_
            #.  `github.com/foxBMS/mcu-common               <https://github.com/foxBMS/mcu-common>`_
            #.  `github.com/foxBMS/mcu-freertos             <https://github.com/foxBMS/mcu-freertos>`_
            #.  `github.com/foxBMS/mcu-hal                  <https://github.com/foxBMS/mcu-hal>`_
            #.  `github.com/foxBMS/mcu-primary              <https://github.com/foxBMS/mcu-primary>`_
            #.  `github.com/foxBMS/mcu-secondary            <https://github.com/foxBMS/mcu-secondary>`_
            #.  `github.com/foxBMS/documentation            <https://github.com/foxBMS/documentation>`_
            #.  `github.com/foxBMS/hw-extensions            <https://github.com/foxBMS/hw-extension>`_
            #.  `github.com/foxBMS/hw-interface             <https://github.com/foxBMS/hw-interface-ltc6820>`_
            #.  `github.com/foxBMS/hw-master                <https://github.com/foxBMS/hw-master>`_
            #.  `github.com/foxBMS/hw-slave-12-ltc6811-1    <https://github.com/foxBMS/hw-slave-12-ltc6811-1>`_
            #.  `github.com/foxBMS/tools                    <https://github.com/foxBMS/tools>`_

        #.  Put the repositories in a structure like it is shown below. This structure is **mandatory**!

..  warning::
    Do not change directory names or the structure inside ``foxbms-setup``. If this is changed most, if not all, ``wscripts``, have to be heavily adpated and this can get very complex extremly fast.

After that step the directory structure in the ``foxbms-setup`` directory should look like:

..  code-block:: none

    foxbms-setup <dir>
    |___.git <dir> (*)
    |___build <dir>
    |___embedded-software <dir>
    |   |___mcu-bootloader <dir>
    |   |___mcu-common <dir>
    |   |___mcu-freertos <dir>
    |   |___mcu-hal <dir>
    |   |___mcu-primary <dir>
    |   |___mcu-secondary <dir>
    |
    |___documentation <dir>
    |   |___hw-extension
    |   |___hw-interface-ltc6820
    |   |___hw-master
    |   |___hw-slave-12-ltc6811-1
    |
    |___hardware <dir>
    |___tools <dir>
    |
    |___.gitignore <file> (*)
    |___.config.yaml <file> (*)
    |___bootstrap.py <file>
    |___build.py <file>
    |___CHANGELOG.md <file>
    |___LICENSE.md <file>
    |___README.md <file>
    |___wscript <file>


(*) Directories and files with starting full stop are hidden in Windows with the default configuration.

Building the Binaries and Documentation
---------------------------------------
|foxbms| targets can be build using the command line or using the |foxbms| Eclipse workspace. This section describes the build from command line. Details on building the binaries using the Eclipse Workspace can be found in ":ref:`getting_started_eclipse_workspace`".

The script for building targets is found in |foxbms|-setup and called ``build.py``. A help is displayed by running ``python build.py -h``.

In the |foxbms| project, several targets can be built. The output is stored in a subdirectory of ``/build``. These are

 - Primary MCU:

  - Doxygen documentation

   - This target is built with ``python build.py -p --doxygen``.
   - The output directory is ``build/primary/doxygen``.
   - The main document of the software documentation is found at ``build\primary\doxygen\html\index.html``.

  - Binaries

   - This target is built with ``python build.py -p``.
   - The output directory is ``build/primary/embedded-software``.
   - The files generated in the directory ``build/secondary/embedded-software/mcu-secondary/src/general`` are: ``foxbms.elf``, ``foxbms_flash.bin``, ``foxbms_flashheader.bin`` and ``foxbms.hex``.

 - Secondary MCU:

  - Doxygen documentation

   - This target is built with ``python build.py -s --doxygen``.
   - The output directory is ``build/secondary/doxygen``.
   - The main document of the software documentation is found at ``build\secondary\doxygen\html\index.html``.

  - Binaries

   - This target is built with ``python build.py -s``.
   - The output directory is ``build/secondary/embedded-software``.
   - The files generated in the directory ``build/secondary/embedded-software/mcu-secondary/src/general`` are: ``foxbms.elf``, ``foxbms_flash.bin``, ``foxbms_flashheader.bin`` and ``foxbms.hex``.

 - General documentation

  - Sphinx documentation

   - This target is built with ``python build.py --sphinx``.
   - The output directory is ``build/sphinx/``.
   - The main document of the software documentation is found in ``build/sphinx/documentation/doc/sphinx/html/index.html``.

Cleaning the Binaries
---------------------

This section describes how to clean from command line. Details on cleaning the binaries using the Eclipse Workspace are found in ":ref:`getting_started_eclipse_workspace`".

Cleaning the targets is done from the |foxbms|-setup directory with the ``build.py`` script, using the option ``--clean``.

The binary targets described in build can be cleaned with the following commands:

 - ``python build.py -p --clean`` cleans the primary binaries and the Doxygen documentation.
 - ``python build.py -s --clean`` cleans the secondary binaries and the Doxygen documentation.
