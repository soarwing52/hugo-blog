---

title: "RealSense 學習筆記/教學/分享（四）：計算兩點實際距離"

date: 2019-05-08

description: "RealSense 學習筆記/教學/分享（四）：計算兩點實際距離"

---



好的，錄製了深度跟色彩兩幅畫面並且把他們配對好之後呢，接下來就是主要任務了



要計算兩點的距離



如果說要介面，Python已經有十分多的選擇可以選



我最後選了matplotlib來作為工具



第一版本我用了opencv



設定了左鍵按下，按住然後放開



        def measure (event,x,y,flags,param):  

            global xi,yi,x0,y0  

            if event == cv2.EVENT_LBUTTONDOWN:  

                xi,yi = x,y  

                print 'xi,yi= ' + str(xi) + ',' + str(yi)  

                return xi,yi



            elif event == cv2.EVENT_LBUTTONUP:  

                x0,y0 = x,y  

                print 'x0,y0= ' + str(x0) + ',' + str(y0)  

                calculate_3D(xi, yi, x0, y0)  

                return x0,y0







定義了起點跟終點之後 就是計算距離







運用到depth_frame, color intrinsics兩個參數







一個是取到距離 另外一個是用來轉換pixel座標到3D空間座標用的



其實數學部分很簡單，就是根號x y z的平方



不過我在找計算面積，用向量或是其他解，發現好像目前辦不到，太難了



        def calculate_3D (xi, yi, x0, y0):



        udist = depth_frame.get_distance(xi, yi)



        vdist = depth_frame.get_distance(x0, y0)



        print udist, vdist



        point1 = rs.rs2_deproject_pixel_to_point(color_intrin, [xi, yi], udist)



        point2 = rs.rs2_deproject_pixel_to_point(color_intrin, [x0, y0], vdist)



        print 'start(x,y,z): '+ str(point1)+'\n' + 'end(x,y,z): ' +str(point2)



        dist = math.sqrt(



        math.pow(point1[0] - point2[0], 2) + math.pow(point1[1] - point2[1],2) + math.pow(



            point1[2] - point2[2], 2))



            cm = dist * 100



        decimal2 = "%.2f" % cm



        print 'Vermessung: ' + str(decimal2)+ 'cm'



        MessageBox(text='Vermessung: ' + decimal2 + ' cm')















然後得到的距離顯示演進像是以下



我最一開始的簡略版



用最弱的print



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/0ca42-2nd2bshell.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/64d96-2nd2bshell.png)



看不到選到的地方，然後就是跳出一個視窗說多遠



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/255f9-message2bbox.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/693e2-message2bbox.png)



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/a70e9-2nd2bresult.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/a70e9-2nd2bresult.png)



用這個顯示出視窗



import ctypes  

from ctypes import c_int, WINFUNCTYPE, windll  

from ctypes.wintypes import HWND, LPCWSTR, UINT  

prototype = WINFUNCTYPE(c_int, HWND, LPCWSTR, LPCWSTR, UINT)  

paramflags = (1, "hwnd", 0), (1, "text", "Hi"), (1, "caption", "Result"), (1,

"flags", 0)  

MessageBox = prototype(("MessageBoxW", windll.user32), paramflags)







但是這真的是太簡陋了，剛好在這個[討論串](https://github.com/matplotlib/matplotlib/issues/7216)有人寫了Ruler工具







於是我就改用matplotlib然後改寫了他的工具作為3D測量用







(其實我在做這裡的時候還不會OOP(Object-oriented programming) 所以弄一弄竟然能用了XD







改寫的有



兩點座標的輸出  

@property



    def ruler_length(self):



        line_pp = self.ruler.get_path().vertices



        x0 = line_pp[0][0]



        y0 = line_pp[0][1]



        x1 = line_pp[1][0]



        y1 = line_pp[1][1]



        xa,xb,ya,yb = int(x0),int(x1),int(y0), int(y1)



        depth_frame = self.depth_frame



        color_intrin = self.color_intrin



        ant = self.calculate_3D(xa,xb,ya,yb,depth_frame,color_intrin)



        return ant











定義3D距離



def calculate_3D(self, xa,xb,ya,yb,depth_frame,color_intrin):



       udist = depth_frame.get_distance(xa, ya)



        vdist = depth_frame.get_distance(xb, yb)



        point1 = rs.rs2_deproject_pixel_to_point(color_intrin, [xa, ya], udist)



        point2 = rs.rs2_deproject_pixel_to_point(color_intrin, [xb, yb], vdist)



        dist = math.sqrt(



            math.pow(point1[0] - point2[0], 2) + math.pow(point1[1] - point2[1], 2) + math.pow(



                point 1[2] - point 2[2], 2))



        cm = dist * 100



        decimal 2 = "%.2f" % cm



        return decimal 2



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/af14b-figure_1.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/18530-figure_1.png)







到這一步，大致是把在範例裡面用C++寫的工具轉到python裡面成功使用了







<https://github.com/IntelRealSense/librealsense/tree/master/examples/measure>







只能說 滿不容易的







到這邊大約三個禮拜過去了







下一個階段就是在測量的精準度上提升了







會針對上一篇提到的不相應幀的修正







其實現在回頭看，這裡就比較偏向GUI的部分了，要做出好的圖形介面給其他人使用







從最初只能從shell裡面看到結果 到視窗 到最後的借用別人的工具







\-------------------------------------------------------------------------------------------------------------



在這段之後，接著我把這個嫁接到ArcMap上面，用hyperlink可以打開







hyperlink 可以用 Script 打開



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/57c90-hyper.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/a123c-hyper.png)







所以看起來就萬事太平了







結果因為realsense本身的程式碼還有缺陷，在我需要的設定下會凍結



pipeline.stop()執行後程式就死了







原本用import直接打開應該是最順的，結果一天到晚arcgis沒有回應，不行



後來嘗試了multithread/multiprocess 結果32位元的ArcGIS並不支援，難怪現在要做arcGISpro



首先先有路的線



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/e30f2-25e5259c259625e7258925878.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/163a8-25e5259c259625e7258925878.png)



點開就會跳出matplotlib視窗



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/32605-25e5259c259625e7258925879.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/6c5c9-25e5259c259625e7258925879.png)



 按左鍵即可測量



[![](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/001d0-25e5259c259625e72589258710.png)](https://jaythecheyi.home.blog/wp-

content/uploads/2019/11/0a310-25e5259c259625e72589258710.png)



後來用了繞很遠的方法：



subprocess.call("cmd")



叫出了指令，然後在script裡面下指令 把道路的ID跟檔案位置丟進去



CMD打開python measure.py -w 道路編號 --path 路徑 --num 照片編號



最後就可以避免沒有回應 可喜可賀 可喜可賀







但是還是有小缺點，因為arcmap沒有辦法多線程操作，所以在打開這個CMD視窗的時候，arcmap本身是凍結狀態的，關掉視窗才會回復可操作的狀態，不過目前這樣已經很滿意了



















