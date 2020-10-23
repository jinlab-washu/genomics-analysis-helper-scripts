# Using HaplotypeCaller to Call Small Nucleotide Variations (SNVs)

#### Command below "exports" the directories in the LSF_DOCKER_VOLUMES variable so that the docker image can access them. Make sure to replace $GROUP with your compute group. 
*If you are not sure what your compute group is run the command `groups`.* The output should look something like this:

Your compute group should be named something like this `compute-$lab` where $lab is the directory given to your lab or PI.

1. Export your group as a 'variable' named GROUP. Replace YOUR_GROUP with your actual group name 

    > If you look at Week5, we also ran this command. For simplicity when running the following commands, let's set a variable for the group.

    `export GROUP=YOUR_GROUP`
    
    > To access a variable's value in linux, we use the dollar sign `$`. If we set `GROUP=compute-group2`, $GROUP will equal compute-group2. You can double check this by running the command `echo $GROUP`.
    
2. Run the command below to start an interactive session with the GATK4 docker image. The GATK4 version is 4.1.9.0
    ```
    LSF_DOCKER_VOLUMES="$HOME:$HOME /scratch1/fs1/$GROUP:/scratch1/fs1/$GROUP /storage1/fs1/$GROUP/Active:/storage1/fs1/$GROUP/Active /storage1/fs1/bga/Active:/storage1/fs1/bga/Active" \
    bsub -G compute-$GROUP -a 'docker(broadinstitute/gatk:4.1.9.0)' -M 8000M -R 'select[mem>8000M] rusage[mem=8000M]' -Is -q general-interactive /bin/bash
    ```
2. Next Run the --list commmand to get a list of all the tools.

    ` /gatk/gatk --list`
    
3. Now lets do some Genotyping! Lets call SNVs with GATK HaplotypeCaller for our example cram alignment file.
    
    Run the command below:
    
        /gatk/gatk --java-options "-Xmx4g" HaplotypeCaller -R /gscmnt/gc2560/core/model_data/ref_build_aligner_index_data/2887491634/build21f22873ebe0486c8e6f69c15435aa96/aligner-index-blad8-1-1.gsc.wustl.edu-tmooney-331-75fff591a14f4f7c910247fc39c4ea7f/bwamem/0_7_15/all_sequences.fa -I ~/chr13.cram -O chr13_test.g.vcf.gz -L chr13
    
    Here the required inputs for HaplotypeCaller are a reference fasta file `-R`, an input alignment file (bam/cram/sam) `-I`, and and output file name `-O` to specify the name of the output vcf file. We also supply two additionally paramters. `--java-options` and `-L`. The java-options specifies the amount of "heap" memory we are suppling to run the command. This is a little advanced, but we are supplying the program with enough memory to run. GATK documents have good defaults for these on their tools page. See [here](https://gatk.broadinstitute.org/hc/en-us/categories/360002369672). The `-L` options specifies the genomic intervals we want haplotype caller to make calls for. Here we give it chr13 as our alignment file was downsized to only include alignments for chr13.

    
