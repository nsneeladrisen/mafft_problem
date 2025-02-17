## Problem

- Create an MSA of the query domain with members within its superfamily

## What I expected to see

- MSA containing the query domain and its homologues from the superfamily

## What I actually got

- MAFFT is throwing an error stating

```
inputfile = orig
53434 x 2723 - 219 p
nadd = 1
nthread = 0
nthreadpair = 0
nthreadtb = 0
ppenalty_ex = 0
stacksize: 8192 kb->10436 kb
rescale = 1
Gap Penalty = -1.53, +0.00, +0.00



Making a distance matrix ..

There are 546 ambiguous characters.
 3001 / 53434/SAN/cath/cath_v4_3_0/neel/mafft_installation/mafft_installation/bin/mafft: line 2718: 20532 Killed                  "$prefix/disttbfast" -q $npickup -E $cycledisttbfast -V "-"$gopdist -s $unalignlevel $legacygapopt $mergearg -W $tuplesize $termgapopt $outnum $addarg $add2ndhalfarg -C $numthreads-$numthreadstb $memopt $weightopt $treeinopt $treeoutopt $distoutopt $seqtype $model -g $gexp -f "-"$gop -Q $spfactor -h $aof $param_fft $algopt $treealg $scoreoutarg $anchoropt -x $maxanchorseparation $oneiterationopt < infile > pre 2>> "$progressfile"
 ```

## Minimal steps to reproduce

Use MAFFT to align the query sequence (Q12767-1.20.1110.10-FF-000019.fasta) to the the aligned fasta (Q12767-1.20.1110.10-FF-000019.fas) created in the previous step. This creates the aligned query with the MSA in Q12767-1.20.1110.10-FF-000019-quey.fas - 

```
/home/ucbtenx/mafft/bin/mafft --add /home/ucbtenx/cath_domain_fasta/$a --reorder /home/ucbtenx/hmmsearch_results/$b.fas> /home/ucbtenx/hmmsearch_results/$b-query.fas
```

## Environment

- HMMER - hmmer-3.3.2
- MAFFT - mafft-7.475-with-extensions
- hh-suite - https://github.com/soedinglab/hh-suite (dated ~Aug 2020)
- neff - https://github.com/gjoni/gremlin3

## Dataset
1. Build the hmm from the funfam MSA using 1.20.1110.10-FF-000019.faa as input and 1.20.1110.10-FF-000019.hmm as output - 

```
/home/ucbtenx/hmmer-3.3.2/bin/hmmbuild /home/ucbtenx/required_funfams/$sf'-FF-'$ff'.hmm' /home/ucbtenx/required_funfams/$sf'-FF-'$ff'.faa'
```

2. Search the superfamily (1.20.1110.10.all_sequences) for homologues using the hmm (1.20.1110.10-FF-000019.hmm) built in the previous step to produce a stockholm alignment in Q12767-1.20.1110.10-FF-000019.sto- 

```
/home/ucbtenx/hmmer-3.3.2/bin/hmmsearch -E 0.001 --domE 0.001 --incdomE 0.001 -A /home/ucbtenx/hmmsearch_results/$b.sto /home/ucbtenx/required_funfams/$sf'-FF-'$ff'.hmm' /home/ucbtenx/superfamily/$sf.all_sequences
```

4. Reformat the sto alignment from the previous step (Q12767-1.20.1110.10-FF-000019.sto) into an aligned fasta (Q12767-1.20.1110.10-FF-000019.fas) using hh-suites reformat.pl script - 

```
perl /home/ucbtenx/reformat.pl /home/ucbtenx/hmmsearch_results/$b.sto /home/ucbtenx/hmmsearch_results/$b.fas
```

5. Use MAFFT to align the query sequence (Q12767-1.20.1110.10-FF-000019.fasta) to the the aligned fasta (Q12767-1.20.1110.10-FF-000019.fas) created in the previous step. This creates the aligned query with the MSA in Q12767-1.20.1110.10-FF-000019-quey.fas - 

```
/home/ucbtenx/mafft/bin/mafft --add /home/ucbtenx/cath_domain_fasta/$a --reorder /home/ucbtenx/hmmsearch_results/$b.fas> /home/ucbtenx/hmmsearch_results/$b-query.fas
```

