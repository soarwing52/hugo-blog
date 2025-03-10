---

title: "InceptionV3 遷移學習教學介紹"

date: 2021-02-20

description: "InceptionV3 遷移學習教學介紹"

---



這是我在2019年找到的一個教學，後來這個人也沒有在拍新影片了，滿可惜的



這是目前我訓練最快卻也最精準的一個，其實我也還不知道為什麼，其他模型教學都沒有他訓練的精準，花的時間也都很久



[影片](https://www.youtube.com/watch?v=oXpsAiSajE0)



[github](https://github.com/MicrocontrollersAndMore/TensorFlow_Tut_2_Classification_Walk-

through)



最無腦的使用方法，就是直接clone下來之後



在資料夾內建立training_images資料夾，依照分類名稱建立子資料夾



然後執行retrain.py



或是



    

    

    python retrain.py --bottleneck_dir="C:\tf_files" --how_many_training_steps=500 --model_dir="C:\tf_files" --output_graph="C:\tf_files\retrained_graph.pb" --output_labels="C:\tf_files\retrained_labels.txt" --image_dir="C:\tf_files\training_images"



要把路徑 C:\tf_files改成自己在用的就可以了



接下來跑test.py就可以從opencv介面看到分類結果



而且訓練很快，總共一千多張的照片不到十分鐘就訓練完畢，而且最準



甚至他建議要跑4000次epoch我用測試的500次就已經很準了



從上次的道路鋪面分類，到這次的人孔都是這樣



這邊的遷移學習就是把softmax這個最後一層，把特徵分類的權重分別對應到類別(哪一種人孔)抽換，從原本的一千多種物件(鳥、車等等)換成這次訓練的種類



或許ImageAI會把特徵的權重也重新計算才會花比較久並且精準度因為照片數量不足而下降



從這篇 <https://keras.io/guides/transfer_learning/>



裡面可以看到，要調教模型要先把前面幾層freeze等到真正都調完還要精進再試著用更多資料，unfreeze



不過目前還沒做到這一步，還會持續對於這個ChrisDamm做的程式研究為主



