.. image:: https://github.com/nickcafferry/PSSpred/workflows/PSSpred/badge.svg?event=workflow_run
   :target: 

PSSpred
===============

PSSpred (Protein Secondary Structure prediction) is a simple neural network training algorithm for accurate protein secondary structure prediction. It first collects multiple sequence alignments using PSI-BLAST. Amino-acid frequence and log-odds data with Henikoff weights are then used to train secondary structure, separately, based on the Rumelhart error backpropagation method. The final secondary structure prediction result is a combination of 7 neural network predictors from different profile data and parameters. The program is freely downloadable at the bottom of this page.
