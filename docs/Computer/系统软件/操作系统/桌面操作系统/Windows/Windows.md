# Windows


### 系统软件




##### Windows脚本扩展及运行程序


###### Run

**事件查看器**
`win + R << eventvwr.msc`


###### PowerShell

**在PowerShell中查看当前目录下所有文件夹及文件的大小**
```powershell
Get-ChildItem -Directory | ForEach-Object {
	$size = (Get-ChildItem -Path $_.FullName -Recurse -File | Measure-Object -Property Length -Sum).Sum
	
	[PSCustomObject]@{
		Directory = $_.FullName
		Size = "{0:N2} MB " -f ($size / 1MB)
	}
}
```