

# Thesis: Modifying Multimodal Models for Handling of Sequence Data

The contents in this repository are part of my Master's Thesis on "Modifying Multimodal Models for Handling of Sequence Data". This project has been developed at Theis Lab at the Institute of Computational Biology of Helmholtz Munich.

<!-- PROJECT SHIELDS -->

![Language](https://img.shields.io/badge/language-python-brightgreen)

 
## Content

- [ScBasset Paper Reproduce Results](#paper)

- [Modified GLUE Results](#modified_GLUE)

- [scbasset_GLUE_testing](#scbasset_GLUE_testing)

- [Used Framework](#used-framework)


### Source Dataset 
the scGLUE dataset can be downloaded via :
- [http://download.gao-lab.org/GLUE/tutorial/Chen-2019-RNA.h5ad](http://download.gao-lab.org/GLUE/tutorial/Chen-2019-RNA.h5ad)

- [http://download.gao-lab.org/GLUE/tutorial/Chen-2019-ATAC.h5ad](http://download.gao-lab.org/GLUE/tutorial/Chen-2019-ATAC.h5ad) 

<b name="paper"></b>
### ScBasset Paper Reproduce Results
Go to scBasset gitHub to install required packages or use:
```sh
conda env create -f env_scb.yml 
```
to set up environment for scBasset


Visit notebooks in order: 
1. [Construct train and validation datasets for scBasset](https://github.com/ml5376/thesis/blob/new/paper_reproduce/make_anndata.ipynb) 
2. [Training process/logs](https://github.com/ml5376/thesis/blob/new/paper_reproduce/continue_training.ipynb) 
3. [UMAP visualization with anotated cell type compare to a fully trained scBasset model provided by scBasset gitHub](https://github.com/ml5376/thesis/blob/new/paper_reproduce/comparison_to_download.ipynb)

### modified_GLUE
This folder contain M1-4 and M4(lsi) implementation:
1. Data preprocessing based on scGLUE data. [Construct subsample for modified GLUE testing](https://github.com/ml5376/thesis/blob/new/modified_GLUE/prepare_GLUE_data.ipynb). The tested sampled dataset is too large to upload, use preprocessing notebook(link above) for sampling smaller dataset from the source data.

2. UMAP visualization of learned embedding and scIB metrics performance score [reseults](https://github.com/ml5376/thesis/blob/new/modified_GLUE/Modified_model_scib.ipynb).

Weight of modified GLUE is saved to [weight folder](https://github.com/ml5376/thesis/tree/new/modified_GLUE/weight). See sepcification of the name of modified models corresponding to model M1-4: 

```
M0 - scglue
M1 - scglue3
M2 - scglue3_copy
M3 - scglue3_subsample
M4 - scglue3_scb
M4(lsi) - scglue_scb_lsi
```

and GLUE + scIB environment can be set up with
``` 
conda env create -f env_scb.yml 
```


### scbasset_GLUE_testing

Because this experiment is repeated for three times in the thesis. How to obtain the results for 400 epochs will be listed below as an example. For other experiments just follow the notebooks to get results. 

1. Download the subsampled datasets

```sh
!wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=14nb6pcG__i34dJxPSAV737QQivQ-TgAE' -O gdrive_atac0.h5ad


!wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=19-8dzYezWrp34XnrYt_tgvi07ias6OxP' -O gdrive_atac1.h5ad


!wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1g2YDTrhfLXoEpQNMe5rZswMlY4y5Z8IV' -O gdrive_atac2.h5ad

```
note that three atac subsamples are used in both 200 epochs and 400 epochs experiment and one rna subsample can be retrieved with code

```
#generate rna 
# rna = ad.read_h5ad('/home/ubuntu0/scBasset/Chen-2019-RNA.h5ad')
# rna.layers["counts"] = rna.X.copy()

# #select high expressive genes as in the tutorial the number 2000
# sc.pp.highly_variable_genes(rna, n_top_genes=60000, flavor="seurat_v3")

# sc.pp.normalize_total(rna)
# sc.pp.log1p(rna)
# sc.pp.scale(rna)
# sc.tl.pca(rna, n_comps=100, svd_solver="auto")

# sc.pp.neighbors(rna, metric="cosine")
# sc.tl.umap(rna)
# sc.pl.umap(rna,color='cell_type')

# scglue.data.get_gene_annotation(
#     rna, gtf="/home/ubuntu0/GLUE/evaluation/workflow/scripts/gencode.vM25.chr_patch_hapl_scaff.annotation.gtf.gz",
#     gtf_by="gene_name"
# )

```
follow by the second block of [200 epochs experimets](https://github.com/ml5376/thesis/blob/new/scbasset_GLUE_testing/train_glue_on_scb200.ipynb) and [400 epochs experiment](https://github.com/ml5376/thesis/blob/new/scbasset_GLUE_testing/train_glue_on_scb400.ipynb) to continue testing. The download link for gencode.vM25.chr_patch_hapl_scaff.annotation.gtf.gz is provided in [scglue tutorial page](https://scglue.readthedocs.io/en/latest/preprocessing.html )


2. The saved scBasset embedding and saved GLUE model for 400 epochs experiment is located in folder [400ep](https://github.com/ml5376/thesis/tree/new/scbasset_GLUE_testing/400ep) and [400epglue](https://github.com/ml5376/thesis/tree/new/scbasset_GLUE_testing/400epglue) respectively. 

3. The saved scBasset embedding and saved GLUE model for 200 epochs experiment is located in folder [2024-01-31-22_55_35scb_embedding](https://github.com/ml5376/thesis/tree/new/2024-01-31-22_55_35scb_embedding) and [2024-01-31-22_55_35glue](https://github.com/ml5376/thesis/tree/new/2024-01-31-22_55_35glue) respectively. 







<a name="used-framework"></a>
### Used Framework
- [scGLUE](https://github.com/gao-lab/GLUE)
- [scBasset](https://github.com/calico/scBasset)

