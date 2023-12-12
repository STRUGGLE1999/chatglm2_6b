
OBS上传到对象:[https://support.huaweicloud.com/utiltg-obs/obs_11_0004.html](https://support.huaweicloud.com/utiltg-obs/obs_11_0004.html)
最近参加一个华为云大模型AI微调比赛，比赛链接：[https://competition.huaweicloud.com/information/1000041979/introduction](https://competition.huaweicloud.com/information/1000041979/introduction)
## 在Linux终端通过obsutil工具上传下载文件
根据自己的linux版本：
```
uname -m
```

我的是 Linux 系统是基于 ARM 64 位架构的。
下载和安装obsutil:[https://support.huaweicloud.com/utiltg-obs/obs_11_0003.html](https://support.huaweicloud.com/utiltg-obs/obs_11_0003.html)
### 解压安装：
先下载obsutil_linux_arm64.tar.gz压缩包，解压，增加可执行权限：
```python
tar -xzvf obsutil_linux_arm64.tar.gz
chmod 755 obsutil
```
继续在目录中执行以下命令，如果能顺利返回obsutil版本号，说明安装成功。
```
./obsutil version

```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1700793121274-5dbf5409-604d-4614-b00c-a7e784a315d7.png#averageHue=%2312100e&clientId=ufebda536-4ec9-4&from=paste&height=45&id=ud38e1f14&originHeight=89&originWidth=1124&originalType=binary&ratio=2&rotation=0&showTitle=false&size=15866&status=done&style=none&taskId=u144ebb8c-3c12-4c02-b14b-3c5c9564272&title=&width=562)
## 初始化配置
使用永久AK、SK进行初始化配置：
这里的AK和SK需要去华为云平台**创建访问密钥，**创建访问密钥的操作步骤如下：

1. 在控制台单击页面右上角的用户名，并选择“我的凭证”。
2. 在“我的凭证”页面，单击左侧导航栏的“访问密钥”。
3. 在“访问密钥”页面，单击“新增访问密钥”。

会下载一个csv表格，打开就要AK和SK
使用下列命令配置：
```
./obsutil config -i=ak -k=sk -e=endpoint
```
endpoint表示连接OBS的服务地址，可以去这里查看当前开通的区域和终端节点地址：[https://developer.huaweicloud.com/endpoint?OBS](https://developer.huaweicloud.com/endpoint?OBS)
比如我要下载的桶在西南-贵阳一，则-e=[https://obs.cn-southwest-2.myhuaweicloud.com](https://obs.cn-southwest-2.myhuaweicloud.com)
```python
./obsutil config -i=UELP7EYDQSBGK5D5PGG7 -k=7ksycdTe9l4BSozqciWCwIE4sUG4VoUorUFaQhFK -e=https://obs.cn-southwest-2.myhuaweicloud.com
```
显示以下信息则说明配置成功：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1700793538769-1594d317-63c4-4035-a6cc-a710a552a3a1.png#averageHue=%23070606&clientId=ufebda536-4ec9-4&from=paste&height=75&id=u5a2e35de&originHeight=150&originWidth=1021&originalType=binary&ratio=2&rotation=0&showTitle=false&size=13509&status=done&style=none&taskId=ub8cc9fb0-c64e-4a91-b7f3-f092105d136&title=&width=510.5)
还可以使用下述命令检查连通性：

```
./obsutil ls -s
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1700793632667-ca68a5a0-5e1a-4064-a256-d3554b6dbf9c.png#averageHue=%230a0908&clientId=ufebda536-4ec9-4&from=paste&height=167&id=u365d0ee4&originHeight=334&originWidth=1132&originalType=binary&ratio=2&rotation=0&showTitle=false&size=41807&status=done&style=none&taskId=uc0695651-b770-40ff-b06b-c54b7f6f21c&title=&width=566)
根据命令回显结果，检查配置结果：

- 如果返回结果中包含“Bucket number :”，表明配置正确。
- 如果返回结果中包含“Http status [403]”，表明访问密钥配置有误。
- 如果返回结果中包含“A connection attempt failed”，表明无法连接OBS服务，请检查网络环境是否正常。

最后就可以从华为云OBS桶中复制大模型微调代码和预训练权重，进入obsutil文件所在目录，执行下述命令：

```
./obsutil cp obs://dtse-models-guiyang1/competition/glm2/chatglm2_6b_lora  ../ -f -r
```
```
./obsutil cp obs://llm-mindspore-ei/wxp/chatglm2-6b/glm2_6b_ms.ckpt ../chatglm2_6b_loraglm2_6b.ckpt
```
然后就可以上传数据集到notebook，然后跟着步骤进行训练就可以啦。
### 百模千态开源大模型AI挑战赛指导手册[https://developer.huaweicloud.com/develop/aigallery/article/detail?id=3a8fdf15-915f-45da-b2ec-4a6292725524](https://developer.huaweicloud.com/develop/aigallery/article/detail?id=3a8fdf15-915f-45da-b2ec-4a6292725524)
## 评估结果：
physician_val_1.jsonl:
不是选择题格式
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701067785460-89d47d8a-66fb-4832-826d-1ab337524158.png#averageHue=%23dcdcdb&clientId=u5cc3845b-3417-4&from=paste&height=133&id=uc4d3edc7&originHeight=266&originWidth=1751&originalType=binary&ratio=2&rotation=0&showTitle=false&size=61484&status=done&style=none&taskId=u6ac5cbd8-584b-43ab-84e4-c0c56232956&title=&width=875.5)

physician_val.jsonl
选择题格式
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701068848564-aa4c8272-9c5c-4e28-b20d-e2d23fddaf25.png#averageHue=%23dedddd&clientId=u5cc3845b-3417-4&from=paste&height=159&id=u45f7a97b&originHeight=318&originWidth=1692&originalType=binary&ratio=2&rotation=0&showTitle=false&size=38386&status=done&style=none&taskId=u4e53618f-4eba-40b0-a437-c720eb43f50&title=&width=846)
physician_test_1.jsonl：非选择题格式
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701070949076-b39379f4-a94e-4bbf-9d2c-8154dfbff317.png#averageHue=%23dbdad9&clientId=u5cc3845b-3417-4&from=paste&height=100&id=u1701efeb&originHeight=199&originWidth=695&originalType=binary&ratio=2&rotation=0&showTitle=false&size=26244&status=done&style=none&taskId=ue3024856-f716-4cff-a35c-0dd338724e7&title=&width=347.5)

下一步：
让不是选择题格式的数据训练
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701070339749-a1d4b30a-5d38-403a-9db2-65d4a65d9ab5.png#averageHue=%23dbdad8&clientId=u5cc3845b-3417-4&from=paste&height=146&id=u918805f6&originHeight=292&originWidth=1773&originalType=binary&ratio=2&rotation=0&showTitle=false&size=69435&status=done&style=none&taskId=u18c6db12-e22f-4848-ba29-73b4f8b97c5&title=&width=886.5)

用不是选择题格式的数据进行训练，训练的损失比较正常：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701070443945-d679f603-d00f-467c-b8de-7303afcc2e24.png#averageHue=%23f6f4f3&clientId=u5cc3845b-3417-4&from=paste&height=487&id=u1d9902f0&originHeight=973&originWidth=1820&originalType=binary&ratio=2&rotation=0&showTitle=false&size=321192&status=done&style=none&taskId=u7f427a3a-4900-47f3-ab67-2724347c4fb&title=&width=910)


基准模型的评估：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701761899667-272c5b4e-6a0f-4de3-9e81-eea32a76a754.png#averageHue=%23f6f6f6&clientId=uf8e15034-ca12-4&from=paste&height=188&id=u938ef485&originHeight=375&originWidth=616&originalType=binary&ratio=2&rotation=0&showTitle=false&size=23049&status=done&style=none&taskId=udfd64310-df01-4f83-9d9f-a51257a1141&title=&width=308)
processed_test_datasets.jsonl
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701764610945-09c3d954-2312-44ba-aa73-ba9e753b5060.png#averageHue=%23dbdad9&clientId=uf8e15034-ca12-4&from=paste&height=157&id=uc508f1e8&originHeight=314&originWidth=1807&originalType=binary&ratio=2&rotation=0&showTitle=false&size=81123&status=done&style=none&taskId=u1acae60d-2308-4b89-80b2-8940f681b11&title=&width=903.5)

./obsutil cp ../mindformers/scripts/mf_standalone/output/checkpoint/rank_0/glm2-6b-lora_rank_0-1250_4.ckpt  obs://llm-mindspore-ei/wxp/chatglm2-6b/glm2_6b_ms.ckpt  obs://chatglm-3df5/checkpoint/glm2_6b.ckpt

最近用的一个训练模型评估结果：

processed_sampled_train_data.jsonl
processed_test_datasets.jsonl
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701787816924-48743014-62b1-4815-b32b-e3299ba7ddd4.png#averageHue=%23fbfaf9&clientId=ua85fe178-9a0f-4&from=paste&height=132&id=ue1373efa&originHeight=264&originWidth=1849&originalType=binary&ratio=2&rotation=0&showTitle=false&size=60474&status=done&style=none&taskId=ud4d49196-4728-4780-bd58-58a665bf6b6&title=&width=924.5)

这里挑选数据用的代码：
```
import json
import random

def adjust_output_length(output, reference_output, max_difference=20):
    output_words = output.split()
    reference_words = reference_output.split()

    # Adjust output length
    if len(output_words) > len(reference_words) + max_difference:
        output_words = output_words[:len(reference_words) + max_difference]
    elif len(output_words) < len(reference_words) - max_difference:
        output_words += reference_words[len(output_words) - max_difference:]

    return ' '.join(output_words)

def random_sample(input_data, output_data, sample_size):
    # Create a list of indices
    indices = list(range(len(input_data)))

    # Randomly shuffle the indices
    random.shuffle(indices)

    # Select a random subset of indices
    selected_indices = indices[:sample_size]

    # Create a new list to store sampled data
    sampled_data = []

    for index in selected_indices:
        input_text = input_data[index]['input']
        output_text = adjust_output_length(output_data[index]['output'], reference_output, max_difference=20)
        sampled_data.append({'input': input_text, 'output': output_text})

    return sampled_data

def save_to_jsonl(data, output_path):
    with open(output_path, 'w', encoding='utf-8') as file:
        for entry in data:
            file.write(json.dumps(entry, ensure_ascii=False) + '\n')

# Load data from train_datasets.jsonl
with open('train_datasets.jsonl', 'r', encoding='utf-8') as file:
    train_data = [json.loads(line) for line in file]

# Reference output for adjusting length
reference_output = "癫痫是一组由大脑神经元异常放电所引起的短暂中枢神经系统功能失常为特征的慢性脑部疾病，具有突然发生反复发作的特征。根据所侵犯神经原的部位和发放扩散的范围，功能失常可表现为运动感觉意识行为自主神经功能等不同障碍，或兼而有之。大多数的癫痫病人经过正规治疗都是可以治好的。癫痫是一种临床综合症，它的特征是大脑神经细胞反复发作的异常放电大脑功能失调导致癫痫发作。想要彻底杜绝癫痫发作就必须修复受损脑神经元细胞，平衡异常放电。只要病人积极配合，会达到较高的治愈率和理想的疗效。引发癫痫的因素有很多，最常见的就是头部受到巨大伤害，极有可能会造成癫痫疾病的发作，其次就是感冒高烧长时间不退，也有肯能会导致癫痫，还有就是饮食不规律，有一顿没一顿的吃，太饱了或者是太饿了都是会引发癫痫疾病的发生的，还有如果挑食或者是偏食，身体上面的营养跟不上就会造成营养不良，也是会引发癫痫的发生，同时长时间的熬夜，没有好的作息时间，同样的也是会引发癫痫疾病的。可以多方面的了解一些治疗信息以便更好的治疗，积极地面对病情，及时选择有效的治疗措施，积极的治疗，合理的用药，选择科学的治疗方案，癫痫病人应积极的面对病情，早日的摆脱癫痫疾病发作以及先兆带来的困扰，对于癫痫来说，最可靠的预防就是按时按量地服用抗癫痫药物。癫痫是一种临床综合症，它的特征是大脑神经细胞反复发作的异常放电，导致大脑功能失调。在治疗之前首先要查明原因分清类型对症治疗盲目的治疗是治不好病的癫痫本是一种慢性脑部疾病应选择一家专业的癫痫医院进行治疗。"

# Randomly sample 8000 data points and adjust output length
sampled_data = random_sample(train_data, train_data, 8000)

# Save the sampled data to a new JSONL file
save_to_jsonl(sampled_data, 'sampled_train_data.jsonl')

```


```
#删除停用词

import json
import jieba

def remove_stopwords(text, stopwords):
    words = jieba.cut(text)
    filtered_words = [word for word in words if word not in stopwords]
    return ' '.join(filtered_words)

def remove_stopwords_from_jsonl(input_file, output_file, stopwords):
    with open(input_file, 'r', encoding='utf-8') as infile, open(output_file, 'w', encoding='utf-8') as outfile:
        for line in infile:
            data_entry = json.loads(line)
            input_text = data_entry.get('input', '').strip()
            output_text = data_entry.get('output', '').strip()

            if input_text and output_text:
                input_text = remove_stopwords(input_text, stopwords)
                output_text = remove_stopwords(output_text, stopwords)

                updated_entry = {'input': input_text, 'output': output_text}
                outfile.write(json.dumps(updated_entry, ensure_ascii=False) + '\n')

# Load stopwords from cn_stopwords.txt
with open('cn_stopwords.txt', 'r', encoding='utf-8') as stopwords_file:
    cn_stopwords = set(stopwords_file.read().splitlines())

# Remove stopwords from sampled_train_data.jsonl and save to a new file
remove_stopwords_from_jsonl('sampled_train_data.jsonl', 'processed_test_datasets.jsonl', cn_stopwords)

print("Stopwords removal completed. Processed data saved to processed_sampled_train_data.jsonl.")

```

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701827955277-cec21206-c2d0-4ec3-9428-65fe219f4b8a.png#averageHue=%23f8f6f5&clientId=u6d0c1daf-71d5-4&from=paste&height=373&id=u90d6fdf8&originHeight=745&originWidth=1837&originalType=binary&ratio=2&rotation=0&showTitle=false&size=219030&status=done&style=none&taskId=u40903f9a-fc94-4f87-8b09-13a69ff04b9&title=&width=918.5)

报错：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701831256492-f704c49d-ee4f-4bb2-a9c8-aa2ce188c8e1.png#averageHue=%23f4d4d2&clientId=u6d0c1daf-71d5-4&from=paste&height=495&id=ue02f4be0&originHeight=989&originWidth=1792&originalType=binary&ratio=2&rotation=0&showTitle=false&size=304883&status=done&style=none&taskId=u5735aa78-7cb1-4ef0-bfe4-b65f1599145&title=&width=896)
解决办法：
去终端输入：
npu-smi info
显示现在正在进行的进程：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701831347947-0605b358-9b8d-4925-926d-561408f7d032.png#averageHue=%230d0c0c&clientId=u6d0c1daf-71d5-4&from=paste&height=274&id=u98e8e570&originHeight=548&originWidth=1657&originalType=binary&ratio=2&rotation=0&showTitle=false&size=55341&status=done&style=none&taskId=ud1f5dd8e-77b9-4632-aa7c-93c8e6629aa&title=&width=828.5)
使用kill命令终止这个进程：
kill -9 3257

然后从以下代码部分重新执行：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22838017/1701831397866-07fa104b-29c5-44ff-87ea-97ef8061eb00.png#averageHue=%23f5f1f0&clientId=u6d0c1daf-71d5-4&from=paste&height=80&id=u664accd6&originHeight=160&originWidth=1827&originalType=binary&ratio=2&rotation=0&showTitle=false&size=25753&status=done&style=none&taskId=uec620b7a-3574-4572-a40d-a304dac2cfa&title=&width=913.5)

最终提交判分系统为58.03分。

![](https://cdn.nlark.com/yuque/0/2023/png/22838017/1702378070084-ff349be9-74f9-4a4e-85bd-537c39e9dfc5.png?x-oss-process=image%2Fresize%2Cw_1500%2Climit_0#averageHue=%23fbfafa&from=url&id=HIrOy&originHeight=826&originWidth=1500&originalType=binary&ratio=2&rotation=0&showTitle=false&status=done&style=none&title=)

