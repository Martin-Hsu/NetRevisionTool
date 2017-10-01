# Introduction
NetRevisionTool 這個專案是為了讓vistual studio build的時候可以去讀取 AssemblyInfo.cs.bak 的資訊來產生AssemblyInfo.cs

# 支援的VCS
VCS (version control system)
* Git (我目前測試的部分)
* Subversion

# 為什麼fork一份，與原作有什麼差異
原作的版本跑在中文的環境下會有問題，因此修改了一些

# 如何使用NetRevisionTool
Step 1:
將NetRevisionTool.exe放在$(ProjectDir)裡面
並在專案屬性中=>建置事件=>建置前事件命令列=>填上:$(ProjectDir)NetRevisionTool.exe /info /patch $(ProjectDir)

Step 2:
複製一份 Properties/AssemblyInfo.cs 改名為 AssemblyInfo.cs.bak

Step 3:
修改AssemblyInfo.cs.bak
[assembly: AssemblyVersion("1.{c:hms.}")]
[assembly: AssemblyFileVersion("1.{c:ymd.}")]
[assembly: AssemblyInformationalVersion("{branch}-{chash}")]

這樣的話執行過後，AssemblyInfo.cs會變成:
[assembly: AssemblyVersion("1.18.38.42")]
[assembly: AssemblyFileVersion("1.2017.10.01")]
[assembly: AssemblyInformationalVersion("master-e0d6cec5690ac2e23fc492d01343baa44e496945")]

{c:hms.} : 表示commit的時間
{c:ymd.} : 表示commit的日期
{branch} : 表示目前的branch
{chash} : 表示目前的commit hash

# 如何在程式碼中讀取AssemblyInfo資訊
using System.Reflection;

Assembly assembly = Assembly.GetExecutingAssembly();
var AssemblyInformationalVersionAttribute = assembly
                  .GetCustomAttributes(typeof(AssemblyInformationalVersionAttribute), false)
                  .OfType<AssemblyInformationalVersionAttribute>()
                  .FirstOrDefault();
				  
string info = AssemblyInformationalVersionAttribute.InformationalVersion;
//info 會是 master-e0d6cec5690ac2e23fc492d01343baa44e496945 

# Licence and terms of use
This software is released under the terms of the GNU GPL licence, version 3. You can find the detailed terms and conditions in the download or on the [GNU website](http://www.gnu.org/licenses/gpl-3.0.html).
