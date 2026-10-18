# Brain Gene Expression and Alzheimer's Risk Genes

## Gene expression data
I used the Allen Human Brain Atlas data through the abagen toolbox, with the Desikan-Killiany atlas (83 regions). Only 2 of the 6 donors have right hemisphere samples, so I used bidirectional mirroring (lr_mirror='bidirectional') to copy left hemisphere data to the right. The final table has 83 regions and 15,633 genes. Each value shows how active a gene is in a region.

## Disease gene list
I chose Alzheimer's disease because it has many GWAS studies, so the gene list is large. I downloaded the associations from the GWAS Catalog (Alzheimer disease, MONDO_0004975). The file has 7,464 associations. With the standard cutoff p < 5e-8, I got 1,093 genes that are also in my expression table. That felt like too many and could include weaker genes, so I used a stricter cutoff of p < 1e-10. This gave 474 genes.

## Regional scores
For each brain region, I took the average expression of the 474 Alzheimer's genes. A higher score means these genes are more active there. The top 10 regions were all cortical (for example frontal pole, transverse temporal, parsopercularis and entorhinal). The bottom 10 were all subcortical (brainstem, accumbens, amygdala, caudate, putamen, hippocampus). Scores range from about 0.48 to 0.54, so the difference is small.

## Random gene test (cortex only)
To check if the pattern was real, I compared my 474 genes against 1,000 random gene lists of the same size. I only used cortical regions, because cortex and subcortex differ a lot in expression anyway. The most significant regions were lateral occipital, transverse temporal, lingual, isthmus cingulate and precuneus (p between 0.000 and 0.002). The entorhinal cortex, an early Alzheimer's region, was not significant (left p = 0.073, right p = 0.084). I did not correct for multiple testing, so the p-values should be read with caution.

## Conclusion

I wanted to see if Alzheimer's risk genes are more active in some brain regions than others. I used 474 genes from GWAS and compared their expression across 83 regions.

The genes were more active than random genes in several cortical areas, mainly occipital, temporal and cingulate regions (lateral occipital, transverse temporal, lingual, isthmus cingulate and precuneus). I was a bit surprised that entorhinal cortex did not come out significant (p = 0.073 and 0.084), because it is one of the first regions damaged in Alzheimer's.

I can't say this proves where Alzheimer's starts. GWAS genes only show DNA variants linked to risk, the expression data comes from 6 healthy donors, and I did not correct for multiple testing. Still, it suggests that risk genes are not spread evenly across the brain.

## Files
- `brain_gene_expression.ipynb`: full code and outputs
- `gene_list.csv`: the 474 Alzheimer's genes used
- `ahba.csv`: expression matrix (83 x 15,633), if uploaded

## Notes
abagen needed a small fix to work with newer pandas. I used an AI assistant to help debug code and understand concepts.
