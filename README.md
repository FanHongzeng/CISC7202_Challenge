## Challenge Report Ground-Satellite Geo-localization

Cross-view geolocalization identifies the geographic location of street view images by matching them with a georeferenced satel-lite database. Significant challenges arise due to the drastic appearance and geometry differences between views. The University-1652 dataset offers a novel approach to cross-view geolocalization. This dataset includes images from three different platforms: synthetic drones, satellites, and ground cameras, covering 1,652 university buildings worldwide. 

In this report, I will use the University-1652 dataset to evaluate the model’s performance in Ground-to-Satellite recognition.

### Requirements

- Python: 3.9.4
- Ubuntu: 24.04 LTS

- For the other packages, please refer to the `requirements.txt`. You can just execute following command to create the conda environment.

  ```
  conda create --name env_name --file requirements.txt
  ```

- download University1652 and competition test data

  ```
  ##University1652
  #download from: https://github.com/layumi/University1652-Baseline/blob/master/Request.md
  
  ##competition test data
  ##download from: https://hdueducn-my.sharepoint.com/personal/wongtyu_hdu_edu_cn/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fwongtyu%5Fhdu%5Fedu%5Fcn%2FDocuments%2FDatasets%2FACM%5FMM%5FWorkshop&ga=1
  ```

### Usage

#### model train

```
python train.py --gpu_ids 0 --name three_view_long --views 3 --extra --droprate 0.75 --share --stride 2 --h 256 --w 256 --batchsize 32 --data_dir your_data_path
```

#### model evaluate and compare features

```
python test.py --gpu_ids 0 --name three_view_long --test_dir your_data_path --batchsize 32 --which_epoch 59
python demo.py --query_index 1 --test_dir your_data_path
```

#### model test on competition test data

```
python test_160k.py --gpu_ids 0 --name three_view_long --test_dir your_data_path  --batchsize 32 --which_epoch 59
```

### Result

The following table show model performance comparison on University-1652 Dataset

| Model                                         | Recall@1 | Recall@5 | Recall@10 |
| --------------------------------------------- | -------- | -------- | --------- |
| three_view_net+droprate0.75+stride1+h256+w256 | 1.20     | 4.27     | 7.64      |
| three_view_net+droprate0.75+h256+w256         | 1.32     | 4.42     | 7.41      |
| three_view_net+droprate0.75+pool='avg+max'    | 1.05     | 4.42     | 7.21      |
| ft_net                                        | 0.93     | 2.95     | 4.69      |
| ft_net + Stride1                              | 0.54     | 2.33     | 4.46      |
| ft_net + lr0.001 + Stride1                    | 0.39     | 2.13     | 4.07      |

#### Citation

The following paper uses and reports the result of the baseline model.

```
@article{zheng2020university,
  title={University-1652: A Multi-view Multi-source Benchmark for Drone-based Geo-localization},
  author={Zheng, Zhedong and Wei, Yunchao and Yang, Yi},
  journal={ACM Multimedia},
  year={2020}
}

@inproceedings{zheng2023uavm,
  title={UAVM'23: 2023 Workshop on UAVs in Multimedia: Capturing the World from a New Perspective},
  author={Zheng, Zhedong and Shi, Yujiao and Wang, Tingyu and Liu, Jun and Fang, Jianwu and Wei, Yunchao and Chua, Tat-seng},
  booktitle={Proceedings of the 31st ACM International Conference on Multimedia},
  pages={9715--9717},
  year={2023}
}

@article{zeng2022geo,
 title={Geo-Localization via Ground-to-Satellite Cross-View Image Retrieval},
 author={Zelong Zeng and Zheng Wang and Fan Yang and Shin'ichi Satoh},
 journal={IEEE Transactions on Multimedia},
 year={2022},
 volume={24},
 pages={1234-1245},
 doi={10.1109/TMM.2022.3144066},
 url={https://arxiv.org/abs/2205.10878}
}
```

