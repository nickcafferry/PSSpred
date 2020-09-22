PSSpred
===============

|Workflow| |Licence| |Travis| |Codecov| |Appveyor| |Gitter| |Documentation Status| |Circleci|

.. |Workflow| image:: https://github.com/nickcafferry/PSSpred/workflows/PSSpred/badge.svg
   :target: https://github.com/nickcafferry/PSSpred/actions/runs/263139727
   
.. |Licence| image:: https://img.shields.io/badge/license-MIT-blue.svg?style=flat
   :target: http://choosealicense.com/licenses/mit/
   
.. |Travis| image:: https://travis-ci.com/nickcafferry/PSSpred.svg?branch=master
   :target: https://travis-ci.com/nickcafferry/PSSpred
    
.. |Codecov| image:: https://codecov.io/gh/nickcafferry/PSSpred/branch/master/graph/badge.svg
   :target: https://codecov.io/gh/nickcafferry/PSSpred

.. |Appveyor| image:: https://ci.appveyor.com/api/projects/status/j5e243jmixcnqpy2?svg=true
   :target: https://ci.appveyor.com/project/nickcafferry/psspred

.. |Gitter| image:: https://badges.gitter.im/PSSpred/community.svg
   :target: https://gitter.im/PSSpred/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge

.. |Circleci| image:: https://circleci.com/gh/nickcafferry/PSSpred.svg?style=svg
   :target: https://circleci.com/gh/nickcafferry/PSSpred

.. |Documentation Status| image:: https://readthedocs.org/projects/psspred/badge/?version=latest
   :target: https://psspred.readthedocs.io/en/latest/?badge=latest

A simple neural network training algorithm for accurate protein secondary structure prediction.

PSSpred (Protein Secondary Structure prediction) is a simple neural network training algorithm for accurate protein secondary structure prediction. It first collects multiple sequence alignments using PSI-BLAST. Amino-acid frequence and log-odds data with Henikoff weights are then used to train secondary structure, separately, based on the Rumelhart error backpropagation method. The final secondary structure prediction result is a combination of 7 neural network predictors from different profile data and parameters. The program is freely downloadable on this page.

We have community chat at `Gitter <https://gitter.im/PSSpred/community#>`_. Feel free to ask us anything there. We have a very welcoming and helpful community.

.. raw:: html
   
    <script src="https://3Dmol.org/build/3Dmol-min.js" async></script>     
         <div style="height: 400px; width: 400px; position: relative;" class='viewer_3Dmoljs' data-href='https://tensorflow-ml.readthedocs.io/zh/latest/_static/QHD4.pdb' data-backgroundcolor='0xffffff' data-style='cartoon:color=spectrum'></div>  
         

Installation
-------

No installation is needed! 

Simply fork this project and edit the file ```seq.fasta``` (file path: src/PSSpred_v4/seq.fasta) in ```FASTA Format``` in your own repository, then you can acquire the outputs through `github worflow <https://github.com/nickcafferry/PSSpred/actions/runs/263139727>`_ in about 8 minutes, and download them via `artifacts link <https://github.com/nickcafferry/PSSpred/suites/1217285162/artifacts/18180747>`_. The output files contains two results, one for ```seq.dat```  (PSSpred prediction in I-TASSER format), one for ```seq.dat.ss```  (the original confidence file). If you want to check more results, you need to edit github workflow file `PSSPred.yml <https://github.com/nickcafferry/PSSpred/blob/master/.github/workflows/PSSPred.yml>`_:

.. image:: https://avatars3.githubusercontent.com/in/15368?s=64&v=4
   :target: https://github.com/features/actions
Github-Actions
^^^^^^^^^^^^^

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

Not familiar with ```FASTA format``` ? Don't panick, this project is very user-friendly. You can type the following protein sequence::
   
   MVLSEGEWQLVLHVWAKVEADVAGHGQDILIRLFKSHPETLEKFDRVKHLKTEAEMKASEDLKKHGVTVLTALGAILKKKGHHEAELKPLAQSHATKHKIPIKYLEFISEAIIHVLHSRHPGNFGADAQLELGAMNKAFRKDIAAKYKELGYQG

