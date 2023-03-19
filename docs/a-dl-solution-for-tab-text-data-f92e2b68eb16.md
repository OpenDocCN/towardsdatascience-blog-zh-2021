# 制表文本数据的 DL 解决方案

> 原文：<https://towardsdatascience.com/a-dl-solution-for-tab-text-data-f92e2b68eb16?source=collection_archive---------27----------------------->

## 它能打败 XGBoost 吗？

![](img/60a2cfaf421fb3e58cc21ee97f567b27.png)

图像[去飞溅](https://images.unsplash.com/photo-1495592822108-9e6261896da8?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1740&q=80)

## 动机

DL 模型在视觉和自然语言处理等领域取得了巨大的成就。然而，当涉及到表格数据时，它似乎显示出较低的成功率，并且在大多数应用程序中，它的性能比 [XGBoost](https://machinelearningmastery.com/gentle-introduction-xgboost-applied-machine-learning/) 差。

有很多原因可以解释这种现象，例如，对于表格数据没有明显的可排序度量(当我们处理布尔变量或类别时，我们可以优先假定的唯一度量是离散的)。但是，对组合数据的处理很少:既包含表格列又包含文本的数据。在这篇文章中，我将介绍这种数据库的 DL 解决方案。我们的目标变量将是一个多分类变量。

为了展示引擎，我将使用 [scikit-learn](https://scikit-learn.org/stable/) 的数据集服务创建一个模拟数据库。

```
**def** get_mock_db(my_pickle_name):
     covert=fetch_covtype(data_home=**None**, download_if_missing=**True**,
                            random_state=**None**, shuffle=**False**)
     categories=[**'alt.atheism'**, **'soc.religion.christian'**,            
                       **'comp.graphics'**, **'sci.med'**]
     twenty_train = fetch_20newsgroups(subset=**'train'**,remove= 
           (**'headers'**, **'footers'**, **'quotes'**), categories=categories,          
                                shuffle=**True**, random_state=42)clean_text=[(i.replace(**'\n'**,**''**).replace(**'\t'**,**''**).replace('\\',
     ''),j) for i,j in **z**ip(twenty_train.data,twenty_train.target)]    
     clean_text =[  i **for** i **in** clean_text **if** len(i[0])>20]
     len_0 =len([i **for** i **in** clean_text **if** i[1]==0])
     len_1 =len([i **for** i **in** clean_text **if** i[1]==1])
     len_2= len([i **for** i **in** clean_text **if** i[1]==2])
     len_3 =len([i **for** i **in** clean_text **if** i[1]==3])
 a, b =covert.data.shape
 cov_0 = [covert.data[j] **for** j **in** range(a) **if** covert[**'target'**]
     [j] ==4 ][:len_0]cntr=0
**for** j **in** range(len(clean_text)):
    **if** clean_text[j][1]==0:
            a1,b1 =clean_text[j]
            clean_text[j]= (a1, [cntr],b1)
        cntr+=1
    **if** cntr ==len_0:
            **break** cov_1 = [covert.data[j] **for** j **in** range(a) **if** covert[**'target'**]
     [j]==1 ][:len_1]
cntr=0
**for** j **in** range(len(clean_text)):
    **if** (len(clean_text[j])==2) **and** clean_text[j][1]==1:
        a1,b1 =clean_text[j]
        clean_text[j]= (a1, cov_1[cntr],b1)
        cntr+=1
    **if** cntr ==len_0:
        **break**cov_2=[covert.data[j] **for** j **in** range(a) **if** covert[**'target'**][j]==3 ][:len_2]
cntr=0
**for** j **in** range(len(clean_text)):
    **if** (len(clean_text[j])==2) **and** clean_text[j][1]==2:
        a1,b1 =clean_text[j]
        clean_text[j]= (a1, cov_2[cntr],b1)
        cntr+=1
    **if** cntr ==len_0:
        **break**cov_3=[covert.data[j] **for** j **in** range(a) **if** covert[**'target'**][j]==3 ][:len_3]
cntr=0
**for** j **in** range(len(clean_text)):
    **if** (len(clean_text[j])==2) **and** clean_text[j][1]==3:
        a1,b1 =clean_text[j]
        clean_text[j]= (a1, cov_3[cntr],b1)
        cntr+=1
    **if** cntr ==len_0:
        **break
with** open(my_pickle_name, **'wb'**) **as** f:
    pickle.dump(clean_text, f, pickle.HIGHEST_PROTOCOL)
print (**"files_genrated"**)
**return**
```

我们可以用多种方式转换表格数据，比如按原样接受整数列，执行确定性聚合，或者将布尔变量转换为一个统一的编码表示。然而，在这篇文章中，我将尝试一种不同的方法:我将为。表格数据。为此，我们将使用 [Tabnet](https://github.com/dreamquark-ai/tabnet) 。这个[包](https://arxiv.org/pdf/1908.07442.pdf)提供了几种嵌入方法。

下面的代码执行这样的嵌入

```
**from** pytorch_tabnet.pretraining **import** TabNetPretrainer
clf = TabNetPretrainer()   clf.fit(X_train[:n_steps])#Here we embed the data 
embed_tabular = clf.predict(X_test)[1]raw_data =[(text[j], embed_tabular[j], label[j]i[0],j,i[2]) **for** i,j **in** zip(data, embed_tabular]
```

现在，数据已经准备好并保存到 pickle 文件中。我们可以使用 [Huggingsface 的](https://huggingface.co/)分词器对文本进行预处理

## 标记化

在这一部分中，我们将文本转换成一种允许 Huggingsface 引擎执行嵌入的格式。我习惯了标记化器的类型:

[伯特](https://huggingface.co/bert-base-multilingual-uncased)和[FnetT5**。**前者因](https://huggingface.co/google/fnet-base)[变压器](https://arxiv.org/pdf/1706.03762.pdf)的出现而广为人知，后者是一种全新的方法，建议用 FFT 代替多头层。人们可以在这里了解它。

```
**def** get_tokenizer(data_yaml, bert_tokeinizer_name):
    **if** data_yaml == enum_obj.huggins_embedding.fnet.value:
        **return** FNetTokenizer.from_pretrained(**'google/fnet-base'**)
    tokenizer = AutoTokenizer.from_pretrained(bert_tokeinizer_name,   
             use_fast=**True**)
    **return** tokenizerbatch_encoding = tokenizer.batch_encode_plus([text  **for text ** **in** data], **tokenizer_params, return_tensors=**'pt'**)
```

根据外部 Yaml 标志，我们选择我们的记号赋予器。最后一行是用伪代码写的，表示令牌化步骤本身。

## 创建张量文件夹

为了训练和评估，我们需要将数据(表格和文本的组合)放入张量文件的文件夹中，这些文件将使用 Pytorch 的**数据加载器**上传到神经网络。我们给出了生成这个文件夹的几段代码。我们从这两个函数开始:

```
 **def** bert_gen_tensor(input_t, tab_tensor, lab_all_Place,     
       file_name, batch_enc, index_0):
    input_m = torch.squeeze(torch.index_select(
         batch_enc[**"attention_mask"**], dim=0,                   
                 index=torch.tensor(index_0))) 
    torch.save([input_t, input_m, tab_tensor, lab_all_Place],     
            file_name)
    **return

def** fnet_gen_tensor(input_t, tab_tensor, lab_all_Place,          
           file_name, batch_enc=**None**, index_0=-1):  
     torch.save([input_t, tab_tensor, lab_all_Place], file_name)
     **return**
```

人们可以看出它们几乎是相似的。它们反映了 **Bert** 和 **Fnet 所需要的张量之间的差异，** Bert 需要一个键: **attention_mask"** ，而 Fnet 不存在这个键。因此，一方面，我们需要为每种嵌入方法保存不同的张量集，而另一方面，我们希望有一个唯一的代码。我们如下解决这个问题:

```
**def** generate_data_folder_w_tensor(data_lab_and_docs, data_yaml):
 .
 . **if** data_yaml[**'embed_val'**] ==               
                       enum_obj.huggins_embedding.fnet.value:
        proc_func = fnet_gen_tensor
    **else**:
        proc_func = bert_gen_tensor
```

除了关键问题之外，该文件还包含输入数据、表格数据和标签。我们对这两种方法的需求处理如下:

我们现在准备迭代数据项并创建 tensors 文件夹

```
**for** i, data **in** enumerate(data_lab_and_docs):
        file_name = pref_val + **"_"** + str(i) + **"_"** + suff_val
        tab_tensor = torch.tensor(data[1], dtype=torch.float32)
        input_t =  
      torch.squeeze(torch.index_select(batch_encoding[**"input_ids"**],dim=0,    index=torch.tensor(i)))

        proc_func(input_t, tab_tensor,  data[2],file_name, 
                batch_enc= batch_encoding, 0index_0=i)
        dic_for_pic[file_name]= data[2]

        **with** open(data_yaml[**'labales_file'**], **'wb'**) **as** f:
           pickle.dump(dic_for_pic, f, pickle.HIGHEST_PROTOCOL)
```

这个循环非常简单；我们设置文件名并使用函数来保存信息。我们创建一个字典，将文件名映射到它们的标签值，以备将来需要。

当过程结束时，我们有一个 tensors 文件夹用于训练和评估步骤。我们差不多可以开始训练了。差不多？是啊！因为我们正在使用 Pytorch，所以我们需要创建我们的**数据加载器。**

```
**import** torch
**import** enum_collection **as** enum_obj
**import** random

**class** dataset_tensor :
    **def** __init__ (self, list_of_files, embed_val ):

        self.list_of_files =list_of_files
        random.shuffle(list_of_files)
        **if** embed_val==enum_obj.huggins_embedding.fnet.value:
            self.ref =[0,1,2]
        **else**:
            self.ref=[1,2,3]

    **def** __getitem__(self, idx):
       aa =torch.load(self.list_of_files[idx])
       **return** aa[0], aa[self.ref[0]], a[self.ref[1]],aa[self.ref[2]]

    **def** __len__(self) :
        **return** len(self.list_of_files)
```

您可能会注意到，这是 Pytorch 数据加载器的标准结构。有一件事需要澄清: **self.ref** 数组。由于 **Fnet** 和 **Bert** 在单个文件中使用不同的张量集合，并且我们希望使用相同的训练程序，我们为 **Fnet** 输出一个 void 变量(输入项两次)。 **self.ref** 决定我们输出的文件中张量的索引。

# **型号**

这可能是代码中最有趣的部分。该模型执行两个步骤:

*   嵌入按照给定的配方( **Fnet** 或 **Bert** )嵌入标记化的数据
*   处理嵌入张量，并将其映射为类别数量大小的张量

第一步取决于我们选择的嵌入类型。因此，我们要求他们每个人都有一个特殊的“**前锋**

```
**class** my_fnet(FNetForSequenceClassification):

    **def** __init__(self, config, dim=768):
        super(my_fnet, self).__init__(config )
        self.dim= dim
        self.num_labels = 4

        self.distilbert = FNetModel(config)
        self.init_weights()

        self.pre_classifier = nn.Linear(self.dim, self.dim)
    **def** forward(self,  input_ids=**None**):
            outputs = self.distilbert( input_ids=input_ids)
            pooled_output = outputs[1]
            pooled_output = self.dropout(pooled_output)
            **return** pooled_output**class** my_bert(BertForSequenceClassification):
 **def** __init__(self, config, dim=768):
        super(my_bert, self).__init__(config )
        self.dim= dim
        self.num_labels = 4
   self.distilbert = BertModel(config)
        self.init_weights()

        self.pre_classifier = nn.Linear(self.dim, self.dim)
    **def** forward(self,
                    input_ids=**None**,
                    attention_mask=**None**,
                    head_mask=**None**,
                    inputs_embeds=**None**,
                    output_attentions=**None**,
                    output_hidden_states=**None**,
                    return_dict=**None**,

                    ):
            return_dict = return_dict 
          **if** return_dict **is not None else** self.config.use_return_dict
          distilbert_output = self.distilbert(
                input_ids=input_ids,
                attention_mask=attention_mask,
                head_mask=head_mask,
                inputs_embeds=inputs_embeds,
                output_attentions=output_attentions,
                output_hidden_states=output_hidden_states,
                return_dict=return_dict,
            )
            hidden_state = distilbert_output[0]  pooled_output = hidden_state[:, 0]  *# pooled_output = self.pre_classifier(pooled_output)  
            # hidden_state = distilbert_output[0]* **return** pooled_output
```

模型本身如下所示:

```
**class** my_model(nn.Module):
   **def** __init__(self, data_yaml, my_tab_form=1, dim=768):
        super(my_model, self).__init__( )
        self.forward =self.bert_forward
        **if** data_yaml[**'embed_val'**] ==    
               eum_obj.huggins_embedding.distil_bert.value:
            self.dist_model  = my_dist.from_pretrained(**'distilbert-           
                         base-multilingual-cased'**,num_labels=4 )
        **elif** data_yaml[**'embed_val'**] ==   
          enum_obj.huggins_embedding.base_bert.value:
           self.dist_model =   
           my_bert.from_pretrained(**'bert-base-multilingual-cased'**,  
                             num_labels=4)
        **else**:
            self.dist_model = my_fnet.from_pretrained(**'google/fnet-     
                       base'**, num_labels=4)
            self.forward = self.fnet_forward
        **if** my_tab_form>0 :
           localtab= data_yaml[**'tab_format'**]
        **else** :
            localtab =my_tab_form
        **if** localtab ==  enum_obj.tab_label.no_tab.value:
            print (**"no_tab"**)
            self.embed_to_fc = self.cat_no_tab
            self.tab_dim = 0
        **else** :
            self.embed_to_fc = self.cat_w_tab
            self.tab_dim =data_yaml[**'tab_dim'**]

        self.dim=dim
        self.num_labels =4

        self.pre_classifier = nn.Linear( self.dim, self.dim)
        self.inter_m0= nn.Linear(self.dim +self.tab_dim,216)

        self.inter_m1 = nn.Linear(216,64)
        self.inter_m1a = nn.Linear(64, 32)

        self.inter_m3 = nn.Linear(32, self.num_labels)
        self.classifier = nn.Linear(self.dim, self.num_labels)
        self.dropout = nn.Dropout(0.2)

   **def** cat_no_tab (self, hidden, x):
        **return** hidden
    **def** cat_w_tab (self, hidden, x):
        **return** torch.cat((hidden, x),dim=1)

    **def** fnet_forward(self, x,
                 input_ids=**None**, attention_mask=**None**):

        hidden_state = self.dist_model(input_ids)

        pooled_output = torch.cat((hidden_state, x), dim=1)
        pooled_output = self.inter_m0(pooled_output)  *# (bs, dim)* pooled_output = nn.ReLU()(pooled_output)  *# (bs, dim)* pooled_output = self.dropout(pooled_output)  *# (bs, dim)* pooled_output = self.inter_m1(pooled_output)  *# (bs, dim)* pooled_output = nn.ReLU()(pooled_output)  *# (bs, dim)* pooled_output = self.dropout(pooled_output)  *# (bs, dim)* pooled_output = self.inter_m1a(pooled_output)   pooled_output = nn.ReLU()(pooled_output)  pooled_output = self.dropout(pooled_output)    logits = self.inter_m3(pooled_output)  **return** logits

   **def** bert_forward(self, x, input_ids=**None**,                
        attention_mask=**None**) :
        hidden_state =self.dist_model(input_ids, attention_mask)
        pooled_output = self.embed_to_fc(hidden_state, x)

        pooled_output = self.inter_m0(pooled_output)  pooled_output = nn.ReLU()(pooled_output)  *# (bs, dim)* pooled_output = self.dropout(pooled_output)  *# (bs, dim)* pooled_output = self.inter_m1(pooled_output)  *# (bs, dim)* pooled_output = nn.ReLU()(pooled_output)  *# (bs, dim)* pooled_output = self.dropout(pooled_output)  *# (bs, dim)* pooled_output = self.inter_m1a (pooled_output)  *# (bs, dim)* pooled_output = nn.ReLU()(pooled_output)  *# (bs, dim)* pooled_output = self.dropout(pooled_output)  *# (bs, dim)* logits = self.inter_m3(pooled_output)  *# (bs, num_labels)* **return** logits
```

除了我们已经讨论过的不同转发功能之外。我们有 **my_tab_form** 标志。这个标志表示我们是单独使用文本还是组合数据(我们通常使用后者)。在代码方面，我们用一个 void 函数代替 **torch.cat** (它输出输入张量)

```
**if** my_tab_form>0 :
           localtab= data_yaml[**'tab_format'**]
**else** :
            localtab =my_tab_form
```

## 损失函数

我们的损失往往是一个标准的交叉熵。尽管如此，因为对于某些应用程序，我们需要减少与其中一个类别相关联的特定故障，所以我添加了几个特定的函数来“唯一地惩罚”这些错误。人们可以在这里阅读更多

# 主循环

最后，我们可以使用来自外部的整个块来执行训练步骤。我们从带来数据开始:

```
**with** open(**"project_yaml.yaml"**, **'r'**) **as** stream:
    data_yaml = yaml.safe_load(stream)
list_of_files =os.listdir(data_yaml[**'tensors_folder'**])
list_of_files =[data_yaml[**'tensors_folder'**]+i **for** i **in** list_of_files]X_train,X_test =train_test_split(list_of_files, test_size=0.2)# Creating dataloader!!
train_t =dataset_tensor(X_train, data_yaml[**'embed_val'**])
train_loader = DataLoader(train_t, batch_size=data_yaml[**'batch_size'**], shuffle=**True**)

test_t = dataset_tensor(X_test, data_yaml[**'embed_val'**])
test_loader = DataLoader(test_t, batch_size=data_yaml[**'batch_size'**], shuffle=**True**)
```

我们上传 Yaml 文件并将数据分割以进行训练和测试(例如，0.2 的测试大小不需要深入的理论支持)。对于不太熟悉 torch 的读者来说，最后几行为训练和测试文件创建了数据加载器。

```
device = **""
if** torch.cuda.is_available():
    device = torch.device(**"cuda:0"**)

model = my_model(data_yaml) **#pre -training usage
if** data_yaml[**'improv_model'**]:
    print(**"Loading mode"**)
    model_place = data_yaml[**'pre_trained_folder'**]
    print (model_place)
    model.load_state_dict(torch.load(model_place, map_location=**'cpu'**))

**if** device:
    model =model.to(device)
```

我们为拥有 GPU 的幸运读者设置了设备。之后，我们定义模型结构并上传其权重，以防我们希望使用预训练的模型。

```
optimizer = torch.optim.AdamW(model.parameters(),
                              lr=1e-5, eps=1e-8)
loss_man = loss_manager(data_yaml[**'batch_size'**],   
        data_yaml[**'target_val'**],data_yaml[**'reg_term'**])
model.train()
```

我们定义了优化器(它应该降低大多数变压器任务的学习率)和损失函数。现在我们可以执行训练迭代

```
**for** i_epoc **in** range(data_yaml[**'n_epochs'**]):
    running_loss = 0.0
    counter_a=0
    **for** batch_idx, data **in** enumerate(train_loader):

        a, b, d, c=  data
        **if** device :
            a=a.to(device)
            b=b.to(device)
            c=c.to(device)
            d=d.to(device)

        ss=model(x=d, input_ids=a, attention_mask=b)

        loss =loss_man.crit(ss, c)
        running_loss += loss.item()

        loss.backward()
        print(loss, batch_idx)
        optimizer.step()
        optimizer.zero_grad()

    print (**"Epoch loss= "**,running_loss/(counter_a+0.))
    print (**"End of epoc"**)
    torch.save(model.state_dict(),  data_yaml[**'models_folder'**] + **"model_epoch_"** + str(i_epoc) + **".bin"**)
```

我们可以在培训结束时使用 **tqdm** 添加一个评估循环:

```
**from** tqdm **import** tqdm
.
.
. **with** torch.no_grad():
    **with** tqdm(total=len(test_loader), ncols=70) **as** pbar:
        labels = []
        predic_y = []
        **for** batch_idx, data **in** enumerate(test_loader):

            a, b, d, c = data
            **if** device:
                a = a.to(device)
                b = b.to(device)
                c = c.to(device)
                d = d.to(device)
            labels.append(c)
            outp = model(x=d, input_ids=a, attention_mask=b)
            probs = nn.functional.softmax(outp, dim=1)
            predic_y.append(probs)
            pbar.update(1)

        y_true, y_pred = convert_eval_score_and_label_to_np(labels, predic_y)
```

结果和他们的分析留给他们自己感兴趣的读者。

# 摘要

我们提出了一个组合数据模型和一个训练它的 DL 引擎。从架构上讲，我们提出了一个简单的神经网络。但是，对于一些经过训练和测试的任务，它显示出比类似的 XGBoost 模型更好的结果。这给了 DL 引擎能够很好地处理表格数据的希望。除了单纯的 DL 方面之外，我相信组合数据任务被赋予了很好的数学谜题，因为它不仅研究不同的数据类型，还研究它们所导致的度量和拓扑。可排序和不可排序变量以及完全和不完全拓扑之间的接口可以提供广泛的研究领域。在阅读这篇文章时，人们可能会问的另一个问题是，我们是否可以改进用于 Fnet 的 FFT(例如，使用小波)。

## 承认

我希望感谢 Uri Itai 在撰写本文期间提供了富有成效的想法。

代码位于[这里](https://github.com/natank1/Text_Tab-DL)。