# team-adam-smith23
[![license](https://img.shields.io/github/license/touche-webis-de/team-adam-smith23)](https://github.com/touche-webis-de/team-adam-smith23/blob/main/LICENSE)

Dockerized version of the winning submission to ValueEval'23. Needs a [Docker installation](https://docs.docker.com/engine/installation/).

## Usage

The classifier requires two mounted folders, `input` (containing an `arguments.tsv` from the [dataset](https://doi.org/10.5281/zenodo.6814563)) and `output`.
```bash
mkdir input output
wget https://zenodo.org/record/7550385/files/arguments-test.tsv > -O input/arguments.tsv
```

Create a [submission file](https://touche.webis.de/semeval23/touche23-web/index.html#submission) in `output`:
```bash
docker run --rm -it \
  --volume "$PWD/input:$input" \
  --volume "$PWD/output:$output" \
  ghcr.io/webis-de/valueeval23-adam-smith-12:1.0.0-cpu \
  python3 /app/predict.py --inputDataset /input --outputDir /output
```


## Provenance

This repository is based on the `predict.ipynb` as well as the `data_modules`, `models`, and `toolbox` directories of [https://github.com/danielschroter/human_value_detector](https://github.com/danielschroter/human_value_detector). The parts of the code copied from the `predict.ipynb` are marked with `START` and `END`. Necessary changes are marked in the code.


## Build Docker Image

Unzip the [model files](https://zenodo.org/record/7656534) into [checkpoints](checkpoints) (creating `checkpoints/human_value_trained_models` with 24 files inside).
```bash
docker build -t ghcr.io/webis-de/valueeval23-adam-smith-12:1.0.0-cpu -f Dockerfile .
```
