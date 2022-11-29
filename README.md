# NECPlus

We present NEC+, the multivariate time series prediction model we detail in our paper, "An Extreme-Adaptive Time Series Prediction Model Based on Probability-Enhanced LSTM Neural Networks," which will be presented at AAAI 2023. Additionally, this GitHub repo contains the data that were used in this paper. If you make use of our code or data, please cite our paper.

```bibtex
@inproceedings{Li2022AAAI23,
  author    = {Yanhong Li and Jack Xi and David C. Anastasiu},
  title     = {An Extreme-Adaptive Time Series Prediction Model Based on Probability-Enhanced LSTM Neural Networks},
  booktitle = {Thirty-Seventh {AAAI} Conference on Artificial Intelligence, {AAAI} 2023},
  pages     = {},
  publisher = {{AAAI} Press},
  year      = {2023},
}
```


## Preliminaries

Experiments were executed in an Anaconda 3 environment with Python 3.9.8. The following will create an Anaconda environment and install the requisite packages for the project.

```bash
conda create --name necplus python=3.8.3
conda activate necplus
conda install pytorch==1.11.0 torchvision==0.12.0 cudatoolkit=10.2 -c pytorch
python -m pip install -r requirements.txt
```

## Files organization

In the ./data directory, there are 9 reservoir sensor files (file names start with reservoir_stor*) and 3 rain sensor files (file names start with raingauge_byhour* ) datasets.

## Training mode

The 3 Jupyter notebooks:

1. "NEC_Classifier.ipynb"
2. "NEC_Normal.ipynb"
3. "NEC_Extreme.ipynb"

are used to train the C model, N model, and E model, respectively. 

1. Use "Val_test_set_gen.ipynb" to generate validation set and test set file samples. The script uses random sampling to generate a validation set and save it to a file in ./data named `<sensor_id>_validation_timestamps_24avg.tsv` and a test set file saved as `./data/<sensor_id>_test_timestamps_24avg.tsv`. Before continuing, make sure the extreme events in the validation set follow a similar distribution to the rest of the data in your data set. 

2. Set parameters. "sensor_id" and \epsilon should be set consistently in the 3 Jupyter notebooks. The other parameters (described in the paper) can be assigned separately in the code.

3. Run the 3 Jupyter notebooks in any order or in parallel. After the training, the best models are saved in "./model" directory, and the forecasting results will be stored in the "./test" directory. 

4. Use "TestResult_Merging.ipynb" to combine the 3 parts in "./test" to generate the final prediction file.

## Inference mode

This can be easily achieved by hiding the train_loop() code line in the 3 Jupyter Notebooks, once the model has been trained. The other steps are the same as during training since we still need to follow the same steps to preprocess the original data and get the GMM Indicators. Fortunately, this step only needs to be run one time. After that, "generate_test()" can be executed repeately with different test timestamps.


