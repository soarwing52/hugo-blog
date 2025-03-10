---

title: "Raspberry Pi + Realsense: Usage of class, PyQT"

date: 2019-11-01

description: "Raspberry Pi + Realsense: Usage of class, PyQT"

---



So not only did this project helped me know more about tcp transmission and

also helped my old scripts I based on the structure and with minimum change I

made a pyQT version of the camera app



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/65228-pyqtcam.jpg)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/ef385-pyqtcam.jpg)



Remember the structure



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/5c920-flask.jpg)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/7f9f9-flask.jpg)



All I need to do is to change Flask into pyQT and do the same start decision

loop function The buttons are the same, and with more information can be pass

to the GUI easily The current issue is only the import

<https://github.com/IntelRealSense/librealsense/issues/4856>

<https://github.com/IntelRealSense/librealsense/issues/4959> Just import

pyrealsense2 in threads or processes it will work just fine. The result is

here <https://github.com/soarwing52/RealsensePython/tree/master/qtcam> This I

used with Qtdesigner, and my class so this is a project with 3 py files. the

temp for the ui created. cmd class for the class loop and the qtcam is the

functions

\-------------------------------------------------------------------------------------------------------------------

Qt designer



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/28f3e-qtdesigner.jpg)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/5e378-qtdesigner.jpg)



with this can save .ui file and use pyuic ui.ui -o ui.py it will create the py

file and the smoothest way I found is



    

    

    from temp import Ui_MainWindow

    

    

    class MainWindow(QtWidgets.QMainWindow, Ui_MainWindow):

        def __init__(self, parent=None):

            super(MainWindow, self).__init__(parent=parent)

            self.setupUi(self)

    

    

    if __name__ == "__main__":

        app = QtWidgets.QApplication(sys.argv)

        w = MainWindow()

        w.show()

        sys.exit(app.exec_())



This can use the ui.py while updating with the least effort

\------------------------------------------------------------------------------------------------------------------------

This modification made my further works more smooth while editting the

functions but also takes more thinking for debugging



