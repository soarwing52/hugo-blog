---

title: "WSL :在Windows上跑Linux"

date: 2021-02-19

description: "WSL :在Windows上跑Linux"

---



我在其他某篇看到一個Udemy的課程，又看到特價三百塊，於是就買來看看



[Deep Learning Computer Vision™ CNN, OpenCV, YOLO, SSD &

GANs](https://www.udemy.com/course/master-deep-learning-computer-visiontm-cnn-

ssd-yolo-gans/)



看完之後 是對於各種模型有了更深的認識，不過離調教出一個好模型還有很長的距離



他有給他安裝好的Ubuntu虛擬機，我用virtual box上面跑，不過實在是太慢了



我就開始嘗試之前有聽說過的，在Windows上面跑Ubuntu，只有指令視窗，沒有介面



先說結論：因為沒有辦法跑出opencv的顯示圖片(imshow) 而且其實這些套件在windows,

anaoconda上面都能跑，所以後來就直接繞回windows上面了



跟著他的教學是對於環境控制、套件安裝還不熟悉，以及對於踩各種神奇的坑還不熟的人適用



當初安裝tensorflow就是各種相依性套件不相容 python所有版本也給他試過 才終於搞定的



好的，進入正題：



在microsoft store就可以直接搜尋ubuntu 這裡可以用LTS(long-term support)的20.04版本



![](https://lh3.googleusercontent.com/jCk8hJJPKM8jO1CXPOKyW0iwFfzLp3mUyry0fT7sJY1sJuAXgrvbEibVSkZ8Ax920QaoFwWpW0IWyxGP0J9_VcjqtwajGkSXoSCK968zhXTwvCSRNkycd0mSG-gr6YiZMKastPbhvl5Z7Wev2lk5LB1AGJV0qZNr7RLmV1of5gXYBrmaRyQ5ZFgikUz-iB9tI6sBeNKQtjnBy065uhj7YxUZGMvpVd4mv1WhrBGSUIqc6pdKi79J-IjypQ4kvC9KAPhpwbD1B6m-9ba9IzTlxRMFCqegVkBgZE_vaGV4PnM9inpKOXB4bkyZ-wL2us7UXZEHiy1uSNrVBnNQGuYbcG3yFw7NsYCuOfsE8_m25V0tx5jIQGctwjA7h3L2T16EJfxeNV8SpHUETFEhSP7voEMrmhwM2SAYvhs9PM4GLlFD6L24PDfs5-sQE_CP--PFK-bbMCyfSbCtn5N6tSwr0TJhgyOVe3TZnEAdwxpMqGYqseYDe9_veN20Vfrbl_AGGOdOUvRHsSiKVMvCqZZBxGV1VV5f8j5lWcaoFTJjlQamuYKP1CyyC-WxxH8e-7ayz13tVKFsareZpa_Wc37f_IVb0jl5pgr6ELcw_z_mRevJPDvwEUXYgp9yZEfRi5EKeIcP-cyycbo6OyDeMOeGY0qPIFz7vpfLE1Nuvwom8xtmR7NVqWZM5nlYJ9hMTg=w1200-h937-no?authuser=0)


安裝好之後就可以直接打開，下指令了



然後這中間要傳檔案 就用



    

    

    explorer.exe .



就可以打開檔案總管，丟檔案 像是.py .ipynb等等進去



然後打開之後 一樣可以打開conda



    

    

    source activate env_name



要打開jupyter notebook則是要注意



    

    

    jupyter notebook --no-browser



因為這裡不能打開介面瀏覽器，也可以額外設定，不過我每次重開bash之後設定都會重置



到這裡基本上就可以把ubuntu在windows上直接跑起來啦



然後要跑docker或是其他ubuntu上的 pytorch等等都可以從這裡開始了



