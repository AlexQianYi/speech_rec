# [Demo of tensorflow environment configuraton (local)]

### Setup
Clone this repo to your local machine, and add the speech_rec directory as a system variable to your `~/.profile`. Instructions given for bash shell:

```bash
git clone https://github.com/AlexQianYi/speech_rec
cd speech_rec
echo "export speech_rec=${PWD}" >> ~/.profile
echo "export PYTHONPATH=${PWD}/src:${PYTHONPATH}" >> ~/.profile
source ~/.profile
```

Create a Conda environment ([Install Conda](https://conda.io/docs/install/quick.html) first)

```bash
conda create --name tf-rnn python=3
source activate tf-rnn
cd $speech_rec
pip install -r requirements.txt
```

### Install TensorFlow

If you have a NVIDIA GPU with already installed

```bash
pip install tensorflow-gpu==1.0.1
```

If you will be running TensorFlow on CPU only (e.g. a MacBook Pro), use the following command (if you get an error the first time you run this command read below):

```bash
pip install --upgrade\
 https://storage.googleapis.com/tensorflow/mac/cpu/tensorflow-1.0.1-py3-none-any.whl
```


### Run unittests
The test example of whether RNN can run on your computer.

```bash
python $speech_rec/src/tests/train_framework/tf_train_ctc_test.py
```


### Run RNN training
All configurations for the RNN training script can be found in `$speech_rec/configs/neural_network.ini`

```bash
python $speech_rec/src/train_framework/tf_train_ctc.py
```

_NOTE: If you have a GPU available, the code will run faster if you set `tf_device = /gpu:0` in `configs/neural_network.ini`_


### TensorBoard configuration
To visualize your results via tensorboard:

```bash
tensorboard --logdir=$speech_rec/models/nn/debug_models/summary/
```

- TensorBoard result can be found in your browser at [http://localhost:6006](http://localhost:6006).
- `tf.name_scope` is used to define parts of the network for visualization in TensorBoard. TensorBoard automatically finds any similarly structured network parts, such as identical fully connected layers and groups them in the graph visualization.
- Related to this are the `tf.summary.* methods` that log values of network parts, such as distributions of layer activations or error rate across epochs. These summaries are grouped within the `tf.name_scope`.
- See the official TensorFlow documentation for more details.


### Data set
The data is separated into folders:

    - Train: train-clean-100-wav (5 examples)
    - Test: test-clean-wav (2 examples)
    - Dev: dev-clean-wav (2 examples)