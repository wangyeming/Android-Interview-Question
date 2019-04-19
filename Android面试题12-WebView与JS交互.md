1. Android和WebView的JS如何通信

# Android和WebView的JS如何通信

* Android去调用JS的代码

1. 通过WebView的loadUrl() 
2. 通过WebView的evaluateJavascript()

* JS调用Android代码的方法有3种：

1. 通过 WebView的addJavascriptInterface()进行对象映射 
2. 通过 WebViewClient 的shouldOverrideUrlLoading ()方法回调拦截 url 
3. 通过 WebChromeClient 的onJsAlert()、onJsConfirm()、onJsPrompt()方法回调拦截JS对话框alert()、confirm()、prompt() 消息