in ```seq_1.txt``` simply, and upload to the directory (path: src/PSSpred_v4/). Wait for almost 8 minutes (check Appveyor build status: pending? failed? passing?), download the `output files <https://ci.appveyor.com/project/nickcafferry/psspred/builds/35307987/artifacts>`_ when the job is done.

.. image:: https://avatars3.githubusercontent.com/ml/11?s=62&v=4
   :target: https://www.appveyor.com/
Appveyor
^^^^^^^^

.. code:: yaml
   
      image: Ubuntu
      
      install:
          - sh: cd src/
          - sh: mkdir nr
          - sh: cd nr/
          - sh: wget -O nr.tar.gz https://zhanglab.ccmb.med.umich.edu/cgi-bin/download_ftp.cgi?ID=nr.tar.gz
          - sh: tar -zxvf nr.tar.gz
          - sh: cd ../PSSpred_v4/
          - sh: ./PSSpred.pl seq_1.txt
          - sh: pwd
      
      # Skip project specific build phase.
      build: off
      
      test_script:
          - "ls"
          - "pwd"
      
      artifacts:
        - path: src\PSSpred_v4\seq.dat
          name: seq.dat
        
        - path: src\PSSpred_v4\seq.dat.ss
          name: seq.dat.ss
      
        - path: src\PSSpred_v4\protein.fasta
          name: protein.fasta

If you prefer to use CircleCI other than Appveyor, it is alright. Just edit the ```seq_2.txt``` (file path: src/PSSpred_v4/seq_2.txt) and commit. For example, you can use the following protein sequence and generatre the secondary structure prediction by your own. Also, change the ```./PSSpred.pl seq_2.txt``` to ```./PSSpred.pl XXX.txt``` if uploading input files with different file names, by editing the following ```config.yml``` file.

.. image:: https://avatars3.githubusercontent.com/ml/7?s=62&v=4
   :target: https://circleci.com/
CircleCI(file path: .circleci/config.yml)
^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: yaml
   
   version: 2

   jobs:
     build: # name of your job
       machine: # executor type
         image: ubuntu-1604:201903-01 # # recommended linux image - includes Ubuntu 16.04, docker 18.09.3, docker-compose 1.23.1
   
       steps:
         - checkout
         - run: |
               cd src/
               mkdir nr
               cd nr/
               wget -O nr.tar.gz https://zhanglab.ccmb.med.umich.edu/cgi-bin/download_ftp.cgi?ID=nr.tar.gz
               tar -zxvf nr.tar.gz
               echo "nr.tar.gz already unpacked!"
               echo "Show the path of this file:"
               pwd
               cd ../
               cd PSSpred_v4/
               ./PSSpred.pl seq_2.txt
               ls
        
         - store_artifacts:
             path: src/PSSpred_v4/seq.dat
             destination: seq.dat
             
         - store_artifacts:
             path: src/PSSpred_v4/seq.dat.ss
             destination: seq.dat.ss
   
         - store_artifacts:
             path: src/PSSpred_v4/protein.fasta
             destination: protein.fasta


Download
--------

To get the git version do

.. code:: sh
   
   $ git clone https://github.com/nickcafferry/PSSpred.git
   
Or simply download the repository using the official Github CLI

.. code:: sh

   $ gh repo clone nickcafferry/PSSpred

You can also click `here <https://zhanglab.ccmb.med.umich.edu/PSSpred/PSSpred_v4.tar.bz2>`_ to download PSSpred package version 4, and `v3 <https://zhanglab.ccmb.med.umich.edu/PSSpred/PSSpred_v3.tar.gz>`_, `v2 <https://zhanglab.ccmb.med.umich.edu/PSSpred/PSSpred_v2.tar.gz>`_, `v1 <https://zhanglab.ccmb.med.umich.edu/PSSpred/PSSpred_v1.tar.gz>`_. Also, you can download the whole package by clicking `source code.zip <https://github.com/nickcafferry/PSSpred/archive/Protein-Secondary-Structure-prediction.zip>`_ or `source code.tar.gz <https://github.com/nickcafferry/PSSpred/archive/Protein-Secondary-Structure-prediction.tar.gz>`_.


