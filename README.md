# randomized_hints_Lukasiewicz

<pre>
An Experiment of Randomized Hints on an Axiom of Infinite-Valued Lukasiewicz Logic
https://arxiv.org/abs/2204.08512
</pre>

# How it run
<pre>
Get OTTER souce code and binary executable.

# wget https://www.mcs.anl.gov/research/projects/AR/otter/dist33/otter-3.3f.tar.gz
# cd otter-3.3f/source
# make 
# make install

Or

# apt install otter

And just type:

# ./test.sh

That's all!
</pre>

<pre>
:~/randomized_hints_Lukasiewicz# time ./test.sh 

-------- PROOF -------- 30888 [binary,30887.1,6.1] $ANS(thm_MV5).

Search stopped by max_proofs option.

clauses generated         788576
sos size                   28482

-------- PROOF -------- 2442 [binary,2441.1,6.1] $ANS(thm_MV5).

Search stopped by max_proofs option.

clauses generated           8352
sos size                    2230

-------- PROOF -------- 6464 [binary,6463.1,6.1] $ANS(thm_MV5).

Search stopped by max_proofs option.

clauses generated          37510
sos size                    6001

real	15m24.567s
user	15m19.551s
sys	0m6.414s
</pre>

You get 150 scripts (tmp-*-60.*) with the 60 hints.

<pre>
~/randomized_hints_Lukasiewicz# ls 
auto.sh                tmp-150-60.102  tmp-150-60.125  tmp-150-60.148  tmp-150-60.36  tmp-150-60.59  tmp-150-60.81
auto.sh~               tmp-150-60.103  tmp-150-60.126  tmp-150-60.149  tmp-150-60.37  tmp-150-60.6   tmp-150-60.82
generated-150-60       tmp-150-60.104  tmp-150-60.127  tmp-150-60.15   tmp-150-60.38  tmp-150-60.60  tmp-150-60.83
generated2-150-60      tmp-150-60.105  tmp-150-60.128  tmp-150-60.16   tmp-150-60.39  tmp-150-60.61  tmp-150-60.84
generated3-150-60      tmp-150-60.106  tmp-150-60.129  tmp-150-60.17   tmp-150-60.4   tmp-150-60.62  tmp-150-60.85
generated3-150-60.png  tmp-150-60.107  tmp-150-60.13   tmp-150-60.18   tmp-150-60.40  tmp-150-60.63  tmp-150-60.86
generate.sh            tmp-150-60.108  tmp-150-60.130  tmp-150-60.19   tmp-150-60.41  tmp-150-60.64  tmp-150-60.87
hints                  tmp-150-60.109  tmp-150-60.131  tmp-150-60.2    tmp-150-60.42  tmp-150-60.65  tmp-150-60.88
hints-150-60           tmp-150-60.11   tmp-150-60.132  tmp-150-60.20   tmp-150-60.43  tmp-150-60.66  tmp-150-60.89
manyval.first          tmp-150-60.110  tmp-150-60.133  tmp-150-60.21   tmp-150-60.44  tmp-150-60.67  tmp-150-60.9
otter                  tmp-150-60.111  tmp-150-60.134  tmp-150-60.22   tmp-150-60.45  tmp-150-60.68  tmp-150-60.90
plot.py                tmp-150-60.112  tmp-150-60.135  tmp-150-60.23   tmp-150-60.46  tmp-150-60.69  tmp-150-60.91
README.md              tmp-150-60.113  tmp-150-60.136  tmp-150-60.24   tmp-150-60.47  tmp-150-60.7   tmp-150-60.92
sort2.sh               tmp-150-60.114  tmp-150-60.137  tmp-150-60.25   tmp-150-60.48  tmp-150-60.70  tmp-150-60.93
sort.sh                tmp-150-60.115  tmp-150-60.138  tmp-150-60.26   tmp-150-60.49  tmp-150-60.71  tmp-150-60.94
sos-150-60             tmp-150-60.116  tmp-150-60.139  tmp-150-60.27   tmp-150-60.5   tmp-150-60.72  tmp-150-60.95
sos2-150-60            tmp-150-60.117  tmp-150-60.14   tmp-150-60.28   tmp-150-60.50  tmp-150-60.73  tmp-150-60.96
sos3-150-60            tmp-150-60.118  tmp-150-60.140  tmp-150-60.29   tmp-150-60.51  tmp-150-60.74  tmp-150-60.97
test.sh                tmp-150-60.119  tmp-150-60.141  tmp-150-60.3    tmp-150-60.52  tmp-150-60.75  tmp-150-60.98
tmp-150-60             tmp-150-60.12   tmp-150-60.142  tmp-150-60.30   tmp-150-60.53  tmp-150-60.76  tmp-150-60.99
tmp-150-60.0           tmp-150-60.120  tmp-150-60.143  tmp-150-60.31   tmp-150-60.54  tmp-150-60.77  tmp-out-150-60
tmp-150-60.1           tmp-150-60.121  tmp-150-60.144  tmp-150-60.32   tmp-150-60.55  tmp-150-60.78
tmp-150-60.10          tmp-150-60.122  tmp-150-60.145  tmp-150-60.33   tmp-150-60.56  tmp-150-60.79
tmp-150-60.100         tmp-150-60.123  tmp-150-60.146  tmp-150-60.34   tmp-150-60.57  tmp-150-60.8
tmp-150-60.101         tmp-150-60.124  tmp-150-60.147  tmp-150-60.35   tmp-150-60.58  tmp-150-60.80
</pre>

And

<pre>
~/randomized_hints_Lukasiewicz# python plot.py generated3-150-60
</pre>

<img src="generated3-150-60.png">

# script1: test.sh

<pre>
rm -rf seq*
rm -rf tmp*

rm -rf generated*
rm -rf sos*

./auto.sh 150 60 & 
wait
</pre>

# script2: auto.sh

<pre>
#!/bin/bash

COUNT=0

while [ $COUNT -lt ${1} ]; do
    ./generate.sh ${1} ${2}; ./otter < tmp-${1}-${2} > tmp-out-${1}-${2};

    grep -a "clauses generated" tmp-out-${1}-${2} | grep -v Binary | tee -a generated-${1}-${2}
    grep -v Binary generated-${1}-${2} > generated2-${1}-${2}
    
    grep -a "sos size" tmp-out-${1}-${2} | grep -v Binary | tee -a sos-${1}-${2}
    grep -v Binary sos-${1}-${2} > sos2-${1}-${2}

    cp tmp-${1}-${2} tmp-${1}-${2}.${COUNT}

    COUNT=`expr $COUNT + 1` # COUNT ðCNg
done

cat generated2-${1}-${2} | awk '{print $3}' > generated3-${1}-${2}

cat sos2-${1}-${2} | awk '{print $3}' > sos3-${1}-${2}
</pre>

# script3: generate.sh

<pre>
cat manyval.first > tmp-${1}-${2}
echo "" >> tmp-${1}-${2}

shuf -n ${2} hints > hints-${1}-${2}
cat hints-${1}-${2} >> tmp-${1}-${2}

#shuf -n ${1} hints >> tmp-${1}-${2}

echo "end_of_list." >> tmp-${1}-${2}
</pre>


