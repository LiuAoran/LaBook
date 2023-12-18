---
tags:
  - 技术/NET
  - 技术/WPF
  - 技术/工具包
---
## 常规

在WPF中，当我们要使用MVVM的方式绑定一个普通对象的属性时，界面上往往需要获取到属性变更的通知。一般我们会新建一个类，并继承INotifyPropertyChanged接口。

以下是示例代码：

```C#
class NotifyObject : INotifyPropertyChanged
{
	private int number;
	public int Number
	{
    	get{return number;}
    	set{number = value; OnPeopertyChanged("Number");}
		//Number函数传递Number参数
	}

	private string text;
	public string Text
	{
    	get{return text;}
    	set{text = value;  OnPeopertyChanged("Text");}
    	//Text函数传递Text参数
	}

	public event PropertyChangedEventHandler PropertyChanged;/*  */
	//采用硬编码传递属性名称，一旦拼写错误或忘记更新就会导致界面无法更新
	protected void OnPropertyChanged(string propertyName = "")
    {
        PropertyChangedEventHandler handler = PropertyChanged;
        if (handler != null)
        {
        	handler(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

## 使用CallerMemberName

在 `OnPropertyChanged` 方法中使用了 `CallerMemberName` 特性（关联：[[内容/技术/NET/CSharp/CSharp Tools/Caller Information Attributes|Caller Information Attributes]]中介绍了CallerMemberName），这样可以在调用方法时自动获取调用者的成员名称（在这里是属性名称）。这样一来，在属性更改时，不再需要显式传递属性名称，而是由编译器自动提供。

以下是示例代码：

```C#
class NotifyObject : INotifyPropertyChanged
{
	private int number;
	public int Number
	{
    	get{return number;}
    	set{number = value; OnPeopertyChanged();}
		//Number函数不再传递Number参数，传递空参
	}

	private string text;
	public string Text
	{
    	get{return text;}
    	set{text = value;  OnPeopertyChanged();}
    	//Text函数不再传递Text参数，传递空参
	}

	/*用[CallerMemberName]属性修饰参数，
	 *那么在某个属性发生改变时，
	 *会调用此函数，propertyName就有了该属性的名字，
	 *因此实现前面相同的功能，
	 *但我们不需要显示传入属性名了
	 */
	protected void OnPropertyChanged([CallerMemberName]string propertyName = "")=>
    	PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName))
}
```

## 使用泛型方法

使用了泛型方法 `UpdateProper`，该方法可以用于更新任何类型的属性值，并且在属性更改时触发 `PropertyChanged` 事件。这样的设计可以使代码更加通用和复用。

```C#
class NotifyObject : INotifyPropertyChanged
{
	private int number;
	public int Number
	{
    	get{return number;}
    	set{UpdateProper(ref number, value);}
	}

	private string text;
	public string Text
	{
    	get{return text;}
    	set{UpdateProper(ref number, value);}
	}

	protected void UpdateProper<T>(ref T properValue, 
								   T newValue, 
								   [CallerMemberName] string properName = "")
        {
            if (object.Equals(properValue, newValue))
                return;
 
            properValue = newValue;
            OnPropertyChanged(properName); 
        }


	protected void OnPropertyChanged([CallerMemberName]string propertyName = "")=>
    	PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```