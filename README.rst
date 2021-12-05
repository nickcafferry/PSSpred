`PSSpred <https://github.com/nickcafferry/PSSpred>`_ 
===============

|Documentation Status| |Appveyor| |Workflow| |Licence| |Travis| |Codecov| |Gitter| |Circleci|

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

Copyright |copy| Wei MEI, |MLMS (TM)| |---|
all rights reserved. 
|bamboo|

.. |copy| unicode:: 0xA9 .. copyright sign
.. |MLMS (TM)| unicode:: MLMS U+2122
   .. with trademark sign
.. |---| unicode:: U+02014 .. em dash
   :trim:

.. |bamboo| unicode:: 0x1F024 .. bamboo

A simple `neural network training algorithm <https://www.verypossible.com/insights/machine-learning-algorithms-what-is-a-neural-network>`_ for accurate `protein secondary structure <https://proteinstructures.com/Structure/Structure/secondary-sructure.html>`_ prediction (`PSSpred <https://github.com/nickcafferry/PSSpred>`_ )! See `documentation <https://readthedocs.org/projects/psspred/badge/?version=latest>`_ for more details.

PSSpred (`Protein Secondary Structure <https://proteinstructures.com/Structure/Structure/secondary-sructure.html>`_ prediction) is a simple `neural network training algorithm <https://www.verypossible.com/insights/machine-learning-algorithms-what-is-a-neural-network>`_ for accurate `protein secondary structure <https://proteinstructures.com/Structure/Structure/secondary-sructure.html>`_ prediction. It first collects multiple sequence alignments using `PSI-BLAST <https://www.ebi.ac.uk/Tools/sss/psiblast/>`_. Amino-acid frequence and log-odds data with `Henikoff weights <https://www.sciencedirect.com/topics/biochemistry-genetics-and-molecular-biology/structural-property-of-proteins>`_ are then used to train secondary structure, separately, based on the `Rumelhart error backpropagation method <https://www.sciencedirect.com/topics/engineering/backpropagation-algorithm>`_. The final secondary structure prediction result is a combination of 7 neural network predictors from different profile data and parameters. The program is freely downloadable on this page.

We have a community chat at `Gitter <https://gitter.im/PSSpred/community#>`_. Feel free to ask us anything there. We have a very welcoming and helpful community.

.. raw:: html
   
   <div align="center">
     <img border="0"  src="https://zhanglab.ccmb.med.umich.edu/COVID-19/QHD43415_1.png" width="300">
   </div>

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
            wget -O nr.tar.gz https://zhanggroup.org/PSSpred/nr.tar.gz
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

Not familiar with ```FASTA format```? Don't panick, this project is very user-friendly. You can type the following protein sequence::
   
   MVLSEGEWQLVLHVWAKVEADVAGHGQDILIRLFKSHPETLEKFDRVKHLKTEAEMKASEDLKKHGVTVLTALGAILKKKGHHEAELKPLAQSHATKHKIPIKYLEFISEAIIHVLHSRHPGNFGADAQLELGAMNKAFRKDIAAKYKELGYQG

in ```seq_1.txt``` simply, and upload to the directory (path: src/PSSpred_v4/). Wait for almost 8 minutes (check Appveyor build status: pending? failing? passing?), download the `output files <https://ci.appveyor.com/project/nickcafferry/psspred/builds/35307987/artifacts>`_ when the job is done.

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
          - sh: wget -O nr.tar.gz https://zhanggroup.org/PSSpred/nr.tar.gz
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
               wget -O nr.tar.gz https://zhanggroup.org/PSSpred/nr.tar.gz
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

Notes
^^^^^^^^^^^

- seq.txt is fasta file at current directory (the only input file). If you know about `FASTA format`, you can always use that format.

- output files::
   
   seq.dat
   seq.dat.ss

- PSSpred.pl consists of three steps::
   
   a. prepare and run PSI-BLAST
   b. prepare mtx, pssm.txt, profw, freqccw, freqccwG
   c. run PSSpred and generate output files

Example input file
^^^^^^^^^^^^^^^^^^^^
Input file: seq_1.txt(src/PSSpred_v4/seq_1.txt)

.. code:: python
   
   MESLVPGFNEKTHVQLSLPVLQVRDVLVRGFGDSVEEVLS
   EARQHLKDGTCGLVEVEKGVLPQLEQPYVFIKRSDARTAP
   HGHVMVELVAELEGIQYGRSGETLGVLVPHVGEIPVAYRK
   VLLRKNGNKGAGGHSYGADLKSFDLGDELGTDPYEDFQEN
   WNTKHSSGVTRELMRELNGG   

Snapshot of seq.dat
^^^^^^^^^^^^^^^^^^

.. code:: python
   
       1   MET    1    9 # the first column stands for numbers in order
       2   GLU    1    9 # the second column is the amino acid code (see `About Protein Sequence` for more details)
       3   SER    1    8 # the third one represents the secondary structure code: 1<->helix, 2<->coil, 4<->strand
       4   LEU    1    8 # the fourth one represents the confidence score: 1-9
       5   VAL    1    8
       6   PRO    1    8
       7   GLY    1    8
       8   PHE    1    7
       9   ASN    1    6
      10   GLU    1    3
      11   LYS    1    1
      12   THR    4    3
      13   HIS    4    6
      14   VAL    4    8
      15   GLN    4    9
      16   LEU    4    9
      17   SER    4    8
      18   LEU    4    6
      19   PRO    4    5
      20   VAL    4    5

Snapshot of seq.dat.ss
^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python
   
           180   coil  helix  beta   # 180: the total number of sequence
                                     # Protein secondary structure: coil, helix, beta
         1 M C  0.958  0.024  0.012  # the third column: the most possible secondary structure (C-coil, H-helix, E-strand)
         2 E C  0.900  0.043  0.046  # the second column: input sequence
         3 S C  0.871  0.072  0.061  # the first column: enumeration number
         4 L C  0.872  0.064  0.067  # 4-6 columns: probability of corresponding protein secondary structure
         5 V C  0.891  0.053  0.062 
         6 P C  0.902  0.042  0.061 
         7 G C  0.886  0.046  0.070 
         8 F C  0.808  0.086  0.096 
         9 N C  0.715  0.124  0.154 
        10 E C  0.620  0.124  0.272 
        11 K C  0.546  0.053  0.416 
        12 T E  0.364  0.013  0.636 
        13 H E  0.220  0.007  0.782 
        14 V E  0.105  0.005  0.902 
        15 Q E  0.069  0.004  0.936 
        16 L E  0.076  0.005  0.928 
        17 S E  0.112  0.005  0.895 
        18 L E  0.204  0.005  0.800 
        19 P E  0.230  0.008  0.760 
        20 V E  0.229  0.012  0.760 


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

This project welcomes contributions and suggestions. Most contributions require you to agree to a `MIT LICENCE <https://github.com/nickcafferry/PSSpred/blob/master/LICENSE>`_ (MIT LIC) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit `Code of Conduct <https://github.com/nickcafferry/PSSpred/blob/master/CODE_OF_CONDUCT.md>`_.

Refrence
--------

Renxiang Yan, Dong Xu, Jianyi Yang, Sara Walker, Yang Zhang. `A comparative assessment and analysis of 20 representative sequence alignment methods for protein structure prediction <https://zhanglab.ccmb.med.umich.edu/papers/2013_18.pdf>`_. Scientific Reports, 3: 2619 (2013). 
