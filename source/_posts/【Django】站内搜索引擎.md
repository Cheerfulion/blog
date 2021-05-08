---
title: 【Django】站内搜索引擎
date: 2021-05-06 19:28:02
tags:
    - django
---



## 前言

站内搜索是网站常用的功能之一，其作用是方便用户快速查找站内数据以便查阅。对于一些个人小型网站来说，站内搜索可以通过使用SQL模糊查询实现，但是对于企业级的开发，站内搜索由搜索引擎来实现更为适合。

Django Haystack是一个专门提供搜索功能的Django第三方库，它支持Solr、Elasticsearch、Whoosh和Xapian等多种搜索引擎，配合著名的中文自然语言处理库jieba分词可以实现中文站点的全文搜索系统。

[django-haystack](https://django-haystack.readthedocs.io/en/master/toc.html)



## 安装

```bash
# 如果django是2.0以下，需要指定版本安装，可以参考下面说明或看
pip install django-haystack
pip install whoosh
pip install jieba
```

> 1. 在安装django-haystack还需要找到与django匹配的版本， 官方的haystack document中Changelog里有关于django版本的说明（https://django-haystack.readthedocs.io/en/master/changelog.html)，例如**我这里是python2.7 + django1.8, 对应的版本是：**
>
>    ```bash
>    django-haystack==2.4.0
>    whoosh==2.7.4
>    jieba==0.36.2
>    ```
>
> 2. 无法连接可以使用pip的i参数：`-i https://pypi.tuna.tsinghua.edu.cn/simple`临时指定镜像源。



## 配置haystack

```python
# settings.py
# 1、在app中注册
INSTALLED_APPS = [
    'django.contrib.admin',
    ...  
    'django.contrib.staticfiles',
    'haystack',
    # 你的app名，应该放在haystack下面
    'app',
]

# 2、配置haystack
HAYSTACK_CONNECTIONS = {
    'default': {
        # whoosh搜索引擎默认配置，不支持中文搜索
        # haystack.backends.whoosh_backend.WhooshEngine  
        
        # 设置搜索引擎，文件是app下的whoose_cn_backend.py
        'ENGINE': 'app.whoosh_cn_backend.WhooshEngine',
        'PATH': os.path.join(os.path.dirname(__file__), 'whoosh_index'),
        'INCLUDE_SPELLING': True,
    },
}
# 3、设置每页显示的数据量
HAYSTACK_SEARCH_RESULTS_PER_PAGE = 4
# 4、当数据库改变时，会自动更新索引，非常方便
HAYSTACK_SIGNAL_PROCESSOR = 'haystack.signals.RealtimeSignalProcessor'
```



## 自定义搜索引擎文件实现中文搜索

上面配置属性HAYSTACK_CONNECTIONS的ENGINE指向了项目应用app的`whoosh_cn_backend`文件。该文件是自定义的Whoosh搜索引擎文件，因此，我们需要创建这个文件，把haystack包中的`whoosh_backend.py`内容复制到创建的`whoosh_cn_backend.py`文件中，修改内容如下：

```python
# 引入jieba分词器的模块
from jieba.analyse import ChineseAnalyzer

# 找到下面的内容，修改成：
# schema_fields[field_class.index_fieldname] = TEXT(stored=True, analyzer=StemmingAnalyzer(), field_boost=field_class.boost, sortable=True)
schema_fields[field_class.index_fieldname] = TEXT(stored=True, analyzer=ChineseAnalyzer(), field_boost=field_class.boost, sortable=True)
```



## 搜索逻辑

在要做全文检索的app下创建一个`search_indexes.py`文件，**名字必须是search_indexes.py**。

创建好模型后，就可以编写代码了。这里以文章表acticle为例。

```python
import datetime
from haystack import indexes
# 导入你的文章表
from app.models import Article

# 注意；类名（ArticleIndex）必须是模型名+Index，这里以app下models文件下的Article模型表为例
class ArticleIndex(indexes.SearchIndex, indexes.Indexable):   
    ‘’‘
    document=True  表示haystack和搜索引擎都将使用该字段的内容作为索引
    user_tempalte=True 表示可以使用数据模版，也就是templates目录下的索引字段文件。
    如：templates/search/indexes/app/article_text.txt
    注意： app是你要建立全文检索的app名，article_text是模型表名小写+_text
    ’‘’
    text = indexes.CharField(document=True, use_template=True)
    # 创建一个author字段,model_attr='user'代表对应数据模型Article中的user字段，可以删
 	# author = indexes.CharField(model_attr='user')
 
    # 必须重载这个方法
    def get_model(self):
    	# return 模型表（也就是要做全文检索的表）
        return Article

    def index_queryset(self, using=None):
        """Used when the entire index for model is updated."""
        # 针对哪些数据进行检索，可以在这对数据做处理
        #return self.get_model().objects.filter(pub_date__lte=datetime.datetime.now())
        return self.get_model().objects.all()
```



![image-20210507100507458](http://blog.cdn.ionluo.cn/blog/image-20210507100507458.png)





## 设置模型的索引模板

上面`use_template=True`是使用索引模板建立索引文件，索引模板的路径是固定的，其格式为：`/templates/search/indexes/app名/模型名小写_text.txt`

这里在txt文件里面写要检索的字段：

```txt
# 这里以文章表的 title字段、content字段、为例子
{{ object.title }}
{{ object.content }}
```

![image-20210507101816557](http://blog.cdn.ionluo.cn/blog/image-20210507101816557.png)

上述设置是对Article.title和Article.content两个字段建立索引，当搜索引擎进行检索时，系统会根据搜索条件对这两个字段进行全文搜索匹配，然后将结果排序后并返回。



## 创建索引文件

```bash
python manage.py rebuild_index
```

在项目根目录可以看到whoosh_index文件夹，该文件夹中的文件就是索引文件。

![image-20210507104601716](http://blog.cdn.ionluo.cn/blog/image-20210507104601716.png)



## 实现搜索功能

**urls.py**

```python
# urls.py
from app.views.search import MySearchView

urlpatterns = patterns(
    '',
    ...  
	url(r'^search\.html$', MySearchView(), name='search'),
)
```

**views.search.py**

```python
# views.py
from haystack.views import SearchView as haystackSearchView
from django.shortcuts import render, render_to_response
from django.core.paginator import Paginator, EmptyPage, PageNotAnInteger

class MySearchView(haystackSearchView):
    # 默认的就是这个，可以不指定
    template = 'search/search.html'

    def create_response(self):
        context = {}
        # 这里是出于增加context里面内容的考虑，实际情况自己编码
        # mixin = WebMixin()
        # mixin.request = self.request
        # context = mixin.add_ctx(context)
        # context = mixin.cn_header_menu(context)

        # if not self.request.GET.get('q', ''):
        #     context['show_all'] = True
        #     aboutus = AboutUs.objects.all()
        #     paginator = Paginator(aboutus, settings.HAYSTACK_SEARCH_RESULTS_PER_PAGE)
        #     try:
        #         context['page'] = paginator.page(int(self.request.GET.get('page', 1)))
        #     except PageNotAnInteger:
        #         context['page'] = paginator.page(1)
        #     except EmptyPage:
        #         context['page'] = paginator.page(paginator.num_pages)
        #     return render_to_response(self.template, context, context_instance=self.context_class(self.request))
        #     # return render(self.request, self.template, locals())
        # else:
        # context['show_all'] = False
        (paginator, page) = self.build_page()

        context.update({
            'query': self.query,
            'form': self.form,
            'page': page,
            'paginator': paginator,
            'suggestion': None,
        })

        if self.results and hasattr(self.results, 'query') and self.results.query.backend.include_spelling:
            context['suggestion'] = self.form.get_suggestion()

        context.update(self.extra_context())
        return render_to_response(self.template, context, context_instance=self.context_class(self.request))
```

**templates.search.search.html**

```html
<!-- search.html -->
{% extends 'base.html' %}
{% load highlight %}

{% block content %}
    <h2>Search</h2>

    <form method="get" action="/search.html">
        <table>
            {{ form.as_table }}
            <tr>
                <td>&nbsp;</td>
                <td>
                    <input type="submit" value="Search">
                </td>
            </tr>
        </table>

        {% if query %}
            <h3>Results</h3>

            {% for result in page.object_list %}
                <p>
                    <a href="{{ result.object.get_absolute_url }}">{% highlight result.object.title with query %}</a>
                </p>
            {% empty %}
                <p>No results found.</p>
            {% endfor %}

            {% if page.has_previous or page.has_next %}
                <ul class="pagination">
                    <li>
                        <a href="?q={{ query }}&amp;page={{ page.previous_page_number }}"
                           aria-label="Previous">
                            <i class="fa fa-angle-double-left" aria-hidden="true"></i>
                        </a>
                    </li>
                    <li>
                        Page {{ page.number }} of {{ page.paginator.num_pages }}
                    </li>
                    <li>
                        <a href="?q={{ query }}&amp;page={{ page.next_page_number }}"
                           aria-label="Next">
                            <i class="fa fa-angle-double-right" aria-hidden="true"></i>
                        </a>
                    </li>
        		</ul>
            {% endif %}
        {% else %}
            {# Show some example queries to run, maybe query syntax, something else? #}
        {% endif %}
    </form>
{% endblock %}
```



## 大功告成

如此，在浏览器输入地址http://localhost:8000/search.html?q=1即可访问。



## 问题记录

1. 发现在搜索page超出范围的时候出现了404报错。

   ![image-20210507105704700](http://blog.cdn.ionluo.cn/blog/image-20210507105704700.png)

2. 搜索条件为空该如何显示所有记录而不是空呢？

3. 不好统一返回的字段的key。指定了`title = indexes.CharField(model_attr='name')`来增加一个title字段用来统一html中显示调用`{{ result.object.title }}`, 但是并没有什么效果，还是只能通过`name`来取到值。

   暂时没有发现什么好的主意，这里通过模型类的字符写法返回，html直接用`{{ result.object}}`

   ```python
   # py3是 def __str__(self):
   def __unicode__(self):
       return self.name
   ```

4. 对于搜索结果需要用不同的tab栏目展示貌似不好实现。手册和质检报告是同一个Model，产品的搜索需要结合第三方接口合并结果，这里我不清楚该怎么实现。

   ![image-20210507171627555](http://blog.cdn.ionluo.cn/blog/image-20210507171627555.png)

   



## 参考文献

[django-haystack](https://django-haystack.readthedocs.io/en/master/toc.html)

[Whoosh 2.7.4 documentation](https://whoosh.readthedocs.io/en/latest/index.html)

[Whoosh + jieba 中文检索](https://www.jianshu.com/p/127c8c0b908a)

