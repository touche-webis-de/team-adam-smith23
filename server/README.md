# team-adam-smith23 server

Dockerized version of a server for the winning submission to ValueEval'23 by Schroter et al. [[original code](https://github.com/danielschroter/human_value_detector)]

Needs a [Docker installation](https://docs.docker.com/engine/installation/).

## Public demo

[values.args.me/](https://values.args.me/)

## Usage

The server requires two mounted folders, `/app/checkpoints/human_value_trained_models` (containing the [model files](https://zenodo.org/record/7656534) of the models that should be used) and (optional) `/app/logs` for server logs.

Start the server:
```bash
PORT=8080
docker run --rm -it \
  --volume "$PWD/checkpoints/human_value_trained_models:/app/checkpoints/human_value_trained_models" \
  --volume "$PWD/logs:/app/logs" \
  -p $PORT:8001 \
  ghcr.io/webis-de/valueeval23-adam-smith-server:1.0.0-cpu --internal_port=8001 --threshold=0.26
```

The go to [localhost:PORT/?argument=We+need+to+reduce+our+CO2+emissions+to+save+the+environment.](http://localhost:8080/?argument=We+need+to+reduce+our+CO2+emissions+to+save+the+environment.). You can also send the arguments as `text/plain` POST message.

The result is a JSON object with the classifications:
```json
{
  "Self-direction: thought": "0",
  "Self-direction: action": "0",
  "Stimulation": "0",
  "Hedonism": "0",
  "Achievement": "0",
  "Power: dominance": "0",
  "Power: resources": "0",
  "Face": "0",
  "Security: personal": "0",
  "Security: societal": "0",
  "Tradition": "0",
  "Conformity: rules": "0",
  "Conformity: interpersonal": "0",
  "Humility": "0",
  "Benevolence: caring": "0",
  "Benevolence: dependability": "0",
  "Universalism: concern": "0",
  "Universalism: nature": "1",
  "Universalism: tolerance": "0",
  "Universalism: objectivity": "0"
}
```

Suggested ensembles (names as used in Schroter et al, 2023 [publication under review]. The file pairs of the corresponding models, and only these, need to be placed in `human_value_trained_models`):
- `EN-Thres-LoD` (all 12 models)
  - using `HCV-364`, `HCV-366`, `HCV-368`, `HCV-371`, `HCV-372`, `HCV-373`, `HCV-402`, `HCV-403`, `HCV-405`, `HCV-406`, `HCV-408`, `HCV-409`
  - `--threshold=0.26`
- `EN-Max-F1` (6 models)
  - using `HCV-402`, `HCV-403`, `HCV-405`, `HCV-406`, `HCV-408`, `HCV-409`
  - `--threshold=0.26`
- `EN-Deberta-F1` (3 models)
  - using `HCV-406`, `HCV-408`, `HCV-409`
  - `--threshold=0.27`
- `Single-Deberta-F1` (1 model)
  - using `HCV-409`
  - `--threshold=0.25`


## Build Docker Image

```bash
docker build -t ghcr.io/webis-de/valueeval23-adam-smith-server:1.0.0-cpu -f Dockerfile .
```

