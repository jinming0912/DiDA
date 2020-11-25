基于深度神经网络的特征解耦与域适应的迭代式增强方法

Iterative Boosting of Disentangled Synthesis and Domain Adaptation
## Introduction
In this paper, we present a method for boosting Domain Adaptation(DA) performance by leveraging Disentanglement(Di) analysis. The key idea is that by learning to separately extract both the common and the domain-specific features, one can synthesize more target domain data with supervision, thereby boosting the domain adaptation performance. 

## Usage
The method is implemented and tested with Pytorch in python2 and use tensorboard for visualization.
Take DSN as backbone and dataset as MNIST-MNISTM as an example：

```
python train.py --dataroot ./traindata --name DSN_m --model DSN_mv2 --no_dropout --lr 0.01 --lr_de 1 - gpu_ids 0 --lr_policy nopolicy --which_method DSN
```
run the DA backbone
```
python train2.py --dataroot ./traindata --name Di_DSN_m --model Di_DSN_m --no_dropout --lr 0.00001 --lr_de 1 - gpu_ids 0 --lr_policy nopolicy --which_method DSN --batchSize 64 --which_epoches_DA [which_epoches_DA] --which_usename_DA DSN_m
```
run the Di stage
```
python train3.py --dataroot ./traindata --name DSN_miter1step1 --model DSN_m_iter --no_dropout --lr 0.01 --lr_de 10 - gpu_ids 0 --lr_policy nopolicy --which_method DSN --batchSize 100 --which_epoches_DA [which_epoches_DA] --which_usename_DA DSN_m --which_epoches_Di [which_epoches_Di] --which_usename_Di Di_DSN_m 
```
iteration of DA
```
python train2.py --dataroot ./traindata --name Di_DSN_iter1m --model Di_iter_DSN_m --no_dropout --lr 0.00001 --lr_de 1 - gpu_ids 0 --lr_policy nopolicy --which_method DSN --batchSize 64 --which_epoches_DA [which_epoches_DA] --which_usename_DA DSN_miter1step1 --which_epoches_Di [which_epoches_Di] --which_usename_Di Di_DSN_m 
```
iteration of Di

use tensorboard for visualization:
```
cd <your path>/DiDA
tensorboard --logdir=runs --port=6006
```