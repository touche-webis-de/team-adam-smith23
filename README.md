# team-adam-smith23
[![license](https://img.shields.io/github/license/touche-webis-de/team-adam-smith23)](https://github.com/touche-webis-de/team-adam-smith23/blob/main/LICENSE)

Dockerized version of the winning submission to ValueEval'23

## Prerequisites

 - [Docker](https://docs.docker.com/engine/installation/) for building/using the classifier

## Predict

The classifier requires two mounted folders, `input` and `output`,
with the first one containing the arguments to be classified inside `arguments.tsv`.
```bash
mkdir input output
wget https://zenodo.org/record/7550385/files/arguments-test.tsv > -O input/arguments.tsv
```
The dataset assembled for ValueEval'23 is available
[online](https://zenodo.org/record/7550385).

The Docker image is hosted at `ghcr.io` and will be pulled automatically by `docker run`.

Predictions on all arguments for each of the
[20 value categories](https://touche.webis.de/semeval23/touche23-web/)
with the following command, creating the file `output/predictions.tsv`:
```bash
docker run --rm -it --user 1000:1000 \
  --volume "$PWD/input:$input" \
  --volume "$PWD/output:$output" \
  ghcr.io/webis-de/valueeval23-adam-smith-12:1.0.0-cpu \
  python3 /app/predict.py --inputDataset /input --outputDir /output
```

## Build Docker Images

The required model files can be downloaded under the following link:
[https://zenodo.org/record/7656534](https://zenodo.org/record/7656534)

Place the downloaded zip-Archive in the
[checkpoints](checkpoints)
directory and un-zip the archive (should result in the folder
[checkpoints/human_value_trained_models](checkpoints/human_value_trained_models)
with 24 files inside).
```bash
TAG=1.0.0-cpu # or 'TAG=1.0.0-cuda11.3' if a GPU is available
CUDA=nocuda # or 'CUDA=cuda11.3' if a GPU is available

docker build -t ghcr.io/webis-de/valueeval23-adam-smith-12:$TAG --build-arg CUDA=$CUDA -f Dockerfile .
```

## Remark towards Code Adaptation

The base source code is taken from the `predict.ipynb` file as well as the `data_modules`, `models`, and `toolbox` folders from
[https://github.com/danielschroter/human_value_detector](https://github.com/danielschroter/human_value_detector).

The parts of the code taken directly from the notebook are marked with `START` and `END` comments.
All changes made to lines from the repository are noted directly above each affected expression.
