# Photo Finder

这是一个为[腾讯问卷](http://wj.qq.com)设计的**附件查找和重命名**的小工具。

我在使用腾讯问卷设计的某些调查表中，需要让用户上传照片，腾讯问卷直接提供了上传功能，但后台看到的附件都是随机生成的编码，尤其是将调查结果数据下载到本地后很难找到相对应的照片，因此写了这一个脚本来查找附件，并重命名为可识别的名字。

## 使用准备

1. 在腾讯问卷的统计后台导出原始数据，注意要选择 Excel。
2. 导出所有附件到本地，应该在一个文件夹中，放到跟 `csv` 相同的文件夹里。
3. 将脚本 `photo_finder.py` 放到跟 `csv` 相同的文件夹（假设你已经有了 Python 环境，2 或 3 应该都可以，我在本地是 2.6）。

## 关键逻辑

根据 `csv` 文件生成**附件文件名**和**可识别名字**对应关系，从参数指定列的索引来标记。代码如下：

```python
def load_photo_name_data(csv_file, photo_index = 0, name_index = 0):
    ''' Load wj csv data with specified photo(as key) and name(as value) index to populate data '''
    if not os.path.exists(csv_file):
        print("The csv file [{}] is not exists.".format(csv_file))
        return False

    photo_name_data = dict()

    with open(csv_file) as data_file:
        reader = csv.reader(data_file, delimiter = ',', quotechar = '"')
        reader.next() # skip the header

        for row in reader:
            k = populate_filename_from_hyperlink(row[photo_index])
            photo_name_data[k] = row[name_index]

    return photo_name_data
```

根据上一步的数据，遍历本地附件并重命名。

```python
def find_rename_photos(photos_dir, photo_name_data):
    ''' Find photos local and rename them with names data '''
    if not os.path.exists(photos_dir):
        print("The photos dir [{}] is not exists.".format(photos_dir))
        return False

    for k in photo_name_data:
        parts = k.split('.')

        photo_file = os.path.join(photos_dir, k)
        new_file = os.path.join(photos_dir, photo_name_data[k] + '.' + parts[1])

        print k, ' => ', new_file

        if os.path.exists(photo_file):
            os.rename(photo_file, new_file)
        else:
            print photo_file, ' is NOT exist so skipped. <=', photo_name_data[k]
```

详细代码请看脚本文件[photo_finder.py](photo_finder.py)。