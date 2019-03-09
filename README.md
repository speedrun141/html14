﻿<!DOCTYPE HTML >
<html lang="ja">
<head>
<META http-equiv="Content-Type" content="text/html; charset=Shift_JIS">
<meta http-equiv="Content-Script-Type" content="text/javascript">
<META http-equiv="Content-Style-Type" content="text/css">
<title>奇跡（開脚）</title>
<style type="text/css">
<!--
* { font-size:20px; }
body {
    color:#444488;
    line-height:120%;
}
#guide {
    position:absolute;
    top:1150px;     /* ← ベ?ス?像 ?サイズ以上を指定(?明文の位置) */
    padding:20px;
}
#panel {
    color:#444488;
    background:#EFF3FC;
    cursor:default;
    overflow:auto;
    position:absolute;
    z-index:100;
    top:0;
    width:320px;
    left:1600px;     /* ← ベ?ス?像 ?サイズを指定(パネルの位置合わせ) */
    height:1200px;   /* ← ベ?ス?像 ?サイズを指定(パネルの位置合わせ) */
}
#panel p { text-align:center; }
#panel p, #img-list table {
    background:#DBE4FA;
    border:2px solid #BBC7DC;
    padding:2px;
    margin:0 0 4px 0;
}
#set-list { width:105px; }
#img-body { position:absolute; left:0; top:0; }
#img-list .swnm { width:100%; }
#img-list td { margin:0; padding:0; }
#img-list { height:1100px; overflow:auto; }
-->
</style>
</head>
<BODY onload="init()" background="">
<script src="./build/jquery-1.11.1.min.js"></script>
<script src="maidR.js" type="text/javascript"></script>
<script src="./build/html2image.js" type="text/javascript"></script>
<script src ="settei.js" type="text/javascript"></script>
<div id="img-body">
</div><!-- /#img-body -->
<div id="panel">
<p>
<input type="button" value="?像の保存" id="submitbutton" />
<select id="set-list" onchange="setSelected()">
<!-- 表示セット -->
<option selected>裸</option>
<option>制服</option>
<option>魔法少女</option>
<option>ビッチ</option>
<option>セ?ラ?</option>
<option>ナ?ス</option>
<option>バニ?</option>
<option>チアガ?ル</option>
<option>メイド</option>
<option>ビ?スト</option>
<option>ボンデ?ジ</option>
<option>水着</option>
<option>エロ水着</option>
<option>紐水着</option>
<option>スク水</option>
<option>リリカル</option>
<option>花嫁衣裳</option>
<option>セイバ?</option>
<option>10周年</option>
<!-- 表示セットここまで -->
</select>
</p>
<p>
不透明度 <select id="opacity-list" onchange="opacitySelected()">
<option selected value="100">100%</option>
<option value="90">90%</option>
<option value="80">80%</option>
<option value="70">70%</option>
<option value="60">60%</option>
<option value="50">50%</option>
<option value="40">40%</option>
<option value="30">30%</option>
<option value="20">20%</option>
<option value="10">10%</option>
<!-- 0% には未?? -->
</select>
</p>
<div id="img-list">
<!-- ?像?み?み後、この文書は削除されます -->
<p>?み?み中です。ちょっとまってね…。</p>
<p>※きせかえには Internet Explorer 5.0 / Netscape 6 以降などのDHTML??ブラウザが必要です。</p>
<p>※きせかえ?像の保存には canvas??ブラウザが必要です。</p>
</div>
<p title="フェ?ド?果のON/OFF設定。??はIEのみです" style="display:none">
?果: <input type="radio" name="effect-sw" value="" id="effect-on">ON
<input type="radio" name="effect-sw" value="" checked>OFF 
</p>
</div><!-- /#panel --><!-- /#guide -->
<div hidden>
<canvas  id="sample" width ="1600" height = "1200"></canvas>
</div>
<script>
window.addEventListener("load", function(){
$("#submitbutton").click(function(e){
          function Base64toBlob(_base64){
    		var i;
    		var tmp = _base64.split(',');
    		var data = atob(tmp[1]);
			var mime = tmp[0].split(':')[1].split(';')[0];
		
    		//var buff = new ArrayBuffer(data.length);
		    //var arr = new Uint8Array(buff);
			var arr = new Uint8Array(data.length);
			for (i = 0; i < data.length; i++) {arr[i] = data.charCodeAt(i);}
			var blob = new Blob([arr], { type: mime });
    		return blob;
		  }
		  function saveBlob(_blob,_file)
		  {
		  	  var userAgent = window.navigator.userAgent.toLowerCase();
	 		  if (userAgent.indexOf('msie') > -1 || userAgent.indexOf('trident') > -1){	
        		  window.navigator.msSaveOrOpenBlob(_blob, _file);
    		  }else{
        		  var url = (window.URL || window.webkitURL);
        		  var data = url.createObjectURL(_blob);
        		  var e = document.createEvent("MouseEvents");
        		  e.initMouseEvent("click", true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null);
        		  var a = document.createElementNS("http://www.w3.org/1999/xhtml", "a");
        		  a.href = data;
        		  a.download = _file;   
        		  a.dispatchEvent(e);
    		  }
		  }
        html2canvas(document.body, {
          onrendered: function(image) {
          	var canvas = document.getElementById('sample');
          	var context = canvas.getContext('2d');
          	var img = new Image();
          	img.src =image;
          	context.drawImage(img, 0, 0,1600,1200,0,0,1600,1200);
          	var data = canvas.toDataURL('image/jpeg');
          	var blob = Base64toBlob(data);
          	saveBlob(blob,"kuroiko_open.jpg");
          },
          imageType:'jpeg'
        });
      
});
},false);
</script>
</BODY>
</html>