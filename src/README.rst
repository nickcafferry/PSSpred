This is a script program for protein secondary structure prediction (PSSpred) based on multiple weight neural-network training. It first collects multiple homologous sequences using  PSI-BLAST. Amino-acid frequence and log-odds data with Henikoff weights are then used to train SS separately based on the Rumelhart back-propagation of erros method. The final SS prediction is a  combination of 7 neural network SS predictors.  Comments should be addressed to zhng@umich.edu.

1. how to install PSSpred?
   
   - download 'PSSpred_v3.tar.gz' at `this link <http://zhanglab.ccmb.med.umich.edu/PSSpred/>`_

   - unpack the PSSpred files by "tar -zxvf PSSpred_v3.tar.gz"

   - download and install non-redundant sequence file at `this link <http://zhanglab.ccmb.med.umich.edu/cgi-bin/download_ftp.cgi?ID=nr.tar.gz>`_

   - download and install psi-blast program at `this link <http://zhanglab.ccmb.med.umich.edu/PSSpred/blastv2.6.tar.gz>`_

   - change the path ($blastdir, $db, $PSSpreddir) in 'PSSpred.pl'

2. how to run 'PSSpred.pl' in Linux system?

.. code:: sh

    $ PSSpred.pl seq.txt

   Note: 

     a. seq.txt is fasta file at current directory (the only input file)

     b. output file:
        seq.dat
        seq.dat.ss

     c. PSSpred.pl is split into three steps:
        Step 1: prepare and run PSI-BLAST
        Step 2: prepare mtx, pssm.txt, profw, freqccw, freqccwG
        Step 3: run PSSpred and output files

3. how to cite PSSpred?

   PSSpred has not been published as an article. Please cite it as Y. Zhang, http://zhanglab.ccmb.med.umich.edu/PSSpred 

