```
In [31]: import pickle


In [38]: city
Out[38]: {1: '西安', 2: '北京', 3: '长沙', 4: '南京'}

In [39]: pickle_file = open('city_data.pkl','wb')

In [40]: pickle.dump(city,pickle_file)

In [41]: ls city_data.pkl
 驱动器 C 中的卷是 Windows
 卷的序列号是 229D-4250

 C:\Users\racher 的目录

2018/02/05  12:58                 0 city_data.pkl
               1 个文件              0 字节
               0 个目录 74,154,651,648 可用字节

In [42]: pickle_file.close()
```

```
In [44]: pickle_file = open('city_data.pkl','rb')

In [45]: city = pickle.load(pickle_file)

In [46]: city
Out[46]: {1: '西安', 2: '北京', 3: '长沙', 4: '南京'}
In [47]: pickle_file.close()

```


