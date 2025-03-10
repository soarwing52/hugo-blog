---

title: "重拾ImageAI #再出發要走更遠"

date: 2021-02-19

description: "重拾ImageAI #再出發要走更遠"

---



上次使用Olafen兄弟開發的ImageAI這個套件是2019年在自動辨識車、人已進行馬賽克的時候了



[GitHub專案](https://github.com/soarwing52/Blur_for_Privacy)



當時就是很簡單的找出所有物件，選擇其中式汽車、人的邊界(Bounding Box)然後用OpenCV模糊化處理



經過一年半，TensorFlow也從1進展到2，很多功能都改了，ImageAI也一同演進了，現在使用的是2.4的版本



這次專案要做的有幾項



1.把舊專案重新跑起來



2.針對人孔蓋進行遷移學習，做影像分類



3.進行物件辨識，找出圖片中的人孔蓋，如果可以加以分類



本文就作為本次重新對影像AI做研究的大綱，並在接下來的開發過程中持續更新



