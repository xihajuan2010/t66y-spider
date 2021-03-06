# t66y-spider
A crawler for images, torrents and videos on [t66y.com](t66y.com)

草榴社区爬虫，主要收集達蓋爾的旗幟内的图片，也可用于其他板块内的种子，视频或者图片。

# 用法

    git clone https://github.com/Nymphet/t66y-spider.git
    
    cd t66ySpider/t66ySpider
    
    # 爬達蓋爾的旗幟的帖子，保存帖子标题和图片地址
    
    scrapy crawl DaGaiEr
    
    # 爬新時代的我們的帖子，保存帖子标题和图片地址
    
    scrapy crawl XinShiDai
    
    # 收集亚洲无码原创区帖子内的图片地址和种子链接，也保存帖子标题和帖子url
    
    scrapy crawl YaZhouWuMa
    
    # 亚洲无码转帖交流区，保存图片地址，种子链接，帖子标题，帖子url
    
    scrapy crawl YaZhouWuMaZhuanTie
    
    # 亚洲有码原创区，同上
    
    scrapy crawl YaZhouYouMa
    
    # 亚洲有码转帖交流区，同上
    
    scrapy crawl YaZhouYouMaZhuanTie
    
    # 國產原創区，同上
    
    scrapy crawl GuoChanYuanChuang
    
    # 中字原創
    
    ZhongZiYuanChuang
    
    # 歐美原創
    
    scrapy crawl OuMeiYuanChuang
    
    # 轉帖交流區
    
    scrapy crawl ZhuanTieJiaoLiu
    
scrapy 支持搭配 shadowsocks, tor 等等各种 socks 代理和各种 http 代理，如需使用代理，可在配置文件中设置, 
也可以直接用 proxychains-ng 一类工具。例如使用 proxychians-ng 时，在每个命令前加上 proxychians4 即可

    ./scripts/download.py GuoChanYuanChuang 1 0

# 数据格式

默认格式为 jsonline，python 可以用 ast.literal_eval 读取。各爬虫抓的数据不同，可以查看 items.py 内的定义。目前主要有：

    DaGaiEr     :
    XinShiDai   : 't_title'         : string, 标题
                  't_image_list'    : list  , 大图源地址
                  
    YaZhouWuMa          : 
    YaZhouYouMa         : 
    GuoChanYuanChuang   :
    OuMeiYuanChuang     :
    ZhongZiYuanChuang   :
    YaZhouWuMaZhuanTie  :
    YaZhouYouMaZhuanTie :
    ZhuanTieJiaoLiu:
                  't_title'         : string, 标题
                  't_url'           : string, url
                  't_image_list'    : list  , 大图源地址（会自动把 imgchili 和 imagetwist 的缩略图转为原图）
                  't_torrent_list'  : list  , 种子地址
    
 
# 其它

草榴社区的种子都是传到 rmdown 的，这个爬虫只抽取种子地址，不自动下载种子。如需自动下载，可调用
[rmdown.pl](https://github.com/eccstartup/caoliu-synchronizer/blob/master/rmdown.pl)

图片自动下载默认没有开启，如需开启，在所有文件中把所有 `t_image_list` 替换为 `image_urls`, 
在 settings.py 中加入两行
    
    ITEM_PIPELINES = {'scrapy.pipelines.images.ImagesPipeline': 1} 
    IMAGES_STORE = '/存储目录'
