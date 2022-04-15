# live2d
 The beta version of live2d for the blog of Bensz

# 参考

源代码来自[鸦鸦](https://crowya.com/)的博客文章：

+ [Live2D 宠物功能修改|音乐播放器+右键秘密通道](https://crowya.com/1088)
+ [养一只博客宠物](https://crowya.com/1092)

# 使用

```html
<script src="https://cdn.jsdelivr.net/gh/huangwb8/live2d@latest/autoload.js"></script> 
```

# 介绍

偶然看到一个[鸦鸦](https://crowya.com/)的看板娘方案十分特别。

TA使用的是WordPress平台，通过对[stevenjoezhang/live2d-widget](https://github.com/stevenjoezhang)、[fghrsh/live2d_api](https://github.com/fghrsh/live2d_api)、[summerscar/live2dDemo](https://github.com/summerscar/live2dDemo)、[Aplayer](https://github.com/DIYgod/APlayer)、[MetingJS](https://github.com/metowolf/MetingJS)等项目的代码进行一系列改造，实现了十分干净的效果。

效果总结如下：

+ 音乐播放器功能集成至看板娘的音乐按钮
+ 左击音乐按钮可实现音乐的播放和暂停
+ 右击宠物可打开秘密通道，上面有“音乐播放器”的链接，左击之后可以打开`MetingJS`播放器的完全体；再左击可以隐藏。
+ 其它：我估计是修改`waifu.css`等文件实现的，不明觉厉。
  + 按钮左移
  + 宠物比默认值要小

示意图如下：

![image-20220415170452739](https://chevereto2.hwb0307.top:7500/images/2022/04/15/image-20220415170452739.png)

TA还作了一些和wordpress文件的相关改动，包括：

+ 主题的footer.php文件：

```php+HTML
<!--音乐播放器-->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.css">
<script src="https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/meting@2.0.1/dist/Meting.min.js"></script>
<meting-js id="6912478729" server="netease" type="playlist" theme="#339981" fixed="true"
preload="auto" autoplay="false" loop="all" order="random" volume="0.3"></meting-js>
<script>
function removelrc(){
	$(".aplayer.aplayer-fixed .aplayer-body").addClass("my-hide");
	//document.querySelector('meting-js').aplayer.lrc.hide();
	$(".aplayer.aplayer-fixed .aplayer-icon-lrc").addClass("aplayer-icon-lrc-inactivity");
	$(".aplayer.aplayer-fixed .aplayer-lrc").addClass("aplayer-lrc-hide");
	document.querySelector('meting-js').aplayer.on('play', function () {
    		document.querySelector('meting-js').aplayer.lrc.show();
    		$(".aplayer.aplayer-fixed .aplayer-icon-lrc").removeClass("aplayer-icon-lrc-inactivity");
	});
	document.querySelector('meting-js').aplayer.on('pause', function () {
    		document.querySelector('meting-js').aplayer.lrc.hide();
    		$(".aplayer.aplayer-fixed .aplayer-icon-lrc").addClass("aplayer-icon-lrc-inactivity");
	});
}
document.querySelector('meting-js').addEventListener("DOMNodeInserted",removelrc);
setTimeout(function() {
	document.querySelector('meting-js').removeEventListener("DOMNodeInserted",removelrc);
	//移除左侧栏切换时的监听事件防止页面刷新
	if($("#leftbar_tab_catalog_btn").length > 0){
		var el = document.getElementById('leftbar_tab_catalog_btn'),elClone = el.cloneNode(true);
		el.parentNode.replaceChild(elClone, el);
	}
	var el = document.getElementById('leftbar_tab_overview_btn'),elClone = el.cloneNode(true);
	el.parentNode.replaceChild(elClone, el);
	var el = document.getElementById('leftbar_tab_tools_btn'),elClone = el.cloneNode(true);
	el.parentNode.replaceChild(elClone, el);
}, 5000);
</script>
```

+ Wordpress-dashboard - 主题 - 自定义 - 额外 CSS

```css
.my-hide{
	display:none !important;
}
.zero-margin-bottom{
	margin-bottom:0 !important;
}
```

# 问题

虽然我尝试了作者的代码，但是没有办法成功实现TA的博客效果。

实际效果就是看板娘不能正常显示。

原作者已经没有办法联系上，希望有大佬可以帮忙测试一下这个方案有什么问题。

