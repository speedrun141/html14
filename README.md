<!DOCTYPE HTML >
<html lang="ja">
<head>
<META http-equiv="Content-Type" content="text/html; charset=Shift_JIS">
<meta http-equiv="Content-Script-Type" content="text/javascript">
<META http-equiv="Content-Style-Type" content="text/css">
<title>Ðôîæ£¨ËÒÊÅ£©</title>
<style type="text/css">
<!--
* { font-size:20px; }
body {
    color:#444488;
    line-height:120%;
}
#guide {
    position:absolute;
    top:1150px;     /* ¡ç «Ù?«¹?ßÀ ?«µ«¤«ºì¤ß¾ªòò¦ïÒ(?Ù¥ÙþªÎêÈöÇ) */
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
    left:1600px;     /* ¡ç «Ù?«¹?ßÀ ?«µ«¤«ºªòò¦ïÒ(«Ñ«Í«ëªÎêÈöÇùêªïª») */
    height:1200px;   /* ¡ç «Ù?«¹?ßÀ ?«µ«¤«ºªòò¦ïÒ(«Ñ«Í«ëªÎêÈöÇùêªïª») */
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
<input type="button" value="?ßÀªÎÜÁðí" id="submitbutton" />
<select id="set-list" onchange="setSelected()">
<!-- øúãÆ«»«Ã«È -->
<option selected>Õ£</option>
<option>ð¤Ü×</option>
<option>ØªÛöá´Ò³</option>
<option>«Ó«Ã«Á</option>
<option>«»?«é?</option>
<option>«Ê?«¹</option>
<option>«Ð«Ë?</option>
<option>«Á«¢«¬?«ë</option>
<option>«á«¤«É</option>
<option>«Ó?«¹«È</option>
<option>«Ü«ó«Ç?«¸</option>
<option>â©ó·</option>
<option>«¨«íâ©ó·</option>
<option>Òïâ©ó·</option>
<option>«¹«¯â©</option>
<option>«ê«ê«««ë</option>
<option>ü£Êªëýß×</option>
<option>«»«¤«Ð?</option>
<option>10ñ²Ò´</option>
<!-- øúãÆ«»«Ã«Èª³ª³ªÞªÇ -->
</select>
</p>
<p>
Üô÷âÙ¥Óø <select id="opacity-list" onchange="opacitySelected()">
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
<!-- 0% ªËªÏÚ±?? -->
</select>
</p>
<div id="img-list">
<!-- ?ßÀ?ªß?ªßý­¡¢ª³ªÎÙþßöªÏÞûð¶ªµªìªÞª¹ -->
<p>?ªß?ªßñéªÇª¹¡£ªÁªçªÃªÈªÞªÃªÆªÍ¡¦¡£</p>
<p>¡Øª­ª»ª«ª¨ªËªÏ Internet Explorer 5.0 / Netscape 6 ì¤Ë½ªÊªÉªÎDHTML??«Ö«é«¦«¶ª¬ù±é©ªÇª¹¡£</p>
<p>¡Øª­ª»ª«ª¨?ßÀªÎÜÁðíªËªÏ canvas??«Ö«é«¦«¶ª¬ù±é©ªÇª¹¡£</p>
</div>
<p title="«Õ«§?«É?ÍýªÎON/OFFàâïÒ¡£??ªÏIEªÎªßªÇª¹" style="display:none">
?Íý: <input type="radio" name="effect-sw" value="" id="effect-on">ON
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