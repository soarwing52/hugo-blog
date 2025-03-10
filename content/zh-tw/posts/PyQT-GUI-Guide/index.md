---

title: "RealSense 學習筆記/教學/分享（六）：PyQT製作介面GUI"

date: 2019-11-06

description: "RealSense 學習筆記/教學/分享（六）：PyQT製作介面GUI"

---



GUI: graphic user interface 要給其他同事使用務必要做成有介面 所以就在主要程式都完成後，開始這部分

我做了兩版，Tkinter跟PyQt Tkinter真的太2000年了 雖然是內建很方便，但是就是要用新一點的 最後做出來的是如下：



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/1ac2b-pyqtcam.jpg)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/65228-pyqtcam.jpg)



[GitHub](https://github.com/soarwing52/RealsensePython/blob/master/qtcam/qtcam.py)

<https://github.com/soarwing52/RealsensePython/blob/master/proccessor.py>

這邊就直接講PyQT跟我用到的功能吧 首先就是 QtDesigner 這也是為什麼直接推這個，因為要做GUI最好有個做GUI的GUI



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/28f3e-qtdesigner.jpg)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/28f3e-qtdesigner.jpg)



基本上把想要的功能丟進去 拉一下 我用到的就是 layout 可以自動排列 直/橫/表格，有看到推薦用 Group

這幾個都有一個好處，可以同時改變所有內含的字體 例如這個直列有四個按鈕，可以同時改變所有字體，而不用每個按鈕改

然後這裡要注意的是，先設定好各物件的名稱，比起甚麼Button1 我多會用btn_功能 來表示，然後我要的功能就直接 def 功能

在後面要連接的時候就不會有變數名稱重複跟搞混的問題 至於其他更進階的使用，我這次還沒用到 做完就儲存成.ui檔 然後在指令中輸入 pyuic .ui

-o.py 這樣就獲得了可以用的Class啦



* * *



然後這個做出來的py就不要去動它了，以便以後的更新



這時候開一個新的py檔



from import Ui_MainWindow



class MainWindow(QtWidgets.QMainWindow, Ui_MainWindow):



def __init__(self, parent=None):



super(MainWindow, self).__init__(parent=parent)



self.setupUi(self)



if __name__ == "__main__":



mp.freeze_support()



app = QtWidgets.QApplication(sys.argv)



w = MainWindow()



w.show()



sys.exit(app.exec_())



這樣就可以看到介面啦



這個時候就在class裡面做出想要的的功能就好了



* * *



### 進度條/狀態列



接下來，如果執行全都是在同一個thread，你在操作任何動作的中間，主畫面都會沒有回應



於是有幾個作法



QtTimer, Qthread.



QtTimer就是定時執行/更新



self.timer_camera = QtCore.QTimer()



self.timer_camera.timeout.connect(self.show_image)



self.timer_camera.start(int(1000 / 15))



這個的意思就是我在每 1000/15 毫秒做一次更新 *因為我的相機FPS是15 所以這樣設定



然後做的動作就是執行 self.show_image這個函數



QThread



這個比較複雜了 因為比起原本直接用 threading QtThread可以更好的銜接動作與指令



from PyQt5.QtCore import QThread, pyqtSignal



class msgThread(QThread):



msg = pyqtSignal(str)



def __int__(self, parent=None):



super(msgThread, self).__init__(parent)



def run(self):



while True:



#print('runing')



self.msg.emit(a.msg)



這是子程序端，用pyqtSignal傳輸 run主要動作



在主程式裡面則是要這樣處理 設定好thread,然後回傳完成度，主要我就是為了得到進度條所做的一切努力



def geotag_thread(self):



if not RUNNING:



self.t_geotag = GeotagThread()



self.t_geotag.start()



self.t_geotag.update_progressbar.connect(self.update_progressbar)



self.t_geotag.msg.connect(self.message_reciever)



else:



self.msg.setText('still geotagging')



self.msg.exec_()



def update_progressbar(self, val):



self.progressBar.setValue(val)



def message_reciever(self, msg):



if 'Error' in msg or 'fertig' in msg:



self.textBrowser.append(msg)



self.statusbar.showMessage(msg)



但是在用mp.Pool的時候呢? 這個我在stackoverflow上面也沒有找到答案



在pool這邊用 imap



for result in pool.imap_unordered(color_to_jpg, x):



count += 1



q.put(count * 100 / len(x))



然後我在Qthread裡面又開了一個Thread



q = Queue()



t1 = threading.Thread(target=generate_jpg, args=(self.prj_dir,q))



t1.start()



while RUNNING:



x = q.get()



self.update_progressbar.emit(x)



於是這樣得到了POOL時候的真正進度條



* * *



至於進階的美化等動作，就不是我的專長啦



大概QT做到現在就是



進度條、狀態列的更新、影像顯示



