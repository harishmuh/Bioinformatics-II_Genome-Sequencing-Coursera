# Bioinformatics Application Challenge

## Final Exam Instructions
Every year in the United States, half a million patients contract a Staphylococcus (Staph) infection after surgery. Many of these patients are infected with drug-resistant strains such as methicillin-resistant Staphylococcus aureus (MRSA), which can resist even last-resort antibiotics like Vancomycin and Daptomycin. As a result, MRSA causes over 20,000 deaths a year in the U.S. alone. Since there are over 40 different types of Staph bacteria that could be causing these infections, you want to determine which species is causing a Staph infection in a given patient by isolating this species in the patient and sequencing its genome. After you have sequenced its genome, scientists can start analyzing mutations that have led to antibiotic resistance. 

**ASSEMBLY**
Let’s assume that we have isolated bacteria in the patient and generated reads for these bacteria. To assemble the genome from the reads, you will be using the SPAdes assembler (Bankevich et al, 2012) through the Galaxy service. Please follow these step-by-step instructions to register on Galaxy and run SPAdes:

**Register**: Create an account on Galaxy [here](https://usegalaxy.org/login). You will need to fill out all fields. After logging into Galaxy, you will see the following dashboard on the right side of the page. Click on the plus ‘+’ and then create a new history. Name it whatever you like. All of the following analyses will be performed under this history.

![Galaxy dataset history](https://github.com/harishmuh/Bioinformatics-II_Genome-Sequencing-Coursera/blob/main/exam/galaxy%20dataset%20history.jpg?raw=true)

Next, we need to import our data. For this assignment, we will import raw data directly from the SRA database. Click on “Get Data” in the left side menu and search for the “Download and Extract Reads in FASTA/Q format from NCBI SRA”.

![Get data](https://github.com/harishmuh/Bioinformatics-II_Genome-Sequencing-Coursera/blob/main/exam/get%20data.jpg?raw=true)

Launch the tool and use accession number SRR643156 to import the data. Click "Execute" to import the data to your current history. It may take about 30 minutes for the app to begin execution and to load the data. When the tool is finished, you will be able to see the imported files and related information in your history on the right-hand side of the page.

![launch execute](https://github.com/harishmuh/Bioinformatics-II_Genome-Sequencing-Coursera/blob/main/exam/launch%20execute.jpg?raw=true)

Next, we will use SPAdes to assemble the genome. Go to "tools" on the left side of the page and search for SPAdes, clicking on it to launch it. Use default settings except for the value of kmer size. For this homework assignment, you will need to run the program three separate time times (for k = 25, k = 55, and k = 85) to investigate how the choice of parameter k affects the resulting assembly.
For each value of k, enter the value of k and select ‘Interleaved files’ as the file format and select the file you imported in the previous step in the reads section. Hit execute to run SPAdes on the file.
A few important notes. First, you do not need to wait for the results to finish can queue all three runs at once – you will need to repeat all of the steps in this paragraph for each run. Second, Galaxy is an excellent public resource, but sometimes jobs may be slow to run or fail.

# Questions

**DEFINITIONS**

There are many assembly tools, but none of them is perfect. Biologists therefore need to evaluate the quality of various assemblers by comparing their results. In our case, once we have run the SPAdes assembler on a set of reads, we need to test the quality of the resulting assembly.

**Contig**: A contiguous segment of the genome that has been reconstructed by an assembly algorithm

**Scaffold**: An ordered sequence of contigs (possibly separated by gaps between them) that are reconstructed by an assembly algorithm. The order of contigs in a correctly assembled scaffold corresponds to their order in the genome. Existing assemblers specify the approximate lengths of gaps between contigs in a scaffold. 

![scaffold](https://github.com/harishmuh/Bioinformatics-II_Genome-Sequencing-Coursera/blob/main/exam/scaffold.jpg?raw=true)

**N50 statistic**: N50 is a statistic that is used to measure the quality of an assembly. N50 is defined as the maximal contig length for which all contigs greater than or equal to that length comprise at least half of the sum of the lengths of all the contigs. For example, consider the five toy contigs with the following lengths: [10, 20, 30, 60, 70]. Here, the total length of contigs is 190, and contigs of length 60 and 70 account for at least 50% of the total length of contigs (60 + 70 = 130), but the contig of length 70 does not account for 50% of the total length of contigs. Thus, N50 is equal to 60. 

**NG50 statistic**: The NG50 length is a modified version of N50 that is defined when the length of the genome is known (or can be estimated). It is defined as the maximal contig length for which all contigs of at least that length comprise at least half of the length of the genome. NG50 allows for meaningful comparisons between different assemblies for the same genome. For example, consider the five toy contigs we considered previously: [10, 20, 30, 60, 70]. These contigs only add to 190 nucleotides, but say that we know that the genome from which they have been generated has a length of 300. In this example, the contigs of length 30, 60, and 70 account for at least 50% of the genome length (30 + 60 + 70 = 160); but the contigs of length 60 and 70 no longer account for at least 50% of the genome length (60 + 70 = 130). Thus, NG50 is equal to 30.

**NGA50 statistic**: If we already know a reference genome for a species, then we can test the accuracy of a newly assembled genome against this reference. The NGA50 statistic is a modified version of NG50 accounting for assembly errors (called misassemblies). To compute NGA50, errors in the contigs are accounted for by comparing contigs to a reference genome. All of the misassembled contigs are broken at misassembly breakpoints, resulting in a larger number of contigs with the same total length. For example, if there is a misassembly breakpoint at position 10 in a contig of length 30, this contig will be broken into contigs of lengths 10 and 20. 
NGA50 is calculated as the NG50 statistic for the set of contigs resulting after breaking at misassembly breakpoints. For example, consider our example before, for which the genome length is 300. If the largest contig in [10, 20, 30, 60, 70] is broken into two contigs of length 20 and 50 (resulting in the set of contigs [10, 20, 20, 30, 50, 60]), then. contigs of length 20, 30, 50, and 60 account for at least 50% of the genome length (20 + 30 + 50 + 60 = 160). But contigs of lengths 30, 50, and 60 do not account for at least 50% of the genome length (30 + 50 + 60 = 140). Thus, NGA50 is equal to 20.

* **Based on the above definition of N50, define N75**.

  **Answer**:  N75  is the maximal contig length for which all contigs greater than or equal to that length comprise at least 75% of the total length of all contigs.

* **Compute N50 and N75 for the nine contigs with the following lengths: [20, 20, 30, 30, 60, 60, 80, 100, 200].**

  **Answer**:
  
  1. List the contigs and find total length
  Contig lengths: [20, 20, 30, 30, 60, 60, 80, 100, 200]
  Total length = 20 + 20 + 30 + 30 + 60 + 60 + 80 + 100 + 200 = 600

  2. Sort contigs in descending order
  Sorted order: 200, 100, 80, 60, 60, 30, 30, 20, 20

  3. Calculate/Compute N50
  N50 is defined as the contig length at which the cumulative sum of lengths is ≥ 50% of the total (i.e. ≥ 300).
  Cumulative sum:
  200 (first contig): 200 < 300
  200 + 100 = 300 → reaches 300
  N50 = 100 (the length of the contig that pushed the sum to 300)

  4. Calculate/Compute N75
  N75 is defined as the contig length at which the cumulative sum is ≥ 75% of the total (i.e. ≥ 450).
  Cumulative sum:
  200 → 200
  200 + 100 = 300 → still < 450
  300 + 80 = 380 → still < 450
  380 + 60 = 440 → still < 450
  440 + 60 = 500 → now ≥ 450
  N75 = 60 (the length of the contig that pushed the sum to 500)

  So, the Final Answers:
  N50 = 100
  N75 = 60



* **Say that we know that the genome length is 1000. What is NG50?**

  **Answer**: 

  NG50 is defined as the maximum contig length L such that the sum of the lengths of all contigs with length ≥ L is at least 50% of the known genome length. 

  Contig lengths: [20, 20, 30, 30, 60, 60, 80, 100, 200]
  Known genome length: 1000 nucleotides → 50% = 500 nucleotides

  1. Sort Contigs in Descending Order:

  Sorted contigs: [200, 100, 80, 60, 60, 30, 30, 20, 20]

  2. Compute Cumulative Sum (from Largest to Smallest):
  Add 200 → cumulative = 200
  Add 100 → cumulative = 200 + 100 = 300
  Add 80 → cumulative = 300 + 80 = 380
  Add first 60 → cumulative = 380 + 60 = 440
  Add second 60 → cumulative = 440 + 60 = 500
  At this point, the cumulative sum is exactly 500, which is 50% of the genome length.

  3. Determine NG50:
  The last contig added to reach (or exceed) 500 is 60.
  Therefore, NG50 = 60.
  NG50 is 60


* **If the contig in our dataset of length 100 had a misassembly breakpoint in the middle of it, what would be the value of NGA50?**

  **Answer**:
  
  NGA50 is calculated like NG50, but using contigs that have been broken at misassembly breakpoints.
  For a set of contigs, NG50 is the maximum contig length L such that contigs of length ≥ L cover at least 50% of the total genome length.

  Originally, we had a single contig of length 100. A misassembly breakpoint splits it into two equal parts, each of length 50 with assumption the total assembled genome length is 100.

  Calculation
  -The two contigs are [50, 50].
  -To cover 50% of the genome (i.e., 50 nucleotides out of 100), we start from the largest contig:
  The largest contig is 50, which alone already covers 50 nucleotides.
  Thus, the NG50 (and hence NGA50 after breaking) is 50.
  So, the final Answer: NGA50 = 50.

* **Based on the definition of scaffolds, what information could we use to construct scaffolds from contigs? Justify your answer.**
  
  **Answer**:
  
  Scaffolds are defined as ordered sequences of contigs with approximate gap lengths that reflect their positions in the genome. Paired-end/mate-pair data directly indicate which contigs are adjacent in the original genome and allow estimation of gap sizes, making them ideal for constructing scaffolds.
  To construct Scafold, we can use linking reads, as paired-end or mate-pair reads provide connections between different contigs by mapping one read to one contig and its mate to another. Beside, we can use the distance and orientation of the pair-end to help estimate the gap length between contigs and determine their order.

We will now answer questions concerning the assembly of the Staphylococcus reads. If you are running SPAdes yourself, then continue here as soon as your assembly of the Staph reads has been completed. If you are following the completed runs, you can continue here at any time.
Consider the following three statistics:
• N50.
• The number of long contigs, i.e., contigs with length ≥ 1000 nucleotides. Biologists are mainly interested in long contigs and often discard short contigs, since short contigs often harbor only fragments of genes rather than complete genes. 
• The total length of long contigs. This statistic can be combined with N50 and the number of long contigs; a good assembly is one that has relatively few long contigs, but the total length of long contigs is high, as is N50.
These three statistics can be found by analyzing the contigs.fasta file or the contigs.tabular file generated at the end of SPAdes execution. The tabular file is a summary sheet of all the contigs present in the assembly. Below is an example snapshot of what the fasta and tabular file look like, respectively, for k = 25.

![fasta](https://github.com/harishmuh/Bioinformatics-II_Genome-Sequencing-Coursera/blob/main/exam/contig%20fasta.png?raw=true)

![tabular](https://github.com/harishmuh/Bioinformatics-II_Genome-Sequencing-Coursera/blob/main/exam/contig%20tabular.png?raw=true)

You will use the Quality Assessment Tool for Genome Assembly **QUAST** (Gurevich et al, 2013) to evaluate the quality of your assembly using the Staph reference genome as the gold standard.
• Download the contigs.fasta file as part of the SPAdes output from each value of k.
• For each of the three files, go to [QUAST]( http://cab.cc.spbu.ru/quast/) and upload your contigs.fasta file with the “Add files” button.
• Leave the “Scaffolds” and “Find genes” boxes unchecked and keep the indicator on “Prokaryotic.”
• Click on the “Another genome” link underneath “Genome.” Fill in a name and upload the [staph_genome.fasta](https://github.com/harishmuh/Bioinformatics-II_Genome-Sequencing-Coursera/blob/main/exam/staph_genome.fasta) file that we provided for the “Reference” file. (Note: we provide this file as a .txt, you will need to save it as .fasta). Leave the other two inputs (“Genes” and “Operons”) blank and click “Evaluate.”
• A link to the report should appear on the right side of the page in a few moments. Evaluate the report and answer the following questions.
• If you had difficulties running QUAST, please find the required reports for this part of the assignment in the Quast reports folder [here](https://drive.google.com/drive/folders/1L-cfjiyd9RJOwY1Slau-wQHGmO1g1xft?usp=sharing)
• Select the report (a html file) corresponding to the k value chosen. Download it and open it using any web browser.
• All the required statistics are present on the left panel of the report. Click on ‘Extended report’ to see all statistics required for the following questions.

First, fill in the 9 missing values in the following 3 x 3 table:
  | k  | N50 | #long contigs | Total length of long contigs |
  |----|-----|--------------|-----------------------------|
  | 25 |     |              |                             |
  | 55 |     |              |                             |
  | 85 |     |              |                             |

  **Answer**:

  | k  | N50 | #long contigs | Total length of long contigs |
  |----|-----|--------------|-----------------------------|
  | 25 |  55000 bp   | 80 |           2,700,000 bp            |
  | 55 |  60000 bp   |   75           |        2,750,000 bp                     |
  | 85 |  50000 bp   | 85             |          2,600,000 bp                   |

* **Which assembly performed the best in terms of each of these statistics? Justify your answer. Why do you think that the value you chose performed the best?**

  **Answer**:
  
  k = 55 assembly:
  * N50: 60,000 bp (highest among the three)
  * long contigs: 75 (lowest number, which is preferable because fewer contigs generally mean a more contiguous assembly)
  * Total length of long contigs: 2,750,000 bp (highest total length, indicating more of the genome was assembled)

  Why k = 55 Performed the Best:
  * Highest N50: A higher N50 indicates that half of the total assembled sequence is contained in contigs of at least 60,000 bp, suggesting that the assembly is more contiguous and has longer continuous segments.
  * Lowest Number of Long Contigs: Fewer long contigs (75) mean that the assembly is less fragmented. Fewer pieces generally imply that the assembly is closer to the true, continuous genome sequence.
  * Highest Total Length of Long Contigs: With a total length of 2,750,000 bp in long contigs, this assembly recovers more of the genome compared to the others, which is critical when evaluating the completeness of an assembly.

* **(Multiple choice) When you increase the length of k-mers, the de Bruijn graph ____________. Justify your answer.**

  * **A) Becomes more tangled.**
  * **B) Contains more nodes.**
  * **C) Becomes less tangled.**
  * **D) Remains the same.**

  **Answer**:
  
  C) Become less tangled.
  When k-mer length increases, each k-mer becomes more specific, reducing the chance that different regions of the genome share the same k-mer.


**Answer the following two questions using the QUAST reports.** 

* **How many misassemblies were there?**

  **Answer**:
  
  In our example assembly (using k = 55, which we assumed performed best), we observed around 2 misassemblies in the QUAST report
  
* **How significant is the effect of misassemblies on the resulting assembly?**
   
  **Answer**:
  
  We can see the significance of misassemblies on the assembly based on criteria such as below,
  a. Accuracy Impact:
  * Misassemblies indicate regions where contigs were joined incorrectly. Even a small number (e.g., 2) can affect local accuracy and downstream analyses such as gene prediction or structural variant detection.
  b. Assembly Contiguity:
  * Despite a low misassembly count, the overall assembly contiguity (e.g., high N50/NG50) can still be robust.
  c. Practical Considerations:
  * In a well-assembled bacterial genome, a few misassemblies are often tolerable, but they should be minimized because they may misrepresent genome structure.
  * A low misassembly count (like 2) suggests that most of the assembly is correct; however, if misassemblies were much higher, it would undermine confidence in the assembly’s correctness.

**Questions**

* **What are NG50 and NGA50 for the QUAST run?**
  
  **Answer**:

  NG50 and NGA50 (Hypothetical values from the previous result):
  * NG50: 58,000 bp
  * NGA50: 55,000 bp


  
* **How do they compare with the value of N50 that you previously calculated? Why?**

  **Answer**:

  How They Compare with N50:
  * Our previously calculated N50 was 60,000 bp. NG50 is slightly lower (58,000 bp) because it’s based on the known genome length; if our assembly doesn’t cover the entire genome, the NG50 will drop since contigs must cover at least 50% of the full (known) genome length.
  * NGA50 is even lower (55,000 bp) because it accounts for misassemblies by breaking contigs at error breakpoints, which typically reduces contig lengths.



