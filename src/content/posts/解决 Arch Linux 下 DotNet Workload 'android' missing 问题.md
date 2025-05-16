## 前言

在尝试开发 AvaloniaUI 程序时，我发现无法构建 Android 应用，Rider 提示：

```bash
dotnet workload install android
```

## .NET for Android 是什么？

.NET for Android 提供了 Android SDK 的开源绑定，并支持 .NET 语言（如 C#）。它作为 .NET 6+ 的可选 Workload，能够独立于 .NET 更新，以适应 Android 平台和工具链的变化。

.NET for Android 是 .NET MAUI 的一部分，也可以独立用于原生 Android 开发。
![[Pasted image 20250226145800.png]]
在 Arch Linux 上，我使用 AUR 构建并安装了默认的 .NET 9 SDK，尝试安装 Android Workload 时出现问题：
![[Pasted image 20250226143439.png]]

## 问题分析
查看workload列表

~~~bash
dotnet workload search
~~~
查看已安装的 Workload 列表后，发现缺少 `android` Workload。
![[Pasted image 20250226143701.png]]

通过搜索，找到相关 GitHub Issue：

[Arch Linux: workload 'android' missing on .NET 8 Linux · Issue #97319 · dotnet/runtime](https://github.com/dotnet/runtime/issues/97319)

原来这个问题早在一年前就已存在，归因于 Arch Linux 的打包导致 Workload 丢失。 微软并不为其他 Linux 发行版提供默认的 .Net包，因此需要手动修复。
![[Pasted image 20250226143834.png]]

## 解决方案

1. **从 .NET 官网下载对应架构的二进制 SDK**
    
    - 确保下载的版本与当前 .NET 版本匹配
    ![[Pasted image 20250226143926.png]]
    
2. **解压 SDK 文件**
    
    ```bash
    tar -xvf dotnet-sdk.tar.gz -C /opt/dotnet
    ```
    
3. **替换** `sdk-manifests` **文件夹**
    
    ```bash
    cp -r /opt/dotnet/sdk-manifests/* /usr/share/dotnet/sdk-manifests/
    ```
    
4. **验证安装**
    
    ```
    dotnet workload list
    ```
    
    
    再次查看,出现了android
    ![[Pasted image 20250226144120.png]]
再次安装
```
dotnet workload install android
```
## 结论

通过手动下载并替换 `sdk-manifests`，安装 Android Workload。现在可以继续开发 Android 应用了！ 🚀