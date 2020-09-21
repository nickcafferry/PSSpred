PSSpred
===============

|Workflow| |Licence| |Travis| |Codecov| |Appveyor| |Gitter| |Circleci|

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

.. |Circleci| image:: https://circleci.com/gh/nickcafferry/PSSpred.svg?style=svg
   :target: https://circleci.com/gh/nickcafferry/PSSpred

A simple neural network training algorithm for accurate protein secondary structure prediction.

PSSpred (Protein Secondary Structure prediction) is a simple neural network training algorithm for accurate protein secondary structure prediction. It first collects multiple sequence alignments using PSI-BLAST. Amino-acid frequence and log-odds data with Henikoff weights are then used to train secondary structure, separately, based on the Rumelhart error backpropagation method. The final secondary structure prediction result is a combination of 7 neural network predictors from different profile data and parameters. The program is freely downloadable on this page.

We have community chat at `Gitter <https://gitter.im/PSSpred/community#>`_. Feel free to ask us anything there. We have a very welcoming and helpful community.

Installation
-------

No installation is needed! 

Simply fork this project and edit the file `seq.fasta` in `FASTA Format` in your own repository, then you can acquire the outputs through `github worflow <https://github.com/nickcafferry/PSSpred/actions/runs/263139727>`_ , and download them via `artifacts link <https://github.com/nickcafferry/PSSpred/suites/1217285162/artifacts/18180747>`_. The output files contains two results, one for `seq.dat` (PSSpred prediction in I-TASSER format), one for `seq.dat.ss` (the original confidence file). If you want to check more results, you need to edit github workflow file `PSSPred.yml <https://github.com/nickcafferry/PSSpred/blob/master/.github/workflows/PSSPred.yml>`_.

.. image:: https://avatars3.githubusercontent.com/in/15368?s=64&v=4
   :target: https://github.com/features/actions
Github-Actions

.. code:: yaml
   
   name: PSSpred

   on:
     push:
       branches:
         - master
   
   jobs:
     build_docs_and_deploy:
       runs-on: ubuntu-latest
       name: running PSSpred
   
       steps:
       - name: Checkout
         uses: actions/checkout@master
   
       - name: running perl
         run: |
            echo "Initializing the program....................."
            
            echo "---------------------------------------------"
            cd ../
            mkdir output
            echo "output file already created!"
            
            echo "---------------------------------------------"
            cd PSSpred/
            cd src/
            mkdir nr
            cd nr/
            wget -O nr.tar.gz https://zhanglab.ccmb.med.umich.edu/cgi-bin/download_ftp.cgi?ID=nr.tar.gz
            tar -zxvf nr.tar.gz
            echo "nr.tar.gz already unpacked!"
            echo "Show the path of this file: "
            pwd
            
            cd ../
            cd PSSpred_v4/
            ./PSSpred.pl seq.fasta
            cp seq.dat /home/runner/work/PSSpred/output/
            cp seq.dat.ss /home/runner/work/PSSpred/output/
            cp blast.out /home/runner/work/PSSpred/output/
            cd /home/runner/work/PSSpred/output/
            ls
            pwd
            
       - uses: actions/upload-artifact@v2
         with:
           name: output results
           path: /home/runner/work/PSSpred/output/ 

Download
--------

To get the git version do

.. code:: sh
   
   $ git clone https://github.com/nickcafferry/PSSpred.git
   
Or simply download the repository using the official Github CLI

.. code:: sh

   $ gh repo clone nickcafferry/PSSpred

You can also click `here <https://zhanglab.ccmb.med.umich.edu/PSSpred/PSSpred_v4.tar.bz2>`_ to download PSSpred package Version 4. 

