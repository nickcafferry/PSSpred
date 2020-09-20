PSSpred
===============

|Workflow| |Licence| |Travis| |Codecov| |Appveyor| |Gitter|

.. |Workflow| image:: https://github.com/nickcafferry/PSSpred/workflows/PSSpred/badge.svg
   :target: https://github.com/nickcafferry/PSSpred/actions/runs/263139727
   
.. |Licence| image:: https://img.shields.io/badge/license-MIT-blue.svg?style=flat
   :target: http://choosealicense.com/licenses/mit/
   
.. |Travis| image:: https://travis-ci.com/nickcafferry/PSSpred.svg?branch=master
   :target: https://travis-ci.com/nickcafferry/PSSpred
    
.. |Codecov| image:: https://codecov.io/gh/nickcafferry/PSSpred/branch/master/graph/badge.svg
   :target: https://codecov.io/gh/nickcafferry/PSSpred

.. |Appveyor| image:: https://ci.appveyor.com/api/projects/status/j5e243jmixcnqpy2?svg=true
   :target: https://ci.appveyor.com/project/nickcafferry/psspred)

.. |Gitter| image:: https://badges.gitter.im/PSSpred/community.svg
   :target: https://gitter.im/PSSpred/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge
   

A simple neural network training algorithm for accurate protein secondary structure prediction.

PSSpred (Protein Secondary Structure prediction) is a simple neural network training algorithm for accurate protein secondary structure prediction. It first collects multiple sequence alignments using PSI-BLAST. Amino-acid frequence and log-odds data with Henikoff weights are then used to train secondary structure, separately, based on the Rumelhart error backpropagation method. The final secondary structure prediction result is a combination of 7 neural network predictors from different profile data and parameters. The program is freely downloadable on this page.

We have community chat at `Gitter <https://gitter.im/PSSpred/community#>`_. Feel free to ask us anything there. We have a very welcoming and helpful community.

Install
-------

No installation is needed! 


Download
--------

To get the git version do

.. code:: sh
   
   $ git clone https://github.com/nickcafferry/PSSpred.git
   
Or simply download the repository using the official Github CLI

.. code:: sh

   $ gh repo clone nickcafferry/PSSpred

You can also click `here <https://zhanglab.ccmb.med.umich.edu/PSSpred/PSSpred_v4.tar.bz2>`_ to download PSSpred package Version 4. 

