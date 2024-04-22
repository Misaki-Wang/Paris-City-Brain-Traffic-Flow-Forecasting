## Dataset Description

Training data contains (`iu_ac`, `t_1h`, `etat_barre`, `q`).

Your job is to predict the `estimate_q` for (`iu_ac`, `t_1h`, `etat_barre`) in the test data.

### Files

- `loop_sensor_train.csv` - the training set
- loop_sensor_test_x.csv - the test set
- loop_sensor_test_baseline.csv - a sample submission file in the correct format

### Columns

- `iu_ac` - ID of a loop sensor
- `t_1h` - time
- `etat_barre` - condition of the road segment
- `q` - traffic flow

### Metadata

`geo_reference.csv` - geographic information of loop sensors

- `iu_ac` - ID of the loop sensor
- `libelle` - name of the loop sensor
- `iu_nd_amont` - upstream node
- `iu_nd_aval` - downstream node
- `geo_point_2d` - latitude and longitude of the loop sensor
- `geo_shape` - polyline of the road segment that the loop sensor is located
