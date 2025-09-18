# MFC 项目 `.rc` 文件打不开可视化窗口排坑笔记

## 问题表现
- 打开 `.rc` 文件时报错：
fatal error RC1015: cannot open include file 'afxres.h'

markdown
复制代码
- 新建 MFC 项目一切正常，旧项目打不开。

---

## 原因分析
- 直接打开 `.rc` 文件或 `.vcxproj` 文件，VS 没有加载完整的项目配置。  
- `afxres.h` 是 MFC 自带的头文件，路径需要通过项目属性里的包含目录来指定。  
- `.sln` 文件里保存了原作者的项目结构和配置，绕过它就会丢失环境变量设置。  

---

## 解决方案

### 1. 优先用 `.sln` 文件打开工程
- 双击 `.sln`，让 VS 自动恢复原作者配置。  
- 避免直接用 `.rc` 或 `.vcxproj` 打开。  

### 2. 如果没有 `.sln` 文件
- 检查是否安装了 MFC 组件：  
打开 **Visual Studio Installer → 单个组件 → 勾选 *MFC for C++***。  
- 在项目属性 → `资源` → `常规` → `附加包含目录`，手动添加：  
```text
$(VC_IncludePath)
$(WindowsSDK_IncludePath)
C:\Program Files (x86)\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\<版本号>\atlmfc\include
3. 清理重建
删除 .vs、x64、Debug、Release 文件夹

然后重新生成解决方案

经验总结
打开老项目 → 一定要找 .sln。

没有 .sln → 自己配置包含目录。

afxres.h 在 ...\atlmfc\include 下，找不到就说明 MFC 没装。
