json转plist 本地存读地名

做这个主要就是后端打包给了一份json格式的全球地域名,第一次做plist相关的操作,有几个收获总结下:
 1 关于json解析 (写了很多次了都没记住 这次再记录下来)
{% highlight html linenos %}
{% raw %}
- (NSArray *)readFromJson
{
    NSString *path = [[NSBundle mainBundle] pathForResource:@"china" ofType:@"json"];
    NSData *fileData = [NSData dataWithContentsOfFile:path];
    NSError *error;
    id JsonObject = [NSJSONSerialization JSONObjectWithData:fileData
                                                  options:NSJSONReadingAllowFragments
                                                    error:&error];
    return JsonObject;
}
{% endraw %}
{% endhighlight %}

2 文件的读取(还是没记住的.......)
  1,本地资源文件读取(即直接拖入程序的)
{% highlight html linenos %}
{% raw %}
  NSString *path = [[NSBundle mainBundle] pathForResource:@"china" ofType:@"json"];
  NSData *fileData = [NSData dataWithContentsOfFile:path];
 {% endraw %}
{% endhighlight %}
  2,document内文件读取
{% highlight html linenos %}
{% raw %}
  	 NSArray *paths=NSSearchPathForDirectoriesInDomains(NSDocumentDirectory,NSUserDomainMask,YES);
    NSString *path=[paths objectAtIndex:0];
    NSString *filename=[path stringByAppendingPathComponent:listName];
 {% endraw %}
{% endhighlight %}

3 关于建表的思想
	这次由于涉及到全球200多个国家的省和市,json文本的大小就达到3M多,一开始使用的是主键和外键形式的,但是想想每次取到和一个省相关的市就要遍历一次市表,这肯定是不行的,后来同事一句话点醒了我,何不使用索引,在plist中使用索引我做的就是把省名和id合起来做索引名,这样查询量就大大降低了
