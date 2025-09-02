# BAM and VCF File Region Naming Conventions

(from a conversation with an AI coding agent)

## Common Naming Conventions

### Standard chromosome naming:
- **Human**: `chr1`, `chr2`, ..., `chr22`, `chrX`, `chrY`, `chrM` (mitochondrial)
- **Alternative**: `1`, `2`, ..., `22`, `X`, `Y`, `MT` (without "chr" prefix)

### Special chromosomes:
- **Mitochondrial**: `chrM`, `MT`, or `chrMT`
- **Unplaced contigs**: `chrUn_*` or similar
- **Alternative haplotypes**: `chr*_alt`, `chr*_random`

## Key Considerations

1. **Consistency is critical** - All files in an analysis pipeline must use the same naming convention. Mixing `chr1` and `1` will cause tools to fail or produce incorrect results.

2. **Reference genome dependency** - The naming should match your reference genome:
   - UCSC references typically use `chr` prefix
   - Ensembl/NCBI often omit the prefix
   - 1000 Genomes Project uses no prefix

3. **Tool compatibility** - Some tools are strict about naming:
   - GATK can be sensitive to chromosome naming
   - Samtools/bcftools generally handle both formats
   - IGV can display both but may have display preferences

## Best Practices

- **Check your reference first**: Use `samtools view -H file.bam | head` or look at VCF headers
- **Validate consistency**: Ensure BAM, VCF, and reference FASTA all match
- **Convert if needed**: Tools like `bcftools annotate --rename-chrs` can help standardize
- **Document your choice**: Be explicit about which convention you're using

## Common Issues and Solutions

### Chromosome Name Mismatches
When working with files from different sources, you may encounter:
- BAM file uses `chr1` but VCF uses `1`
- Reference genome doesn't match your data files

### Conversion Tools
```bash
# Check chromosome names in BAM
samtools view -H file.bam | grep -E '^@SQ'

# Check chromosome names in VCF
grep -E '^##contig' file.vcf

# Rename chromosomes in VCF
bcftools annotate --rename-chrs chr_name_mapping.txt input.vcf.gz -Oz -o output.vcf.gz
```

### Creating a chromosome mapping file
```
# Example mapping file (old_name -> new_name)
1	chr1
2	chr2
3	chr3
...
X	chrX
Y	chrY
MT	chrM
```

## Summary

Proper chromosome naming is essential for:
- Tool compatibility
- Data integration
- Analysis accuracy
- Reproducible results

Always verify naming consistency across all files in your analysis pipeline before proceeding with downstream analysis.
