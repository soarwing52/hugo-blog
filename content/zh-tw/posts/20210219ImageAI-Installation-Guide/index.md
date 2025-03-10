---



title: "安裝ImageAI(conda虛擬環境、安裝)"



date: 2021-02-19



description: "安裝ImageAI(conda虛擬環境、安裝)"



---







上次在使用時，是使用PyCharm作為IDE，然後對於NoteBook使用也不多，因此只有用基本的python







這次手上多了幾套工具，於是就用Anaconda來做環境控制，不過主要就是conda install跟pip install的差別







安裝Anaconda這部分，就不多贅述







在打開時，可以用開始中的Anaconda Prompt







![](https://lh3.googleusercontent.com/9rXcqOSayUVD1947Oi_iUibzwooviEyONv6Mg-fyqQHWmAapF8lEZU167TXbo6Ua7AN6Qz4M9Q8g1DNahYBwwwiWfZmPXmWIuGhI3HqL9-56CkE0OxqoBzmCrMg7-AzVcgVvTN-UwDQjqCBmwEvvTEQaddq1RgkP3ibSP4o66FcMl10TvIYbX6Yxc4gudJtD6MiI2svCUoprGaEXCpcok_m_ENME0TPvwyTPqpQxidDuP1hfWHkvAtQImMnoJ8Pvzpr_29wj_GiUT1oIvn5b5oztj6Q4wlMOSfo88IWPTfpl-MKKpZS5pGHQzL_mCfeU4EWBuCbYz4Z3u4sZfYOSR5kYh2CXgiLISJXnGwF_kIgrgYlfAObENjQ0br-qr8V4cS7fFPT2nWAeA8HxRbqzyLWwXtjOvgRQF5wKYkZtCoffWZLcDWSFjEUk784HBfZGUBC7BH2ATG_qeistuh8ut8xvtZSfxmvoJlyCUqLqTQZDpIyRFDDbAjFvTqbC2gSTHOwLqUK3HVy_arp8u9_DVKdkpRQtaE9nQVxeobzdL04Ft5RFCTdBJ5KH2p2yZLEp3ngkC9wzq86uRtJdznkJzI7WemMVP5uwROGmQV7T8NW9rHfaUMEKqHCbVoHGL2LLH2deWUjHraZutI_mF8iCKyiQ2DBgfXKt4WEXk1J1vtU4nCBmsngbZEEHzNwBUg=w978-h513-no?authuser=0)





也可以用D:/Users/[使用者名稱]/anaconda3/Scripts/activate來啟動







在這裡我是用VS CODE裡面的介面操作 在windows指令介面也同樣可以







後面幾行的開啟環境則是等等會解釋







![](https://lh3.googleusercontent.com/9MPMI5W58nRK9egf87cW1zYvUzs7B_q-r79HZBlHAV_lvqfxaOWi7-02GnOpwAYsMuXt-OvzIDXRUoRnSpWx5sB1tFTLdKxsZGok4Cgk74L6iGJ4LD9N2kq0id5VSXQ_Q-2_yf95xt1ZprJRSd8MtcdHhGim_Inr-aarCeYztiB8VamfX7lIILnYKS4ICxnEEhdIOXkGy1sz7Qw5dblZ2M_TRKOFPJ1Bv1RCVCLW315n1pGomlfC3-aMnOc_NxckZxHjHTNu7azz_e9psFYbjEXTgsDZQlJpaUnhfxdF64r10vLkkra5kEcNPGinXiQSWsfBKJ7iPHmPvUjBdqjHOT-IQ6pxGGhnWEbIP83BadoKi6q0neR_9YinSgF1KmfXL6ZnnrEKKwUv4bEXzowVuoc5I8O37vT5PW6BrQZY_W2EiT_IjD59-ywOBBcERlP6W4wJg-TlGzvXqFdqbnXVtTLgXq8GQM581fTp1UjiDT92DmPACDZGpT4B7VOoqL--0mX5luJ0ps5DMIeaQWok4HRBBG2fTo6Ljrp742VVjHNkkDrgBZFKD-QFh6fRpltSPASsikhjRdAbcWUljXcZ8tPCntKDHdE0AWvWXBpwYmsixM0vv_94o72K3y7JYHrV6x6rRk-oLfRXZos5ZrvpSXMBlj4VhzrqPtzH7W06IUjTYyqhUXgkmsQDLSpdrA=w1560-h315-no?authuser=0)





好的，在Anaconda裡面時，會前面有(base)代表這是核心環境，我們在做不同專案的時候會安裝不同版本的套件甚至是python本身，因此這個base是個乾淨的環境，建置其他的時候從這裡開始。







好，第一步就是建置虛擬環境







    



    



    conda create --name env_name python=3.6







這裡用3.6版本 因為在3.8以上會有部分套件不相容







我常用的還有 conda env list







    



    



    (olaf) D:\TFtrain\imgai>conda env list



    # conda environments:



    #



    base                     D:\Users\Jay\anaconda3



    olaf                  *  D:\Users\Jay\anaconda3\envs\olaf



    tf15                     D:\Users\Jay\anaconda3\envs\tf15



    tf36                     D:\Users\Jay\anaconda3\envs\tf36







這裡就顯示了我有哪幾個環境







然後用 conda activate env_name 來開啟







    



    



    D:\TFtrain\imgai>D:/Users/Jay/anaconda3/Scripts/activate



    



    (base) D:\TFtrain\imgai>conda activate base



    



    (base) D:\TFtrain\imgai>conda activate olaf







這段就是開啟conda 然後再開啟我的環境 叫做olaf







接下來就是安裝ImageAI了







    



    



    pip install tensorflow==2.4.0



    pip install keras==2.4.3 numpy==1.19.3 pillow==7.0.0 scipy==1.4.1 h5py==2.10.0 matplotlib==3.3.2 opencv-python keras-resnet==0.2.0



    pip install imageai --upgrade







這樣就完成囉!







我中間踩過的坑主要都是版本問題，從python的版本(3.8,3.7,3.6)







tensorflow 1.16還是2.4.0這幾個都是







所以在安裝的時候要注意是哪一個版本才是對的，或是多試幾個，在安裝tensorflow的時候是真的滿久的







中間還有幾個教學是在ubuntu上面，於是有用了virtual box, WSL兩個 會在下一篇講解







