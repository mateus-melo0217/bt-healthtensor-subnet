<div align="center">

# HealthTensor Subnet
</div>

## Introduction

A revolutionary healthcare subnet on the Bittensor network

### How it works
At the core of our subnet are the miners—skilled AI developers and teams dedicated to training and refining machine learning models. These models are dynamic and continuously improved. Miners publish their latest models on Hugging Face, a platform renowned for its extensive repository of AI models, making these advancements accessible to our community and beyond.

Validators, another essential part of our ecosystem, play a crucial role in assessing these models. They download the models from Hugging Face and evaluate them using their own diverse datasets. This evaluation is not just a binary measure of success or failure; it is a nuanced process involving the ranking of models based on their loss metrics. The lower the loss, the higher the model ranks, reflecting its accuracy and efficiency in solving real-world problems.

### Competition and Reward
Our subnet is built on the foundation of healthy competition, driving our miners to not only participate but to strive for excellence. The rewards they receive are directly linked to the performance of their models, creating a powerful incentive for continuous improvement. The superior a model's accuracy and loss metrics, the greater the rewards for its creator. This dynamic and competitive environment ensures that only the most exceptional and efficient models rise to the top, driving innovation and progress within our subnet.

In addition to miners, validators play a crucial role in our ecosystem and are duly rewarded for their contributions. Their responsibility lies in providing fair and unbiased evaluations, thereby upholding the transparency and meritocracy of our network. By offering impartial assessments, validators are instrumental in determining which models receive the highest rewards, ultimately guiding the trajectory of AI development within our subnet.

This system of incentivization not only fosters healthy competition but also ensures that the most deserving models and contributors are appropriately recognized and rewarded. It creates a robust framework where excellence is celebrated, driving the continual advancement of AI capabilities within our subnet.

By aligning rewards with performance and fairness, we are able to cultivate an environment where only the best and most deserving models and contributors are able to thrive. This not only benefits individual miners and validators, but also bolsters the overall quality and integrity of our network. It serves as a powerful mechanism for promoting continuous improvement and innovation, ultimately driving the evolution of AI development within our subnet.

In essence, our subnet's reward system is designed to fuel excellence, transparency, and meritocracy. It empowers both miners and validators to strive for the highest standards, ensuring that the most exceptional models rise to prominence while upholding the integrity of our network. This approach not only benefits individual participants, but also contributes to the collective advancement of AI technology within our subnet.

---
## Installation

### Install

