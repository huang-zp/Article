>** 使用Python Xlwt简单操作Execl文件， 写入 ，合并单元格， 增加简单样式**

## 简单写入

	import xlwt
	wbk = xlwt.Workbook()
	sheet = wbk.add_sheet('firstsheet')
	sheet.write(0, 0, label='value1')
	sheet.write(0, 1, label='value2')
	sheet.write(0, 2, label='value3')
	wbk.save('firstexecl.xls')
	

![](http://oumkbl9du.bkt.clouddn.com/2018-02-02-GDbNl-2018-02-02_15-48-00.png)
## 合并单元格write_merge
增加代码

	sheet.write_merge(1,2,1,2,label='merge')

![](http://oumkbl9du.bkt.clouddn.com/2018-02-02-oacKd-2018-02-02_15-51-13.png)

**参数解释：**
write_merge(a, b, c, d, label)
**a:行
b:行+跨行数
c:列
d:列+跨列数
label:值**

write_merge(1,2,1,2,label='merge')意思是，行坐标为1,然后跨行数为2-1,也就是再跨一行。列坐标为1,然后跨列数为2-1,也就是再跨一列。

## 简单样式

	style = xlwt.easyxf('font: color-index red, bold on')
	sheet.write(0, 0, label='value1', style=style)

![](http://oumkbl9du.bkt.clouddn.com/2018-02-02-X57kg-2018-02-02_16-03-19.png)

## 简单计算

	sheet.write(1, 0, label=1)
	sheet.write(2, 0, label=2)
	sheet.write(3, 0, xlwt.Formula("A2+A3"))
![](http://oumkbl9du.bkt.clouddn.com/2018-02-02-5XZT1-2018-02-02_16-07-54.png)

## 代码

	import xlwt
	wbk = xlwt.Workbook()
	sheet = wbk.add_sheet('firstsheet')
	style = xlwt.easyxf('font: color-index red, bold on')
	sheet.write(0, 0, label='value1', style=style)
	sheet.write(0, 1, label='value2')
	sheet.write(0, 2, label='value3')
	sheet.write(1, 0, label=1)
	sheet.write(2, 0, label=2)
	sheet.write(3, 0, xlwt.Formula("A2+A3"))
	sheet.write_merge(1,2,1,2,label='merge')
	wbk.save('firstexecl.xls')


