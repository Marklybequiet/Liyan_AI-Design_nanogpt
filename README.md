# Liyan_AI-Design_nanogpt
## install

```
pip install torch numpy transformers datasets tiktoken wandb tqdm
```

Dependencies:

- [pytorch](https://pytorch.org) >2 and <3 
- [numpy](https://numpy.org/install/) <3
-  `transformers` for huggingface transformers <3 (to load GPT-2 checkpoints)
-  `datasets` for huggingface datasets <3 (if you want to download + preprocess OpenWebText)
-  `tiktoken` for OpenAI's fast BPE code <3
-  `wandb` for optional logging <3
-  `tqdm` for progress bars <3

## dataset prepare

```sh
python data/doupo/prepare.py
```

This creates a `train.bin` and `val.bin` in that data directory. I have already resolved it here.

## training for CPU

Because I don't have a GPU, I use a CPU for simple training

```sh
python train.py config/doupo.py --device=cpu --compile=False --eval_iters=20 --log_interval=1 --block_size=64 --batch_size=12 --n_layer=6 --n_head=6 --n_embd=384 --max_iters=5000 --lr_decay_iters=2000 --dropout=0.0
```

This will take about 20 minutes to train the model. If you want it to be faster：

```sh
python train.py config/doupo.py --device=cpu --compile=False --eval_iters=20 --log_interval=1 --block_size=64 --batch_size=12 --n_layer=4 --n_head=4 --n_embd=128 --max_iters=5000 --lr_decay_iters=2000 --dropout=0.0
```

This will take about 5 minutes to train the model.

## training for GPU

```sh
python train.py config/doupo.py
```

You can change the parameters inside. The files in config can provide parameter references

## generates samples

Based on the configuration, the model checkpoints are being written into the `--out_dir` directory `out-shakespeare-char`. So once the training finishes we can sample from the best model by pointing the sampling script at this directory:

```sh
python sample.py --out_dir=out-doupo
```
