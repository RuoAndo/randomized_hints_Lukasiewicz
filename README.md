# randomized_hints_Lukasiewicz

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
</pre>

# test.sh

<pre>
rm -rf seq*
rm -rf tmp*

rm -rf generated*
rm -rf sos*

./auto.sh 150 60 & 
wait
</pre>

# auto.sh

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

# generate.sh

<pre>
cat manyval.first > tmp-${1}-${2}
echo "" >> tmp-${1}-${2}

shuf -n ${2} hints > hints-${1}-${2}
cat hints-${1}-${2} >> tmp-${1}-${2}

#shuf -n ${1} hints >> tmp-${1}-${2}

echo "end_of_list." >> tmp-${1}-${2}
</pre>


