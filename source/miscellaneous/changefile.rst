修改后缀
1
for file in *.原后缀; do mv "$file" "`echo $file | sed s/.原后缀/.新后缀/`"; done
Copy
格式转换
heic转jpg
首先，安装转换工具。

1
yay -S libheif
Copy
然后可以使用heif-convert命令转换。

1
heif-convert input.heic output.jpg
Copy
批量转换，命令如下。

1
for file in *.heic; do heif-convert "$file" ${file/%.heic/.jpg} && rm "$file"; done
Copy
此命令的原理是根据当前文件夹下的.heic文件生成.jpg文件，如果成功生成则删除原.heic文件，如果未成功则不会删除原文件。 若未能生成.jpg文件的原因是Input file 'filename.heic' is a JPEG image则可以使用批量修改后缀的方法直接将文件的后缀改为.jpg。
