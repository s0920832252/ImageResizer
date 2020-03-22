# ImageResizer

[紀錄]
(4182-28)/4182 = 0.99330463892

直覺想法是 ResizeImages 內的動作鐵定超過 50 ms
所以起手式就包個 Task.Run ...
本來想說 再看看還有甚麼可以加加看....
然後發現加了反而會變慢= =.

例如 :
1. processBitmap 改成 async 方法.
其方法內的處理用 Task.Run 包起來
2. IO 處理 改成 Task 處理
string destFile = Path.Combine(destPath, imgName + ".jpg");
processedImage.Save(destFile, ImageFormat.Jpeg);
用 Task.Run 包起來

然後再用 await 試試看.

會變慢的理由是否是因為 , 這兩個就算額外開一個 Thread 給它們執行 ,
也只是讓原本起手式的 Thread 在那邊等待而已.
反而只是造成 Content Switch 的白白損耗 ...???
