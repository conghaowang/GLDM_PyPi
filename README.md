# GLDM_PyPi

This [repository](https://github.com/nomoresomethingwentwrong/GLDM_PyPi) contains the user documentation of [gldm package](https://pypi.org/project/gldm/). Implementation of GLDM is stored [here](https://github.com/nomoresomethingwentwrong/GLDM). 

## Environment Setup

`gldm` depends on `pytorch`, `pytorch-geometric` and `rdkit` libraries. To install the dependencies, please create a conda environment with the config file `GLDM.yml`:

```bash
conda env create --file=environment.yml
conda activate gldm
```

`gldm` can then be installed via pip:

```bash
pip install gldm
```

## Adapt *pytorch-geometric* Package

We need to customize the pre-batching and mini-batching behaviors of `pytorch-geometric`. Please check the details in [adapted_pkgs/pyg/README.md](adapted_pkgs/pyg/README.md).

## Inference with Pretrained Models

### Download Pretrained Models

Our models were developed in two stages. First, the autoencoder models were trained to learn the latent space of the molecular graphs. Then, the diffusion models were trained to manufacture the latent space. The pretrained autoencoder models can be downloaded from [here](https://zenodo.org/records/10782445), and the diffusion models can be downloaded from [here](https://zenodo.org/records/10782451).


### Usage

A simple example to generate 10 molecules with dummy gene expression profiles and save the generated molecules as `samples.png`:

```python
from gldm import sampleMol
import torch

if __name__ == "__main__":
    model_path = 'GLDM_models/GLDM_WAE_cond.pt'
    config_file = 'config/GLDM_WAE_cond.yml'
    # gene_expression_file = 'pseudo_gene_expr_dose.pt'
    dummy_gene_expr = torch.rand((10, 979))  # 978 gene expression profile and 1 dosage value
    sampleMol(model_path, config_file, gene_expression=dummy_gene_expr, num_samples=10, output_file='samples.pkl', save_img='samples.png')
```
A similar example can be found in [Example_usage.ipynb](Example_usage.ipynb).

The `sampleMol` function has the following parameters:

| Parameter | Description |
| --- | --- |
| `model_path` | The path to the pretrained model file. |
| `config_file` | The path to the configuration file. |
| `gene_expression` | A PyTorch tensor or a Numpy array or a list representing the gene expression + dosage. |
| `gene_expression_file` | The path to the file containing the gene expression tensors. If `gene_expression` is provided, this parameter will be ignored. |
| `num_samples` | The number of molecules to be generated. |
| `output_file` | The pickle file where the generated molecules will be saved. |
| `save_img` | The path where the generated image will be saved. |

**Note**: 
- The default configuration files are provided in the [config](config) folder. Remember to change `config['model']['first_stage_config']['ckpt_path']` to the path that you stored the pretrained autoencoder models.
- We provide conditional and unconditional models using VAE, AAE and WAE losses. Please ensure the autoencoder model and the diffusion model are consistent in terms of the loss function and the conditional setting.
- Ensure the gene expression data are provided if the conditional models are used. 

## Training GLDM from scratch

***Coming soon ...***

---

## Citation

```
@article{10.1093/bib/bbae142,
    author = {Wang, Conghao and Ong, Hiok Hian and Chiba, Shunsuke and Rajapakse, Jagath C},
    title = "{GLDM: hit molecule generation with constrained graph latent diffusion model}",
    journal = {Briefings in Bioinformatics},
    volume = {25},
    number = {3},
    pages = {bbae142},
    year = {2024},
    month = {04},
    issn = {1477-4054},
    doi = {10.1093/bib/bbae142},
    url = {https://doi.org/10.1093/bib/bbae142},
    eprint = {https://academic.oup.com/bib/article-pdf/25/3/bbae142/57160111/bbae142.pdf},
}
```
