## Ljk's ops utilities

This project is a collection of **scripts(shell or python)** and **python code snippet** which I wrote in the past. I will try to make these code reuse as easy as possible and I hope this can help others.


#### 1. 对数值型列表的追加过程加入对最大值(index:-1)/最小值(index:-2)的维护， 对于需要频繁调用max()/min()方法的场景可大幅提高性能 
[python.functions.append_list_preserve_extremum()](https://github.com/jkklee/Ljk-ops-utilities/blob/master/python/functions.py#L23)

```
>>> a=[0,1]
>>> append_list_preserve_extremum(a,-1)
>>> a
[0, -1, 1]
>>> append_list_preserve_extremum(a,8)
>>> a
[0, 1, -1, 8]
>>> append_list_preserve_extremum(a,-2)
>>> a
[0, 1, -1, -2, 8]
```
#### 2. 对以B为单位的数值返回更可读的size单位（ls命令的-h参数做的事情）  
[python.functions.get_human_size()](https://github.com/jkklee/Ljk-ops-utilities/blob/master/python/functions.py#L43)

```
>>> get_human_size(100)
'100.00 B'
>>> get_human_size(3000)
'2.93 KB'
>>> get_human_size(20480)
'20.00 KB'
>>> get_human_size(4096000)
'3.91 MB'
```
#### 3. 带唯一性检测的queue对象
[python.unique_queue.py](https://github.com/jkklee/Ljk-ops-utilities/blob/master/python/unique_queue.py)

```
>>> from uniquequeue import UniqueQueue
>>> uq=UniqueQueue()
>>> uq.put(1,unique=True) #带上unique参数
>>> uq.put(1,unique=True)
#可观察到put两次相同值，只能取1次
>>> uq.get_nowait()
1
>>> uq.get_nowait()
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "/usr/local/python37/lib/python3.7/queue.py", line 198, in get_nowait
        return self.get(block=False)
    File "/usr/local/python37/lib/python3.7/queue.py", line 167, in get
        raise Empty
    _queue.Empty
    
>>> uq.put_unique(2) #便利方法
>>> uq.put_unique(2)
>>> uq.get_nowait()
2
>>> uq.get_nowait()
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "/usr/local/python37/lib/python3.7/queue.py", line 198, in get_nowait
        return self.get(block=False)
    File "/usr/local/python37/lib/python3.7/queue.py", line 167, in get
        raise Empty
    _queue.Empty
>>> 
```
#### 4. 获取列表的中位数  
[python.functions.get_median()](https://github.com/jkklee/Ljk-ops-utilities/blob/master/python/functions.py#L1)
```
>>> get_median([1,2,3,4,5,6])
3.5
>>> get_median([1,2,3,4,5,6,7])
4.0
```
#### 5. 获取列表的4分位数(参考盒须图思想,用于体现响应时间和响应大小的分布)，以及min和max值  
[python.functions.get_quartile()](https://github.com/jkklee/Ljk-ops-utilities/blob/master/python/functions.py#L7)
```
>>> get_quartile([1,2,3,4,5,6,7,8,9,10])
(1, 3.0, 5.5, 8.0, 10)
>>> get_quartile([1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20])
(1, 5.5, 10.5, 15.5, 20)
```
#### 6. 对uri和args进行抽象化,利于分类  
[python.functions.text_abstract()](https://github.com/jkklee/Ljk-ops-utilities/blob/master/python/functions.py#L58)

    默认规则:  
        - uri中若 两个`/`之间 或 `/`和`.`之间仅由正则`[0-9或-_]+`组成,则将其抽象为'\*'  
        - args中所有参数的值抽象为'\*'  
        - 函数返回包含两个元素的元组，分别为url抽象结果和参数部分抽象结果  
    
```
>>> text_abstract('/user/100/article/5')
('/user/*/article/*', '')
>>> text_abstract('/user/article/5?commentid=10')
('/user/article/*', 'commentid=*')
```
#### 7. 高效的日期时间转换思路，例如：将nginx日志中的时间格式转换成202001011010这种格式  
经简单测试，可见完成任务自定义函数的耗时仅为time模块的约1/7,为datetime模块的约1/4  
[python.functions.convert_time()](https://github.com/jkklee/Ljk-ops-utilities/blob/master/python/functions.py#L89)  
```
>>> convert_time('22/Jul/2018:14:02:07','time_local')
('20180722', '14', '02')
>>> convert_time('2019-02-28T02:04:04+08:00', 'time_iso8601')
('20190228', '02', '04')
```
#### 8. 监控服务器网卡流量，可同时监控多块网卡  
[python.netflow-statics.py](https://github.com/jkklee/Ljk-ops-utilities/blob/master/python/netflow-statics.py)   
```
[jk.li@ops ~]$ python3 /tmp/tt eth0 bond0
start monitoring,press "ctrl+c" to stop

------eth0 bandwidth(Mb/s)------   ------bond0 bandwidth(Mb/s)------  
IN: 0.89      OUT: 2.87            IN: 0.89      OUT: 2.87            
IN: 0.56      OUT: 2.64            IN: 0.56      OUT: 2.64            
IN: 0.54      OUT: 2.42            IN: 0.54      OUT: 2.42            
^C
-----bye-----
```
#### 9. 封装Linux echo命令，方便打印彩色字体   
[shell/color_echo.sh](https://github.com/jkklee/Ljk-ops-utilities/blob/master/shell/color_echo.sh)
     
![example](https://img-blog.csdnimg.cn/20200705092753329.png)   

 
