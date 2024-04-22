<!--
 * @Author: misaki misakiwang74@gmail.com
 * @Date: 2024-04-22 19:07:03
 * @LastEditors: misaki misakiwang74@gmail.com
 * @LastEditTime: 2024-04-22 19:23:06
-->


# Overview

Next-hour traffic flow forecasting.

# Description

Below describes how the train/test data is generated from the original loop sensor data.

## Original loop sensor data

The original data is provided by Department of Roads and Travel - City of Paris through opendata.paris.fr:

- [Road counting - History - Traffic data from permanent sensors](https://parisdata.opendatasoft.com/explore/dataset/comptages-routiers-permanents-historique/information/) This provides the loop sensor data from 2014 to 2022.
- [Road counting - Traffic data from permanent sensors](https://opendata.paris.fr/explore/dataset/comptages-routiers-permanents/information/?disjunctive.libelle&disjunctive.etat_trafic&disjunctive.libelle_nd_amont&disjunctive.libelle_nd_aval) This provides the loop sensor data from 2023 to current day.

Here is an exmaple of historical loop sensor data:

<img src="https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202404221606330.png" alt="image.png" style="zoom: 33%;" />

Here is an exmaple of current loop sensor data:

<img src="https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202404221609479.png" alt="image.png" style="zoom: 50%;" />

According to the [explanation file](https://parisdata.opendatasoft.com/api/datasets/1.0/comptages-routiers-permanents-historique/attachments/notice_donnes_trafic_capteurs_permanents_version_20190607_pdf/), here are the interpretation of the data:

1. `iu_ac`: This could stand for a unique identifier for the traffic measurement area or segment, often referred to as "Identifiant Unique" in French, which translates to "Unique Identifier."
2. `libelle`: Name or label of the road or segment where the data was collected. "Libellé" means "label" or "designation" in French.
3. `t_1h`: Time (hourly base).
4. `q`: Traffic flow.
5. `k`: Occupancy ratio.
6. `etat_trafic`: Traffic status. This is calculatated from k.

   <img src="https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202404221610165.png" alt="image.png" style="zoom:50%;" />
7. `iu_nd_amont`: ID of the upstream node.
8. `libelle_nd_amont`: Name of the upstream node.
9. `iu_nd_aval`: ID of the downstream node.
10. `libelle_nd_aval`: Name of the downstream node.
11. `etat_barre`: The operational status of the road segment.

    <img src="https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202404221610285.png" alt="image.png" style="zoom:50%;" />
12. `date_debut and date_fin`: These fields represent the start and end dates, respectively, possibly indicating the period during which the data was collected or the duration for which the traffic status is relevant.
13. `geo_point_2d`: The geographic coordinates (latitude and longitude) of the road segment or a specific point within it.
14. `geo_shape`: The geometric data (in GeoJSON format) describe the shape and physical layout of the road segment. This is a polygon to represent the road's path.
15. `dessin`: Contains XML-like data describing the road segment shape.

## Metadata: Geographic Reference of Loop Sensor

[Road counting - Geographic reference](https://parisdata.opendatasoft.com/explore/dataset/referentiel-comptages-routiers/information/) provides the geo locations of loop sensors.

Here is an example of the data:

<img src="https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202404221612226.png" alt="image.png" style="zoom:50%;" />


## Data Preprocessing

1. We only keep `iu_ac` , `t_1h`, `stat_barre`,`q` columns. The additional information describing the sensors can be found in the metadata.
2. We only use the records with `q` value as NOT NULL
3. We only use the records where `iu_ac` also exists in the metadata 

## Generate train and test data

Here is how we generate training and testing data:

1. All the records in year 2022 are taken as train
2. For timestamps in year 2023, we take the first 24 hours as train, then 25th hour as test, then discard the next 4 hours. We repeat this process for all the hours in 2023.

## Baseline estimation

The expected arrival time from the last record time in the training data is taken as the baseline for comparison.

# Evaluation

## Evaluation

MAE between actual flow q and estimated flow q. (Note: we may further consider ignore the records where `stat_barre` is Barré, indicating the road segment might have problems)

## Submission File

For each ID in the test set, you must estimate the flow q. The file should contain a header and have the following format:

```
id,estimate_q
1,226.0
2,1643.0
3,1316.0
4,1831.0
5,1514.0
6,842.0
7,1686.0
8,1349.0
9,1832.0
```