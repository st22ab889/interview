你截图里 GitHub 项目中文件夹名称带蓝色且显示类似 `@` 关联哈希值的样式，是 **Git 子模块（Git Submodule）** 的特征，实现步骤如下：

### 一、核心原理  
Git 子模块允许在一个 Git 仓库中嵌套另一个独立 Git 仓库，父仓库记录子模块的仓库地址、关联 commit（即你看到的哈希值，如 `@ 437e1ad` ），在 GitHub 上会自动渲染为这种带 `@` 的蓝色链接样式，点击可跳转子模块对应 commit 页面。

### 二、实现步骤（本地操作 + 推送到 GitHub）  
#### 1. 初始化子模块（本地仓库）  
假设父项目是 `parent-repo`，要添加 `libbloom` 作为子模块：  
```bash
# 进入父项目仓库根目录
cd parent-repo  

# 添加子模块（url 替换为实际子模块仓库地址）
git submodule add https://github.com/example/libbloom.git libbloom  

# 提交子模块配置到父仓库
git commit -m "Add libbloom as submodule"  
git push origin main  # 推送到 GitHub 仓库
```  

#### 2. 关键 Git 命令说明  
| 命令                          | 作用                                                                 |  
|-------------------------------|----------------------------------------------------------------------|  
| `git submodule add <仓库地址>` | 关联子模块仓库，在父仓库生成 `.gitmodules` 记录子模块信息             |  
| `git submodule update --init`  | 克隆项目时，初始化并拉取子模块内容（多人协作或新环境拉取时用）         |  
| `git submodule update --remote`| 更新子模块到远程仓库最新 commit（需提交父仓库记录更新）               |  


### 三、GitHub 上的显示效果  
推送后，GitHub 会自动识别 `.gitmodules` 配置，将子模块文件夹渲染为 **蓝色 + @哈希值** 样式，点击可查看：  
- 子模块仓库地址  
- 关联的具体 commit 内容  
- 子模块仓库的完整文件结构  


### 四、进阶：子模块更新与维护  
#### 1. 更新子模块到最新版本  
```bash
# 进入子模块目录，拉取最新代码
cd libbloom  
git pull origin main  

# 返回父仓库，提交子模块新的 commit 记录
cd ..  
git commit -am "Update libbloom submodule to latest commit"  
git push origin main  
```  

#### 2. 克隆含子模块的项目（协作场景）  
他人克隆父项目时，需额外初始化子模块：  
```bash
# 克隆父仓库
git clone https://github.com/your/parent-repo.git  

# 初始化并拉取子模块内容
cd parent-repo  
git submodule update --init --recursive  
```  


### 五、替代方案（非子模块场景）  
若不想用子模块，但想实现类似“链接关联”效果，可：  
1. 在 README 文档里手动写 Markdown 链接，如 `[libbloom](https://github.com/example/libbloom/tree/437e1ad)`  
2. 用 GitHub Pages + 自定义静态站点工具（如 Jekyll）做目录渲染，但无法像子模块那样自动关联 Git commit  


综上，最直接的方式就是通过 **Git 子模块机制** 实现你截图里的效果，核心依赖 Git 子模块的配置与 GitHub 的自动渲染逻辑 。