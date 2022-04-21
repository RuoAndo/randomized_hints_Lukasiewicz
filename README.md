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

<pre>
~/randomized_hints_Lukasiewicz# python plot.py generated3-150-60
</pre>


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