#### Bittensor

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/opentensor/bittensor/master/scripts/install.sh)"
```

#### Clone the repository from Github
```bash
git clone https://github.com/mateus-melo0217/bt-healthtensor-subnet.git
```

#### Install package dependencies for the repository
```bash
cd bt-healthtensor-subnet
apt install python3-pip -y
python3 -m pip install -e .
```

#### Install `pm2`
```bash
apt update && apt upgrade -y
apt install nodejs npm -y
npm i -g pm2
```

#### Setting Up Your Hugging Face Account
To begin, please go to the Hugging Face website at https://huggingface.co/ and create an account. Once your account is successfully set up, the next important step is to obtain your access_token. This access_token is crucial for carrying out further actions on the platform. You can find detailed instructions on how to secure your access_token by visiting the following link: https://huggingface.co/docs/hub/security-tokens.

After obtaining your access_token, the next step is to locate the .env.example file. Once you have found the file, please proceed by renaming it to .env. This will create a new file with the name .env. Finally, you will need to copy your access_token and paste it into this newly renamed .env file.

By following these steps, you will have successfully set up your account on the Hugging Face platform and secured your access_token, allowing you to make use of the platform's features and capabilities. If you encounter any difficulties or have any questions along the way, feel free to reach out for assistance.
```
To all validators, it is crucial to define the DATASET_LINK provided by demon (on Discord) in the .env file. This link is essential for the proper functioning of the system and must be accurately defined to ensure seamless operations. Please ensure that this step is completed to avoid any potential issues in the future.
```

---
## Running

### Running subtensor locally

#### Install Docker
```bash
apt install docker.io -y
apt install docker-compose -y
```

#### Run Subtensor locally
```bash
git clone https://github.com/opentensor/subtensor.git
cd subtensor
docker-compose up --detach
```

### Running miner
In this innovative healthcare subnet, miners play a crucial role in contributing to disease diagnosis by predicting from medical images. Through continuous training, miners strive to improve their models, with more accurate models earning substantial rewards. Miners have the flexibility to adapt and enhance the structure of their models, datasets, and other factors influencing model accuracy. This collaborative effort aims to advance disease prediction and underscores the vital role miners play in shaping the future of medical diagnostics. By leveraging their expertise and adaptability, miners are driving progress in medical imaging analysis and contributing to the evolution of diagnostic capabilities in healthcare. Their dedication to refining and optimizing predictive models demonstrates their commitment to enhancing patient care and outcomes.

#### Download the dataset for model training
```bash
python3 healthcare/dataset/downloader.py
```

#### Run the miner with `pm2`
```bash
# To run the miner
pm2 start neurons/miner.py --name miner --interpreter python3 -- 
    --netuid # the subnet netuid, default = 
    --subtensor.network # the bittensor chain endpoint, default = finney, local, test (highly recommend running subtensor locally)
    --wallet.name # your miner wallet, default = default
    --wallet.hotkey # your validator hotkey, default = default
    --logging.debug # run in debug mode, alternatively --logging.trace for trace mode
    --num_epochs # the number of training epochs (-1 is infinite), default = -1
    --batch_size # the number of data points processed in a single iteration, default = 32
    --save_model_period # the period of batches during which the model is saved, default = 30
    --model_type # the architecture and structure of the neural network used for training, default = CNN, VGG, RES, EFFICIENT, MOBILE
    --training_mode # the training mode, whether in fast, normal, or slow mode, dictates the pace and intensity of the model training process, default = normal
    --device gpu:0,2 # the device will be used for model training, default = gpu
    --restart # if set, miners will start the training from scratch, default = False
```

- Example 1 (with default values):
```bash
pm2 start neurons/miner.py --name miner --interpreter python3 -- --wallet.name default --wallet.hotkey default --logging.debug
```

- Example 2 (with custom values):
```bash
pm2 start neurons/miner.py --name miner --interpreter python3 --
    --subtensor.network local
    --wallet.name default
    --wallet.hotkey default
    --logging.debug
    --num_epochs 10
    --batch_size 256
    --restart True
    --model_type vgg
    --training_mode fast
    --device cpu:4
```

### Running validator
Validators are essential in the process of evaluating miner's models within the collaborative network. They play a crucial role by regularly sending a variety of images to the miners for assessment. With meticulous attention to detail, they carefully score the miners based on their responses. This contributes to the continuous improvement of models and ensures that the highest standards of performance and accuracy are maintained within the network. Their thorough evaluation process helps to uphold the quality and reliability of the models used in the collaborative network.

#### Run the validator
```bash
# To run the validator
pm2 start neurons/validator.py --name validator --interpreter python3 -- 
    --netuid # the subnet netuid, default = 
    --subtensor.network # the bittensor chain endpoint, default = finney, local, test (highly recommend running subtensor locally)
    --wallet.name # your miner wallet, default = default
    --wallet.hotkey # your validator hotkey, default = default
    --logging.debug # run in debug mode, alternatively --logging.trace for trace mode
    --vpermit_tao_limit # the maximum number of TAO allowed to query a validator with a vpermit, default = 4096
```

- Example 1 (with default values):
```bash
pm2 start neurons/validator.py --name validator --interpreter python3 -- --wallet.name default --wallet.hotkey default --logging.debug
```

- Example 2 (with custom values):
```bash
pm2 start neurons/validator.py --name validator --interpreter python3 --
    --subtensor.network local
    --wallet.name default
    --wallet.hotkey default
    --logging.debug
    --vpermit_tao_limit 1024
```

---
## License
This repository is licensed under the MIT License.
```text
# The MIT License (MIT)
# Copyright © 2023 Yuma Rao

# Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated
# documentation files (the “Software”), to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software,
# and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

# The above copyright notice and this permission notice shall be included in all copies or substantial portions of
# the Software.

# THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO
# THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
# OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
# DEALINGS IN THE SOFTWARE.
```
