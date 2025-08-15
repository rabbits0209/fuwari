---
title: 国庆爬个明日方舟立绘并搭建api
published: 2024-10-05
description: 这篇文章介绍了作者为大家准备了一份国庆礼物——一个爬虫程序，并感谢了一些大佬。作者在新年前要爬取一些数据，并希望能和大家一起进步。
tags: [爬虫, Worker]
category: 技术教程
draft: false
---

大家国庆快乐呀，写了个爬虫送给大家

---

先感谢着几位大佬🍭
[新年前爬个明日方舟的立绘](https://noionion.top/53760.html)
[明日方舟干员立绘爬虫](https://www.heart-of-engine.top/posts/fccf.html)
[CloudFlare Worker搭建随机图片API](https://blog.sukiu.top/Mixed/Random-Image-Worker/)
无意间刷到了[大佬](https://noionion.top/)的博客，看到了这个明日方舟的立绘挺有意思的，想直接抄作业，但奈何代码失效了，就自己改了一下，发出来记录一下

---

看了一下，应该是后面爬链接的那一块失效了，原文是这样的"**给的是 /images/thumb/6/65/%E7%AB%8B%E7%BB%98_%E5%87%AF%E5%B0%94%E5%B8%8C_2.png ，我要的是这个 http://prts.wiki/images/6/65/%E7%AB%8B%E7%BB%98_%E5%87%AF%E5%B0%94%E5%B8%8C_2.png 嘛！**”
现在应该是更新了，给的链接是**https://prts.wiki/w/%E6%96%87%E4%BB%B6:%E7%AB%8B%E7%BB%98_12F_1.png**，但需要的是**https://media.prts.wiki/6/61/%E7%AB%8B%E7%BB%98_12F_1.png**

现在完全牛头不对马嘴嘛🌚

---

所以研究了一下之后，感觉应该可以
我就直接上代码了

```Python
# 引入程序需要的相应包（类似C语言中的# include<stdio.h> 、include<math.h>等）
import os
from bs4 import BeautifulSoup
import time
import requests

# 设置爬取图片的网址
url = "http://prts.wiki/index.php?title=%E7%89%B9%E6%AE%8A:%E6%90%9C%E7%B4%A2&limit=500&offset=0&profile=images&search=%E7%AB%8B%E7%BB%98"


# 重定义请求头，防止被网页发现是爬虫，从而进行反爬操作
headers = {
    "Cookie": "arccount62298=c; arccount62019=c",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36 Edg/87.0.664.66"
}


# 爬取模块
html = requests.get(url, headers=headers) ##  这里是使用requests函数用之前重定义的请求头读取网页信息
### 测试代码
### print(html.text)
html.encoding = html.apparent_encoding ## encoding是从http中的header中的charset字段中提取的编码方式，若header中没有charset字段则默认为ISO-8859-1编码模式，无法解析中文，这是乱码的原因,apparent_encoding会从网页的内容中分析网页编码的方式，所以apparent_encoding比encoding更加准确，当网页出现乱码时可以把apparent_encoding的编码格式赋值给encoding。
soup = BeautifulSoup(html.text, "html.parser") ## "html.parser"是为了让解析速度快，容错率高，具体讲解网址为https://blog.csdn.net/yangjiajia123456/article/details/80959896
list = soup.find_all(class_ = "searchResultImage") ## 注一：对爬取页面进行“检查”操作后发现，所有的图片都包含在一个<table class="searchResultImage">的class中，所以这行代码的作用是查找html代码中标签为class且值为searchRusultImgge的所有代码块
### 测试代码
### print(list)
### 可以看到，相比于html.text，list里没有大量的网页配置信息，只有以图片名称为核心的列表


# 检查项目文件夹下是否有“Arknights”这个文件夹，没有的话新建一个，有的话就不新建了
try:
    os.mkdir("./Arknights")  ##  注二：创建文件夹（这个文件夹创建在现在的项目文件夹里，比方说我这里用的是“明日方舟干员立绘项目”项目文件夹，爬取到的图片就存在“明日方舟干员立绘项目”项目文件夹下的“Arknights”文件夹里，该函数具体介绍网址为https://blog.csdn.net/qq_20412595/article/details/82423764）
    ## 项目文件夹默认在代码存放的地方，如果你放在桌面，那后面生成的Arknights文件夹就在桌面
except:
    pass

# 将当前工作路径切换为“Arknights”这个文件夹
os.chdir("./Arknights") ##  切换当前工作路径（本函数的具体介绍在https://www.runoob.com/python3/python3-os-chdir.html）

# 将最初读取的图片设置为第一张图片
num=0

# 测试用参数
# num1 = 0
# num2 = 0

# 图片的爬取与本地存储
for s in list:
    string = str(s) ## 将list中的单个元素化为字符串，再针对该字符串进行处理

    namebegin = string.find('title="文件') ##  注三：显然，所有立绘的文件名开头都有“文件”二字，找到“文件”二字就相当于找到了立绘 注四：为了读取图片里包含的干员名，我们发现能够通过title入手，向后搜索名称，这里找到了"title"中"t”的位置
    # name_point_1 = namebegin+12 ## 为了读取干员名称，我们这里找到第一个空格的位置name_point_1（这里空格刚好在namebegin后的第二格）
    # name_point_2 = string.find(' 1'or' 2'or' skin') ## 这里找到第二个空格的位置name_point_2（本方法返回值远小于namebegin的数值，是错误的）
    nameend = string[namebegin:].find('png') ## 找到图片文件名末尾的"png"字样对应的位置

    ## 测试代码
    # print(namebegin)
    # print(name_point_1)
    # print(name_point_2)
    # print(nameend)

    ## 这里如果使用name_point_2 = string.find(' 1'or' 2'or' skin')，会显示位置为54，远小于namebegin的位置值，是错误的，所以用这种方法难以找到第二个空格的位置，所以我们将"干员名 1/2/skin(b/_V).png"整体令为name1，再在name1里寻找空格位置
    name1 = string[namebegin+13:namebegin+nameend-1]
    name1begin = name1.find('"干员名"')
    name1_point_1 = name1begin
    name1_point_2 = name1.find(' 1'or' 2'or' skin') ## 根据测试代码我们有，输出结果为-1，即name1[name1begin]对应于name1的最后一个字符

    # 测试代码
    # print(name1) # 输出结果格式"暗索 skin1 V1"
    # print(name1begin)
    # print(name1[name1begin])
    # print(name1_point_1)
    # print(name1_point_2)

    # print(string[(namebegin+nameend-3):(namebegin+nameend-1)]) ## 本代码用来查看后两位并进行检验

    if(string[namebegin+nameend-3:namebegin+nameend-1] in ['V1','V2']): ## 若后两位为V1或V2
        # num1+=1
        # print('num1={}'.format(num1))
        continue
    elif(string[namebegin+nameend-2:namebegin+nameend-1]=='b'): ## 若最后一位为b
        # num2+=1
        # print('num2={}'.format(num2))
        continue
    else:
        name1 = name1 + '.png'

    # 测试代码
    # print(name1)

    # 修改文件名称
    name1 = name1.replace(" ","_") ## 将图片文件名里的空格转为”_“

    # 设置显示的链接名
    urlbegin = string.find('https://media.prts.wiki/thumb/')
    if urlbegin != -1:
        urlend = string.find('.png', urlbegin)
        imgurl_suffix = string[urlbegin+30:urlend+4]  # 获取https://media.prts.wiki/thumb/后面的内容
        imgurl = 'https://media.prts.wiki/' + imgurl_suffix
    # 爬取图片
    img = requests.get(imgurl, headers=headers).content

    # 测试代码
    # print(name1)

    # 图片写入本地文件夹
    if(name1 not in ['精二立绘A.png','Pith_2.png','Sharp_2.png','Stormeye_2.png','Touch_2.png','阿米娅(近卫)_2.png']): ## 为了不读取”精二立绘A“与五位特殊干员（前面博文有说到）的图片，专门做了一个循环（其实爬取后再直接寻找删除也可以）
    ## 经过noionion大佬的提示，or的优先级不高，所以不能用if(name1 != '精二立绘A.png' or 'Pith_2.png' or 'Sharp_2.png' or 'Stormeye_2.png' or 'Touch_2.png' or '阿米娅(近卫)_2.png'):写法
    ## 他还建议我对列表进行硬操作，而不是用!=这种逻辑运算符（说可能会炸，我慌的一批）
    ## 所以我去除了if(name1 != ('精二立绘A.png' or 'Pith_2.png' or 'Sharp_2.png' or 'Stormeye_2.png' or 'Touch_2.png' or '阿米娅(近卫)_2.png'))，改用全新的判断语法
        with open(name1, 'wb') as f:
            f.write(img) ## 将爬取到的图片写入“Arknights”这个文件夹里
            num+=1 ## num=num+1，使得下面print函数里的”已爬取{}张“中的{}对应数值加1
            print("已爬取{}张,图片名称为：{}，链接为：{}".format(num,name1,imgurl)) ## 这里{}的意义与C语言中的"%d"/"%f"十分相近，是从format()函数里读取对应位置的参数
        # "r"--以读方式打开,只能读文件,如果文件不存在,会发生异常
        # "w"--以写方式打开,只能写文件,如果文件不存在,创建该文件;如果文件已存在,先清空,再打开文件
        # "rb"--以二进制读方式打开,只能读文件,如果文件不存在,会发生异常
        # "wb"--以二进制写方式打开,只能写文件,如果文件不存在,创建该文件;如果文件已存在,先清空,再打开文件（所以这个程序就算运行多次，文件夹里也不会出现重复的立绘）
        time.sleep(1) ## 休息一秒后爬取下一张图片，这个函数在链接https://blog.csdn.net/weixin_45949073/article/details/104989562里讲得很清楚
```
2025又look了一下，发现失效了，又重新写了一个
```Python
import os
import requests
from bs4 import BeautifulSoup
from concurrent.futures import ThreadPoolExecutor
from urllib.parse import unquote

# 设置爬取图片的网址
url = "https://prts.wiki/index.php?title=%E7%89%B9%E6%AE%8A:%E6%90%9C%E7%B4%A2&limit=500&offset=0&profile=images&search=%E7%AB%8B%E7%BB%98"

# 重定义请求头，防止被网页发现是爬虫
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36 Edg/87.0.664.66",
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8",
    "Accept-Language": "zh-CN,zh;q=0.9",
    "Accept-Encoding": "gzip, deflate, br",
    "Connection": "keep-alive",
    "Upgrade-Insecure-Requests": "1"
}

def fetch_html(url, headers):
    try:
        response = requests.get(url, headers=headers)
        response.raise_for_status()
        response.encoding = "utf-8"
        return response.text
    except requests.RequestException as e:
        print(f"请求网页时发生错误：{e}")
        exit()

def parse_image_links(html):
    soup = BeautifulSoup(html, "html.parser")
    search_results = soup.find("div", class_="searchresults")
    image_links = []
    for result in search_results.find_all("li", class_="mw-search-result"):
        table = result.find("table", class_="searchResultImage")
        if table:
            td = table.find("td", style="vertical-align: top")
            if td:
                link = td.find("a")
                if link and link.get("href"):
                    image_page_url = "https://prts.wiki" + link.get("href")
                    image_links.append(image_page_url)
    return image_links

def download_image(image_page_url, headers):
    try:
        image_page_response = requests.get(image_page_url, headers=headers)
        image_page_response.raise_for_status()
        image_page_response.encoding = "utf-8"
        
        image_page_soup = BeautifulSoup(image_page_response.text, "html.parser")
        full_image_link = image_page_soup.find("div", class_="fullImageLink", id="file")
        if full_image_link:
            image_link = full_image_link.find("a")
            if image_link and image_link.get("href"):
                image_download_url = unquote(image_link.get("href"), encoding='utf-8').replace("+", "%2B")
                image_name = image_download_url.split("/")[-1]
                
                image_download_response = requests.get(image_download_url, headers=headers)
                image_download_response.raise_for_status()
                with open(image_name, "wb") as f:
                    f.write(image_download_response.content)
                print(f"已成功下载图片：{image_name}")
    except requests.RequestException as e:
        print(f"下载图片失败：{image_page_url}，错误原因：{e}")

def main():
    html = fetch_html(url, headers)
    image_links = parse_image_links(html)
    
    if not os.path.exists("./Arknights"):
        os.mkdir("./Arknights")
    os.chdir("./Arknights")
    
    with ThreadPoolExecutor(max_workers=10) as executor:
        executor.map(lambda link: download_image(link, headers), image_links)

if __name__ == "__main__":
    main()
```




~~这里参照了[https://www.heart-of-engine.top/posts/fccf.html](https://www.heart-of-engine.top/posts/fccf.html)的代码，小小改动了一点，成功运行起来了🌝~~2025的就是完全自己写的了😅

---

后面由于想做成随机图片api所以就用CloudFlare Worker搭建了
直接上教程了

---

图片转 Webp
这步骤可选，转换为 webp 格式后，图片体积压缩而分辨率不变，可以更快地加载。

为了在引用时方便，先把名称按 1、2、3…顺序标号，再通过 PIL 库，将 jpg 和 png 格式的图片转换成 webp。

```Python
import os
from PIL import Image

def rename_and_convert_images(directory):
    # 获取目录中的所有文件
    files = os.listdir(directory)
    # 过滤出 png 和 webp 格式的文件
    png_files = [f for f in files if f.lower().endswith('.png')]
    webp_files = [f for f in files if f.lower().endswith('.webp')]
  
    # 找到现有的最大序号
    max_index = 0
    for f in webp_files:
        try:
            index = int(os.path.splitext(f)[0])
            if index > max_index:
                max_index = index
        except ValueError:
            continue
  
    # 按顺序重命名文件并转换格式
    for index, filename in enumerate(png_files, start=max_index + 1):
        # 构建新的文件名
        new_filename = f"{index}.webp"
        # 构建完整的文件路径
        old_filepath = os.path.join(directory, filename)
        new_filepath = os.path.join(directory, new_filename)
      
        # 打开图像并转换为 webp 格式
        with Image.open(old_filepath) as img:
            img.save(new_filepath, 'webp')
      
        # 删除原始文件
        os.remove(old_filepath)
        print(f"Converted {filename} to {new_filename}")

# 使用示例
directory = './Arknights'  # 替换为你的目录路径
rename_and_convert_images(directory)
```

---

文件上传 Github
这步骤不具体说了，图片通过 *jsdeliver* 引用。

---

CloudFlare Worker
新建一个 worker，代码参考如下：

+因为改了文件名，所以下面只需要修改图片总数 total，就能生成一个随机数访问
+个人通过 type 区别宽屏和竖屏图片，自己按需修改

```JS
addEventListener('fetch', event => {
    event.respondWith(
        handleRequest(event.request).catch((err) =>
            new Response('cfworker error:\n' + err.stack, {
                status: 502,
            })
        )
    );
});

async function handleRequest(request) {
    const url = new URL(request.url);
    switch (url.pathname) {
        case "/img":
            var type = url.searchParams.has("type") ? url.searchParams.get("type") : "pc";
            const total = getTotal(type);
            if (total == 0) return handleImage("pc", getTotal("pc"));
            return handleImage(type, total);
        case "/favicon.ico":
            return fetch("https://gcore.jsdelivr.net/gh/SukiEva/assets/blog/favicon.ico");
        default:
            return handleNotFound();
    }
}

function getTotal(type) {
    switch (type) {
        case "pc": return 175;
        case "mb": return 0;
        default: return 0;
    }
}

async function handleImage(type, total) {
    var index = Math.floor((Math.random() * total)) + 1;
    var img = "https://gcore.jsdelivr.net/gh/SukiEva/assets/webp/" + type + "/" + index + ".webp";
    res = await fetch(img);
    return new Response(res.body, {
        headers: {
            'content-type': 'image/webp',
        },
    });
}

function handleNotFound() {
    return new Response('Not Found', {
        status: 404,
    });
}
```

---

自定义域名
默认是一个 CF 的域名，但是目前已经被国内 ban 了，需要自行添加域名路由。

最后再次祝看到这篇博客的小伙伴：

## 2024，国庆快乐鸭!

在小小的洞里爬呀爬呀爬（ 逃
