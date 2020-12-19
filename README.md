 # 探究transformer解码器中编码器不同层的输出对翻译效果影响
本项目主要探究两种方式下的encoder端输出
- 方式1：从底层到与解码器层对应的编码器层的每层加权和
- 方式2：与解码器层对应的编码器层到顶层的每层加权和
# Dependencies
- fairseq version = 0.6.2
- PyTorch version >= 1.5.0
- Python version >= 3.6
- For training new models, you'll also need an NVIDIA GPU
# Dataset
使用iwslt14-de-en数据集

可通过以下命令获取：  
```py 
# 项目根路径下
cd examples/translation
bash prepare-iwslt14.sh
```
# preprocess
数据集预处理可通过以下命令实现：
```py 
# 项目根路径下
bash preprocess.sh
```
# training
```sh
bash train.sh
```
注：
- 方式1下的信息融合：arch = ltransformer_iwslt_de_en
- 方式2下的信息融合：arch = utransformer_iwslt_de_en

# 实验结果
## 方式1下的encoder-decoder间信息融合
#| Model |  BLEU |
--|--|--
1|Baseline|35.81
2|平均连接层的输出|33.02
3|给层数高的更大权重|33.88
4|给层数低的更大权重|33.82

注：baseline为transformer_t2t_iwslt_de_en
## 方式2下的encoder-decoder间信息融合
#| Model |  BLEU |
--|--|--
1|Baseline|35.81
2|平均连接层的输出|34.20
3|给层数高的更大权重|33.94
4|给层数低的更大权重|32.13

# 总结
在本次尝试中，我们通过改变编码器与解码器的连接来尝试改进现有模型。通过编码器多个层的加权表示代替之前的编码器最高层表示，协调模型的编码器和解码器的连接。然而，实验结果表明，在IWSLT14德英数据集上改动的模型无一例外BLEU得分均低于基线模型，我们对这一现象做出了简单分析。详情见[机器翻译报告](https://github.com/dayangxiaosong/MTProject/blob/main/%E5%91%A8%E5%AE%A3%E5%86%9B-2001888-%E6%9C%BA%E5%99%A8%E7%BF%BB%E8%AF%91%E6%8A%A5%E5%91%8A.pdf)。
