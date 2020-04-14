# Mac上安装HomeBrew并设置国内源

## 1、获取官方安装命令

```bash
# 官方的安装命令是直接通过ruby执行install脚本的，我们这里因为需要修改为国内源，所以不执行下面的命令
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

# 获得安装脚本
curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install >> ./homebrew_install

# 卸载
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
```

## 2、配置国内源

```text
打开 brew_install 文件，修改如下：
找到如下代码：
BREW_REPO = “https://github.com/Homebrew/brew“.freeze
CORE_TAP_REPO = “https://github.com/Homebrew/homebrew-core“.freeze
复制代码更改为：
#BREW_REPO = "https://github.com/Homebrew/brew".freeze
BREW_REPO = "git://mirrors.ustc.edu.cn/brew.git".freeze
CORE_TAP_REPO = "git://mirrors.ustc.edu.cn/homebrew-core.git".freeze
```

> 新版本HomeBrew可能没有CORE_TAP_REPO这句代码，如果没有不用新增。

```bash
sed -i 's#BREW_REPO = “https://github.com/Homebrew/brew“.freeze#BREW_REPO = “https://mirrors.ustc.edu.cn/brew.git “.freeze#g' ./homebrew_install
```

> 上面的命令如果不放心，请手动修改。

## 3、开始安装

```bash
/usr/bin/ruby ./homebrew_install
```

脚本会卡在：

```bash
==> Tapping homebrew/core Cloning into ‘/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core’…
```

这个时候我们可以Ctrl + C 取消执行，然后执行以下命令：

```bash
git clone git://mirrors.ustc.edu.cn/homebrew-core.git/ /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core --depth=1
```

复制代码然后把homebrew-core的镜像地址也设为中科院的国内镜像:

```bash
cd "$(brew --repo)"
git remote set-url origin git://mirrors.ustc.edu.cn/brew.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin git://mirrors.ustc.edu.cn/homebrew-core.git
```

安装完成！

## 4、检查

```bash
brew update
brew doctor
```

## 5、简单使用

```bash
# 安装软件：brew install 软件名，例：
brew install wget
# 搜索软件：brew search 软件名，例：
brew search wget
# 卸载软件：brew uninstall 软件名，例：
brew uninstall wget
# 更新所有软件：
brew update
# 更新具体软件：brew upgrade 软件名 ，例：
brew upgrade git
# 显示已安装软件：
brew list
# 查看软件信息：brew info／home 软件名 ，例：
brew info git
brew home git
# brew home指令是用浏览器打开官方网页查看软件信息

# 查看哪些已安装的程序需要更新： 
brew outdated
# 显示包依赖：
brew reps
# 显示帮助：
brew help
```

## 6、卸载

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
```