Usage
-----

Simply edit the file ```seq.fasta```, or ```seq_1.txt``` or ```seq_2.txt```, or you can upload your own sequence file and change the workflow file (PSSPred.yml, appveyor.yml, config.yml) correspondinlgy. 

About Protein Sequence
^^^^^^^^^^^^^^^^^^^^^^

Sequences are expected to be represented in the standard IUB/IUPAC amino acid and nucleic acid codes, with these exceptions:

- lower-case letters are accepted and are mapped into upper-case;
- a single hyphen or dash can be used to represent a gap of indeterminate length;
- in amino acid sequences, U and * are acceptable letters (see below).
- any numerical digits in the query sequence should either be removed or replaced by appropriate letter codes (e.g., N for unknown nucleic acid residue or X for unknown amino acid residue).


The nucleic acid codes are:
 
.. code:: python

        A --> adenosine           M --> A C (amino)
        C --> cytidine            S --> G C (strong)
        G --> guanine             W --> A T (weak)
        T --> thymidine           B --> G T C
        U --> uridine             D --> G A T
        R --> G A (purine)        H --> A C T
        Y --> T C (pyrimidine)    V --> G C A
        K --> G T (keto)          N --> A G C T (any)
                                    -  gap of indeterminate length

The accepted amino acid codes are:

.. code:: python
   
    A ALA alanine                         P PRO proline
    B ASX aspartate or asparagine         Q GLN glutamine
    C CYS cystine                         R ARG arginine
    D ASP aspartate                       S SER serine
    E GLU glutamate                       T THR threonine
    F PHE phenylalanine                   U     selenocysteine
    G GLY glycine                         V VAL valine
    H HIS histidine                       W TRP tryptophan
    I ILE isoleucine                      Y TYR tyrosine
    K LYS lysine                          Z GLX glutamate or glutamine
    L LEU leucine                         X     any
    M MET methionine                      *     translation stop
    N ASN asparagine                      -     gap of indeterminate length
    
FASTA format
------------

FASTA format is a text-based format for representing either nucleotide sequences or peptide sequences, in which base pairs or amino acids are represented using single-letter codes. A sequence in FASTA format begins with a single-line description, followed by lines of sequence data. The description line is distinguished from the sequence data by a greater-than (">") symbol in the first column. It is recommended that all lines of text be shorter than 80 characters in length.

An example sequence in FASTA format is:

.. code:: python

   >gi|186681228|ref|YP_001864424.1| phycoerythrobilin:ferredoxin oxidoreductase
   MNSERSDVTLYQPFLDYAIAYMRSRLDLEPYPIPTGFESNSAVVGKGKNQEEVVTTSYAFQTAKLRQIRA
   AHVQGGNSLQVLNFVIFPHLNYDLPFFGADLVTLPGGHLIALDMQPLFRDDSAYQAKYTEPILPIFHAHQ
   QHLSWGGDFPEEAQPFFSPAFLWTRPQETAVVETQVFAAFKDYLKAYLDFVEQAEAVTDSQNLVAIKQAQ
   LRYLRYRAEKDPARGMFKRFYGAEWTEEYIHGFLFDLERKLTVVK
   
Contributing
------------

This project welcomes contributions and suggestions. Most contributions require you to agree to a MIT LICENCE (MIT LIC) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit `Code of Conduct <https://github.com/nickcafferry/PSSpred/blob/master/CODE_OF_CONDUCT.md>`_.

Refrence
--------

Renxiang Yan, Dong Xu, Jianyi Yang, Sara Walker, Yang Zhang. A comparative assessment and analysis of 20 representative sequence alignment methods for protein structure prediction. Scientific Reports, 3: 2619 (2013). 
