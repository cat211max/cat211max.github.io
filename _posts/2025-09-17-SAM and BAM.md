Sequence Alignment/Map format
BAM: binary, compressed version of SAM 
+ `Samtools`: set of programs for interacting with `SAM` and `BAM` files 
	+ `samtools view -H NA20538.bam`
+ store the read alignments to a reference genome 
+ store NGS sequencing reads, base qualities
+ header section (optional)
	+ alignment section: contains one record (a single ) 
	+ `key:type:value` tuple

# File structure 

## Header section
starts `@`, followed by a two-letter header record code as defined in https://samtools.github.io/hts-specs/SAMv1.pdf

```bash
@HD VN:1.0 SO:coordinate  
@SQ SN:test_ref LN:17637  
@RG ID:ERR003612 PL:ILLUMINA LB:g1k-sc-NA20538-TOS-1 PI:2000 DS:SRP000540 SM:NA20538 CN:SC
```
+ useful record type: 
+ RG: read group. describe each lane of sequencing 
+ 
## Alignment section   

```bash
ERR005816.1408831 163 Chr1 19999970 23 40M5D30M2I28M =  
20000147 213 GGTGGGTGGATCACCTGAGATCGGGAGTTTGAGACTAGGTGG...  
<=@A@??@=@A@A>@BAA@ABA:>@<>=BBB9@@2B3<=@A@... 
```
![cols of Sam](images/col_of_sam_visual.png)

+ FLAG: pairing, strand, mate strand 
+ RNAME: reference sequence NAME
+ POS: 
+ MAPQ: Phred-scaled 

### CIGAR string 
- M alignment match or mismatch  
- = sequence match  
- X sequence mismatch  
- I insertion into the read (sample sequenced)  
- D deletion from the read (sample sequenced)  
- S soft clipping (clipped sequences *present* in SEQ)  
- H hard clipping (clipped sequences *NOT present* in SEQ)  
- N skipped region from the reference  
- P padding (silent deletion from padded reference)

### Flags 

- 0x1 1 PAIRED paired-end (or multiple-segment) sequencing technology  
- 0x2 2 PROPER_PAIR each segment properly aligned according to the aligner  
- 0x4 4 UNMAP segment unmapped  
- 0x8 8 MUNMAP next segment in the template unmapped  
- 0x10 16 REVERSE SEQ is reverse complemented  
- 0x20 32 MREVERSE SEQ of the next segment in the template is reversed  
- 0x40 64 READ1 the first segment in the template  
- 0x80 128 READ2 the last segment in the template  
- 0x100 256 SECONDARY secondary alignment  
- 0x200 512 QCFAIL not passing quality controls  
- 0x400 1024 DUP PCR or optical duplicate  
- 0x800 2048 SUPPLEMENTARY supplementary alignment

# CARM
+ better compression of genomic data than SAM/BAM
+ using three important concepts
	+ reference based compression 
	+ controlled loss of quality information 

how reference-based compression works 
![CRAM reference based compression](images/CRAM_reference_based_compression.png|525)
