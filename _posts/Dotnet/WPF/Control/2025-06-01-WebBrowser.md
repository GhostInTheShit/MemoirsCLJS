---
categories: WPF
---

# WebBrowser

内置的浏览器,不太好用,会频繁的弹框问你需不需要安装脚本,而且版本很低,某些页面会拒绝.

内置的3个Command
*   BrowseBack
*   BrowseForward
*   GoToPage

方法

*   WebB.CanGoForward
*   WebB.GoForward()
*   WebB.CanGoBack
*   WebB.GoBack()
*   **WebB.Navigate(url)** 导航到url,需要验证url,不合格会报错(需要`http://`或者(`https://`开头))
