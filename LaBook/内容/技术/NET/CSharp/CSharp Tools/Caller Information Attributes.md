#技术/NET #技术/工具包 

`CallerMemberName`，`CallerFilePath` 和 `CallerLineNumber` 是 C# 中的调用者信息特性（Caller Information Attributes），它们提供了在编写代码时获取调用方的信息的便利方式。这些特性通常与可选的参数一起使用，这些参数由编译器在调用时填充。

## CallerMemberName

`CallerMemberName` 是一个可选参数，用于获取调用方的成员名称（方法、属性等）。当在方法中使用 `CallerMemberName` 特性时，编译器将自动填充调用方的成员名称。这对于日志记录、调试和其他场景中很有用，因为无需在每个调用点显式提供成员名称。

(关联：可以用于[[内容/技术/NET/WPF/WPF Tools/简化INotifyPropertyChanged实现|简化INotifyPropertyChanged实现]]）

以下是示例代码：
```C#
public void LogMessage(string message, [CallerMemberName] string memberName = "")
{
    Console.WriteLine($"{memberName}: {message}");
}
```

## CallerFilePath

`CallerFilePath` 用于获取调用方的文件路径。通过在方法参数中使用 `CallerFilePath` 特性，编译器将自动填充调用方的文件路径。这在记录日志和调试时很有用。

以下是示例代码：
```C#
public void LogMessage(string message, [CallerFilePath] string filePath = "")
{
    Console.WriteLine($"{filePath}: {message}");
}
```


## CallerLineNumber

`CallerLineNumber` 用于获取调用方的行号。通过在方法参数中使用 `CallerLineNumber` 特性，编译器将自动填充调用方的行号。

```C#
public void LogMessage(string message, [CallerLineNumber] int lineNumber = 0)
{
    Console.WriteLine($"Line {lineNumber}: {message}");
}
```

这三个特性可以结合使用，以获得更详细的调用方信息。注意，这些特性只有在调用方法时才会由编译器填充，因此在某些情况下，可能会出现不准确或不完整的信息。