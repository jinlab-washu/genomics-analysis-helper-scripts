# Using HaplotypeCaller to Call Small Nucleotide Variations (SNVs)

#### Command below "exports" the directories in the LSF_DOCKER_VOLUMES variable so that the docker image can access them. Make sure to replace $GROUP with your compute group. 
*If you are not sure what your compute group is run the command `groups`.* The output should look something like this:

Your compute group should be named something like this `compute-$lab` where $lab is the directory given to your lab or PI.

1. Run the command below to start an interactive session with the GATK4 docker image. The GATK4 version is 4.1.8.1
    ```
    LSF_DOCKER_VOLUMES="$HOME:$HOME /scratch1/fs1/$GROUP:/scratch1/fs1/$GROUP /storage1/fs1/$GROUP/Active:/storage1/fs1/$GROUP/Active /storage1/fs1/bga/Active:/storage1/fs1/bga/Active" \
    bsub -G compute-$GROUP -a 'docker(broadinstitute/gatk:4.1.9.0)' -M 8000M -R 'select[mem>8000M] rusage[mem=8000M]' -Is -q general-interactive /bin/bash
    ```
2. Next Run the --list commmand to get a list of all the tools.

    ` /gatk/gatk --list`
    
3. Now lets do some Genotyping! Using the data from last week, lets call SNV with GATK HaplotypeCaller.

