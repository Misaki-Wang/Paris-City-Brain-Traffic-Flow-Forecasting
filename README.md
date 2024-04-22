# Paris-City-Brain-Traffic-Flow-Forecasting

## Data download

Kaggle setting

```sh
pip install kaggle
mkdir ~/.kaggle
cp kaggle.json ~/.kaggle/
chmod 600 ~/.kaggle/kaggle.json
```

Download the data

```sh
mkdir data
cd data
kaggle competitions download -c paris-city-brain-traffic-flow-forecasting
unzip paris-city-brain-traffic-flow-forecasting.zip
rm -f paris-city-brain-traffic-flow-forecasting.zip  
```

