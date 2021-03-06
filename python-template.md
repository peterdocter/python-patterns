#  [python设计模式之模板方法模式](http://dongweiming.github.io/python-template.html)

## 模板方法模式

不知道你有没有注意过，我们实现某个业务功能，在不同的对象会有不同的细节实现，
以前说过[策略模式](http://dongweiming.github.io/python-strategy.html),
策略模式是将逻辑封装在一个类(提到的文章中的Duck)中，然后使用委托的方式解决。 模板方法模式的角度是：把不变的框架抽象出来，定义好要传入的细节的接口.
各产品类的公共的行为 会被提出到公共父类，可变的都在这些产品子类中

## python的例子

    
    
    '''http://ginstrom.com/scribbles/2007/10/08/design-patterns-python-style/'''
    
    # 整个例子我们要根据不同需求处理的内容
    ingredients = "spam eggs apple"
    line = '-' * 10
    
    # 这是被模板方法调用的基础函数
    def iter_elements(getter, action):
        """循环处理的骨架"""
        # getter是要迭代的数据，action是要执行的函数
        for element in getter():
            action(element)
            print(line)
    
    def rev_elements(getter, action):
        """反向的"""
        for element in getter()[::-1]:
            action(element)
            print(line)
    
    # 数据经过函数处理就是我们最后传给模板的内容
    def get_list():
        return ingredients.split()
    
    # 同上
    def get_lists():
        return [list(x) for x in ingredients.split()]
    
    # 对数据的操作
    def print_item(item):
        print(item)
    #反向处理数据
    def reverse_item(item):
        print(item[::-1])
    
    # 模板函数
    def make_template(skeleton, getter, action):
        # 它抽象的传入了 骨架，数据，和子类的操作函数
        def template():
            skeleton(getter, action)
        return template
    
    # 列表解析，数据就是前面的2种骨架(定义怎么样迭代)，2个分割数据的函数，正反向打印数据的组合
    templates = [make_template(s, g, a)
                for g in (get_list, get_lists)
                for a in (print_item, reverse_item)
                for s in (iter_elements, rev_elements)]
    
    # 执行
    for template in templates:
        template()
    


