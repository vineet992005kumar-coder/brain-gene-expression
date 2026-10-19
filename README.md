# Brain Gene Expression and Alzheimer's Risk Genes

## Gene expression data
I used the Allen Human Brain Atlas data through the abagen toolbox, with the Desikan-Killiany atlas (83 regions). Only 2 of the 6 donors have right hemisphere samples, so I used bidirectional mirroring (lr_mirror='bidirectional') to copy left hemisphere data to the right. The final table has 83 regions and 15,633 genes. Each value shows how active a gene is in a region.

## Disease gene list
I chose Alzheimer's disease because it has many GWAS studies, so the gene list is large. I downloaded the associations from the GWAS Catalog (Alzheimer disease, MONDO_0004975). The file has 7,464 associations. With the standard cutoff p < 5e-8, I got 1,093 genes that are also in my expression table. That felt like too many and could include weaker genes, so I used a stricter cutoff of p < 1e-10. This gave 837 unique genes, of which 474 were found in my expression table (about 57%). I used these 474 genes for the rest of the analysis.

## Regional scores
For each brain region, I took the average expression of the 474 Alzheimer's genes. A higher score means these genes are more active there. The top 10 regions were all cortical (for example frontal pole, transverse temporal, parsopercularis and entorhinal). The bottom 10 were all subcortical (brainstem, accumbens, amygdala, caudate, putamen, hippocampus). Scores range from about 0.48 to 0.54, so the difference is small.

## Random gene test
To check if the pattern was real, I compared my 474 genes against random gene lists of the same size. As a first check I used 1,000 random lists on cortical regions only. There the most significant regions were transverse temporal, frontal pole (right), lateral occipital, isthmus cingulate, superior temporal and lingual (p between 0.000 and 0.005), and the entorhinal cortex was borderline (left p = 0.051, right p = 0.050).

Then I ran the full test on all 83 regions with 10,000 random gene lists. Each region is compared only with random scores from the same region, so the difference between cortex and subcortex does not matter. For each region I calculated a z-score (how many standard deviations my genes are above random genes) and a p-value. Because I tested 83 regions, I corrected the p-values with the Benjamini-Hochberg FDR method.

The top 10 regions by z-score were all cortical: transverse temporal (left and right, z about 3.1), right frontal pole (2.79), lateral occipital (left 2.78, right 2.65), isthmus cingulate (right 2.77, left 2.53), lingual (right 2.52, left 2.52) and left superior temporal (2.43). Only 2 of 83 regions were significant after correction: the left and right transverse temporal cortex (corrected p = 0.041). The other eight regions in the top 10 have a corrected p of 0.055, just above the 0.05 cutoff. In the brain map of z-scores the cortex looks the reddest.

For the known early Alzheimer's regions, the entorhinal cortex had z = 1.71 (left) and 1.69 (right), corrected p = 0.081 and 0.084. The hippocampus had z = 0.71 (left) and 0.74 (right), corrected p = 0.313 and 0.307. None of them is significant after correction.

## Conclusion

I wanted to see if Alzheimer's risk genes are more active in some brain regions than others. I used 474 genes from GWAS (p < 1e-10) and compared their expression across 83 regions against 10,000 random gene lists of the same size.

All top 10 regions by z-score were cortical. After FDR correction only the left and right transverse temporal cortex were significant (z about 3.1, corrected p = 0.041). Right frontal pole, lateral occipital, isthmus cingulate, lingual and superior temporal had z between 2.4 and 2.8, but their corrected p was 0.055, just above the cutoff. I was a bit surprised that the entorhinal cortex (corrected p = 0.081 and 0.084) and the hippocampus (corrected p about 0.31) were not significant, because they are among the first regions damaged in Alzheimer's.

I can't say this proves where Alzheimer's starts. GWAS genes only show DNA variants linked to risk, the expression data comes from 6 healthy donors, and my test does not account for spatial autocorrelation. In this data, risk gene activity does not clearly explain where Alzheimer's strikes, but the genes are not spread evenly across the brain.

## Limitations
- Brain regions that sit near each other have similar expression (spatial autocorrelation). My random gene test treats regions as independent, so the p-values may be smaller than they should be. A spin test (for example with neuromaps) would handle this, but I did not run it.
- The expression data comes from only 6 donors, and the right hemisphere was copied from the left.
- Nearest-gene problem: the GWAS Catalog maps each variant to the gene it sits in or to the nearest gene(s). For variants between two genes I kept both genes. The true causal gene may be a different one, so some of my 474 genes may not be real risk genes.
- GWAS genes only show DNA variants linked to risk, and about 43% of the 837 genes at p < 1e-10 were not available in the expression table.

## Files
- `brain_gene_expression.ipynb`: full code and outputs
- `gwas-association-downloaded_2026-10-15-MONDO_0004975-withChildTraits.tsv`: GWAS Catalog associations for Alzheimer's disease (MONDO_0004975), the input gene data
- The Desikan-Killiany atlas is downloaded automatically in the notebook with `abagen.fetch_desikan_killiany()`, so it is not stored here.
- `results.csv`: z-score, p-value and corrected p-value for each of the 83 regions
- `brain_zscore.png`: brain map of the z-scores (red = risk genes more active than random genes, blue = less active)
- The notebook also creates `ahba.csv` (expression table) and `gene_list.csv` (1,093 genes at p < 5e-8). These are not stored here.

## How to run
The notebook was run in Google Colab. The GWAS file is read from `/content/`, so upload the `.tsv` file there before running the GWAS cells.

## Notes
abagen needed a small fix to work with newer pandas. I used an AI assistant to help debug code and understand concepts.
