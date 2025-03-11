---

title: "非常簡單的遷移學習 ImageAI"

date: 2021-02-20

description: "非常簡單的遷移學習 ImageAI"

tags:
  - System
  - Work

---

基本上他們已經把這件事情做到沒難度了 照著[原文](https://towardsdatascience.com/train-image-

recognition-ai-with-5-lines-of-code-8ed0bdd8d9ba)做 30行內就解決了

首先在pip/conda安裝好ImageAI之後 就把訓練資料結構設好

在要操作的.py檔案旁 先創造一個資料夾 "總架構資料夾"

在總架構資料夾下面放入訓練資料 train, test 兩個資料夾

裡面則是要的類別資料夾，以我為例就是 雨水人孔、電力人孔、電信人孔，我分別用rain, electricity, telecom代表

接下來就是把他要的程式碼貼上

    

    

    from imageai.Classification.Custom import ClassificationModelTrainer

    

    model_trainer = ClassificationModelTrainer()

    #model_trainer.setModelTypeAsDenseNet121()

    model_trainer.setModelTypeAsInceptionV3()

    #model_trainer.setModelTypeAsResNet50()

    #model_trainer.setModelTypeAsMobileNetv2()

    model_trainer.setDataDirectory("總架構資料夾")

    model_trainer.trainModel(num_objects=3, num_experiments=30, enhance_data=True, batch_size=6, show_network_summary=True)

這裡就可以稍微解釋一下了，雖然也是網路上各文章現學現賣

他提供了四種模型

**MobileNetv2** , **ResNet50** , **InceptionV3  **以及 **DenseNet121**

這裡就可以對這幾個模型做基礎的介紹了

以大小、速度來說 MobileNet最小最快，DensNet最大最慢也最精準

我在做的過程中除了MobileNet都訓練過了，目前尚未進行比較分析，完成後會更新於此

這次比起上次的一個突破點是GPU的使用，上次怎麼用都沒法用GPU訓練，只能用CPU跑

這次終於跳出這行

    

    

    2021-02-20 09:04:37.489225: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library cudart64_110.dll

這裡我安裝了CUDA 11.0的版本 然後去下載cuDNN (需要申請NVidia會員)然後把裡面的 bin, include,

lib都放在C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.0 裡面的相應資料夾

然後就可以使用啦!

我中間踩過的坑 是一開始下載了最新版，不能用，然後網路上有需要10.0版，結果是太舊的版本

其他都是看dll哪一個沒讀到 然後去搜尋問題在哪裡

一段就是真的要把cuDNN資料都複製進去才會跑

還有一個是需要把dll從後面的64_110改成64_08等這種版本小問題

之後用GPU跑的速度，從原本的一個epoch接近900秒變成90秒

不過batch size也必須從遠本的32縮小到6-10

這裡就解釋一下最後一行的train model 可以做的調整

第一個num_objects一定要改成本專案的種類數量 原本的範例是10種，以我這邊為例 就是3種

num_experiments就是只要跑幾次迴圈，每一次如果準確度比上一次好，就會存出一個.h5檔案

enhance_data是選擇是否讓程式產生調教過的影像進行訓練

batch_size就是一次框選幾張影像進行訓練，光是這個本身就可以是好幾篇文章了，不過以最快的概念，就是把500張照片用batch10就是一次十張，跑50次，batch1就是跑500次

而看到的文章是多用1-32之間可以得到最好結果

Show_network_summary：是否顯示模型架構

等到跑完訓練之後呢，做一個新的.py檔案

    

    

    from imageai.Classification.Custom import CustomImageClassification

    import os

    

    execution_path = os.getcwd()

    

    prediction = CustomImageClassification()

    prediction.setModelTypeAsResNet50()

    prediction.setModelPath(".h5檔案名稱")

    prediction.setJsonPath("model_class裡面的.json名稱")

    prediction.loadModel(num_objects=分類數量)

    

    predictions, probabilities = prediction.predictImage(".jpg名稱", result_count=3)

    

    for eachPrediction, eachProbability in zip(predictions, probabilities):

        print(eachPrediction , " : " , eachProbability)

要把.h5, .json都放在與.py相同的資料夾 或是 要把目錄名稱也寫到string裡面

result_count參數代表要看到前幾個預測結果，如果今天有1000種分類，就只要選前幾項最高可能性的，其餘則不需要

* * *

基本上到這邊就很簡單的可以完成訓練，不過訓練出來結果不如我19年找到的一份訓練教學，也適用InceptionV3作為訓練模型，下一篇來介紹

也希望能盡快找到訓練出更精準模型的方法，以及為什麼，未來才能調教他

