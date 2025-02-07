# AF-Cluster

Code and data corresponding to Wayment-Steele, Ovchinnikov, Colwell, Kern (2022) "Prediction of multiple conformational states by combining sequence clustering with AlphaFold2".

## Usage

### To cluster MSA and generate subsampled MSA files:

`python scripts/ClusterMSA.py EX -i initial_msa.a3m -o msas`

Outputs a directory named `msas` that contains

	- msas/EX_000.a3m
	- msas/EX_001.a3m
	...
	- msas/EX_REF.a3m
	- msas/EX_U10-000.a3m
	- msas/EX_U10-001.a3m
	...
	- msas/EX_U100-000.a3m
	- msas/EX_U100-001.a3m
	...
	- msas/EX_REF.a3m
	- msas/EX_clustering_assignments.tsv
	-msas/EX_cluster_metadata.tsv

`EX_000.a3m`, `EX_001.a3m` ... are the clusters identified by DBSCAN.

`EX_U10-000.a3m, ... EX_U10-009.a3m` are uniformly sampled control MSAs of size 10 (Default is to generate 10).

`EX_U100-000.a3m, ... EX_U100-009.a3m` are uniformly sampled control MSAs of size 100 (Default is to generate 10).

`EX_REF.a3m` is a copy of the original MSA.

`EX_clustering_assignments.tsv` contains a list of original sequences and the cluster index they were assigned to (-1 means they were not assigned).

`EX_cluster_metadata.tsv` contains metadata corresponding to clusters.

To also perform PCA and/or tSNE embedding at the same time and save it in `EX_clustering_assignments.tsv` for later analysis:

`python scripts/WriteDBscanClusters.py -i <my_alignment.a3m> -o <outdir> --run_PCA`

or 

`python scripts/WriteDBscanClusters.py -i <my_alignment.a3m> -o <outdir> --run_tSNE`


Example output for KaiB:

```
2OUG
2006 seqs removed for too many gaps, 4745 remaining
eps	n_clusters	n_not_clustered
3.00	1	1186
4.00	1	1186
5.00	3	1180
6.00	8	1161
7.00	12	1147
8.00	36	1045
9.00	39	950
10.00	62	825
Selected eps=10.00
4745 total seqs
315 clusters, 2280 of 4745 not clustered (0.48)
avg identity to query of unclustered: 0.30
avg identity to query of clustered: 0.37
wrote clustering data to msas/2OUG_clustering_assignments.tsv
wrote cluster metadata to msas/2OUG_cluster_metadata.tsv
writing 10 size-10 uniformly sampled clusters
writing 10 size-100 uniformly sampled clusters
```

### To run AF2:

`python scripts/RunAF2.py`

See https://github.com/jproney/AF2Rank for more information on compiling an AlphaFold2 installation.

### To run MSA Transformer:

`python scripts/runESM.py -i <my_subMSA.a3m> -o <outdir>`

### To calculate RMSD to provided reference structure(s):

`python scripts/CalculateModelFeatures.py path/to/pdbs/* -o <my_output_file>.json.zip --ref_struct REF_PDB_1.pdb REF_PDB_2.pdb`

### To reproduce figures in preprint:

See `.ipynb` files included in relevant folders in `data`.
