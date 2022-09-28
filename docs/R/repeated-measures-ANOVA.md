<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>

<title><a href="https://repub.github.io/DLC_statistical_guides/docs/Info/repeated-measures-ANOVA">Repeated measures ANOVA</a> in <code>R</code></title>

<script type="text/javascript">
window.onload = function() {
  var imgs = document.getElementsByTagName('img'), i, img;
  for (i = 0; i < imgs.length; i++) {
    img = imgs[i];
    // center an image if it is the only element of its parent
    if (img.parentElement.childElementCount === 1)
      img.parentElement.style.textAlign = 'center';
  }
};
</script>

<!-- Styles for R syntax highlighter -->
<style type="text/css">
   pre .operator,
   pre .paren {
     color: rgb(104, 118, 135)
   }

   pre .literal {
     color: #990073
   }

   pre .number {
     color: #099;
   }

   pre .comment {
     color: #998;
     font-style: italic
   }

   pre .keyword {
     color: #900;
     font-weight: bold
   }

   pre .identifier {
     color: rgb(0, 0, 0);
   }

   pre .string {
     color: #d14;
   }
</style>

<!-- R syntax highlighter -->
<script type="text/javascript">
var hljs=new function(){function m(p){return p.replace(/&/gm,"&amp;").replace(/</gm,"&lt;")}function f(r,q,p){return RegExp(q,"m"+(r.cI?"i":"")+(p?"g":""))}function b(r){for(var p=0;p<r.childNodes.length;p++){var q=r.childNodes[p];if(q.nodeName=="CODE"){return q}if(!(q.nodeType==3&&q.nodeValue.match(/\s+/))){break}}}function h(t,s){var p="";for(var r=0;r<t.childNodes.length;r++){if(t.childNodes[r].nodeType==3){var q=t.childNodes[r].nodeValue;if(s){q=q.replace(/\n/g,"")}p+=q}else{if(t.childNodes[r].nodeName=="BR"){p+="\n"}else{p+=h(t.childNodes[r])}}}if(/MSIE [678]/.test(navigator.userAgent)){p=p.replace(/\r/g,"\n")}return p}function a(s){var r=s.className.split(/\s+/);r=r.concat(s.parentNode.className.split(/\s+/));for(var q=0;q<r.length;q++){var p=r[q].replace(/^language-/,"");if(e[p]){return p}}}function c(q){var p=[];(function(s,t){for(var r=0;r<s.childNodes.length;r++){if(s.childNodes[r].nodeType==3){t+=s.childNodes[r].nodeValue.length}else{if(s.childNodes[r].nodeName=="BR"){t+=1}else{if(s.childNodes[r].nodeType==1){p.push({event:"start",offset:t,node:s.childNodes[r]});t=arguments.callee(s.childNodes[r],t);p.push({event:"stop",offset:t,node:s.childNodes[r]})}}}}return t})(q,0);return p}function k(y,w,x){var q=0;var z="";var s=[];function u(){if(y.length&&w.length){if(y[0].offset!=w[0].offset){return(y[0].offset<w[0].offset)?y:w}else{return w[0].event=="start"?y:w}}else{return y.length?y:w}}function t(D){var A="<"+D.nodeName.toLowerCase();for(var B=0;B<D.attributes.length;B++){var C=D.attributes[B];A+=" "+C.nodeName.toLowerCase();if(C.value!==undefined&&C.value!==false&&C.value!==null){A+='="'+m(C.value)+'"'}}return A+">"}while(y.length||w.length){var v=u().splice(0,1)[0];z+=m(x.substr(q,v.offset-q));q=v.offset;if(v.event=="start"){z+=t(v.node);s.push(v.node)}else{if(v.event=="stop"){var p,r=s.length;do{r--;p=s[r];z+=("</"+p.nodeName.toLowerCase()+">")}while(p!=v.node);s.splice(r,1);while(r<s.length){z+=t(s[r]);r++}}}}return z+m(x.substr(q))}function j(){function q(x,y,v){if(x.compiled){return}var u;var s=[];if(x.k){x.lR=f(y,x.l||hljs.IR,true);for(var w in x.k){if(!x.k.hasOwnProperty(w)){continue}if(x.k[w] instanceof Object){u=x.k[w]}else{u=x.k;w="keyword"}for(var r in u){if(!u.hasOwnProperty(r)){continue}x.k[r]=[w,u[r]];s.push(r)}}}if(!v){if(x.bWK){x.b="\\b("+s.join("|")+")\\s"}x.bR=f(y,x.b?x.b:"\\B|\\b");if(!x.e&&!x.eW){x.e="\\B|\\b"}if(x.e){x.eR=f(y,x.e)}}if(x.i){x.iR=f(y,x.i)}if(x.r===undefined){x.r=1}if(!x.c){x.c=[]}x.compiled=true;for(var t=0;t<x.c.length;t++){if(x.c[t]=="self"){x.c[t]=x}q(x.c[t],y,false)}if(x.starts){q(x.starts,y,false)}}for(var p in e){if(!e.hasOwnProperty(p)){continue}q(e[p].dM,e[p],true)}}function d(B,C){if(!j.called){j();j.called=true}function q(r,M){for(var L=0;L<M.c.length;L++){if((M.c[L].bR.exec(r)||[null])[0]==r){return M.c[L]}}}function v(L,r){if(D[L].e&&D[L].eR.test(r)){return 1}if(D[L].eW){var M=v(L-1,r);return M?M+1:0}return 0}function w(r,L){return L.i&&L.iR.test(r)}function K(N,O){var M=[];for(var L=0;L<N.c.length;L++){M.push(N.c[L].b)}var r=D.length-1;do{if(D[r].e){M.push(D[r].e)}r--}while(D[r+1].eW);if(N.i){M.push(N.i)}return f(O,M.join("|"),true)}function p(M,L){var N=D[D.length-1];if(!N.t){N.t=K(N,E)}N.t.lastIndex=L;var r=N.t.exec(M);return r?[M.substr(L,r.index-L),r[0],false]:[M.substr(L),"",true]}function z(N,r){var L=E.cI?r[0].toLowerCase():r[0];var M=N.k[L];if(M&&M instanceof Array){return M}return false}function F(L,P){L=m(L);if(!P.k){return L}var r="";var O=0;P.lR.lastIndex=0;var M=P.lR.exec(L);while(M){r+=L.substr(O,M.index-O);var N=z(P,M);if(N){x+=N[1];r+='<span class="'+N[0]+'">'+M[0]+"</span>"}else{r+=M[0]}O=P.lR.lastIndex;M=P.lR.exec(L)}return r+L.substr(O,L.length-O)}function J(L,M){if(M.sL&&e[M.sL]){var r=d(M.sL,L);x+=r.keyword_count;return r.value}else{return F(L,M)}}function I(M,r){var L=M.cN?'<span class="'+M.cN+'">':"";if(M.rB){y+=L;M.buffer=""}else{if(M.eB){y+=m(r)+L;M.buffer=""}else{y+=L;M.buffer=r}}D.push(M);A+=M.r}function G(N,M,Q){var R=D[D.length-1];if(Q){y+=J(R.buffer+N,R);return false}var P=q(M,R);if(P){y+=J(R.buffer+N,R);I(P,M);return P.rB}var L=v(D.length-1,M);if(L){var O=R.cN?"</span>":"";if(R.rE){y+=J(R.buffer+N,R)+O}else{if(R.eE){y+=J(R.buffer+N,R)+O+m(M)}else{y+=J(R.buffer+N+M,R)+O}}while(L>1){O=D[D.length-2].cN?"</span>":"";y+=O;L--;D.length--}var r=D[D.length-1];D.length--;D[D.length-1].buffer="";if(r.starts){I(r.starts,"")}return R.rE}if(w(M,R)){throw"Illegal"}}var E=e[B];var D=[E.dM];var A=0;var x=0;var y="";try{var s,u=0;E.dM.buffer="";do{s=p(C,u);var t=G(s[0],s[1],s[2]);u+=s[0].length;if(!t){u+=s[1].length}}while(!s[2]);if(D.length>1){throw"Illegal"}return{r:A,keyword_count:x,value:y}}catch(H){if(H=="Illegal"){return{r:0,keyword_count:0,value:m(C)}}else{throw H}}}function g(t){var p={keyword_count:0,r:0,value:m(t)};var r=p;for(var q in e){if(!e.hasOwnProperty(q)){continue}var s=d(q,t);s.language=q;if(s.keyword_count+s.r>r.keyword_count+r.r){r=s}if(s.keyword_count+s.r>p.keyword_count+p.r){r=p;p=s}}if(r.language){p.second_best=r}return p}function i(r,q,p){if(q){r=r.replace(/^((<[^>]+>|\t)+)/gm,function(t,w,v,u){return w.replace(/\t/g,q)})}if(p){r=r.replace(/\n/g,"<br>")}return r}function n(t,w,r){var x=h(t,r);var v=a(t);var y,s;if(v){y=d(v,x)}else{return}var q=c(t);if(q.length){s=document.createElement("pre");s.innerHTML=y.value;y.value=k(q,c(s),x)}y.value=i(y.value,w,r);var u=t.className;if(!u.match("(\\s|^)(language-)?"+v+"(\\s|$)")){u=u?(u+" "+v):v}if(/MSIE [678]/.test(navigator.userAgent)&&t.tagName=="CODE"&&t.parentNode.tagName=="PRE"){s=t.parentNode;var p=document.createElement("div");p.innerHTML="<pre><code>"+y.value+"</code></pre>";t=p.firstChild.firstChild;p.firstChild.cN=s.cN;s.parentNode.replaceChild(p.firstChild,s)}else{t.innerHTML=y.value}t.className=u;t.result={language:v,kw:y.keyword_count,re:y.r};if(y.second_best){t.second_best={language:y.second_best.language,kw:y.second_best.keyword_count,re:y.second_best.r}}}function o(){if(o.called){return}o.called=true;var r=document.getElementsByTagName("pre");for(var p=0;p<r.length;p++){var q=b(r[p]);if(q){n(q,hljs.tabReplace)}}}function l(){if(window.addEventListener){window.addEventListener("DOMContentLoaded",o,false);window.addEventListener("load",o,false)}else{if(window.attachEvent){window.attachEvent("onload",o)}else{window.onload=o}}}var e={};this.LANGUAGES=e;this.highlight=d;this.highlightAuto=g;this.fixMarkup=i;this.highlightBlock=n;this.initHighlighting=o;this.initHighlightingOnLoad=l;this.IR="[a-zA-Z][a-zA-Z0-9_]*";this.UIR="[a-zA-Z_][a-zA-Z0-9_]*";this.NR="\\b\\d+(\\.\\d+)?";this.CNR="\\b(0[xX][a-fA-F0-9]+|(\\d+(\\.\\d*)?|\\.\\d+)([eE][-+]?\\d+)?)";this.BNR="\\b(0b[01]+)";this.RSR="!|!=|!==|%|%=|&|&&|&=|\\*|\\*=|\\+|\\+=|,|\\.|-|-=|/|/=|:|;|<|<<|<<=|<=|=|==|===|>|>=|>>|>>=|>>>|>>>=|\\?|\\[|\\{|\\(|\\^|\\^=|\\||\\|=|\\|\\||~";this.ER="(?![\\s\\S])";this.BE={b:"\\\\.",r:0};this.ASM={cN:"string",b:"'",e:"'",i:"\\n",c:[this.BE],r:0};this.QSM={cN:"string",b:'"',e:'"',i:"\\n",c:[this.BE],r:0};this.CLCM={cN:"comment",b:"//",e:"$"};this.CBLCLM={cN:"comment",b:"/\\*",e:"\\*/"};this.HCM={cN:"comment",b:"#",e:"$"};this.NM={cN:"number",b:this.NR,r:0};this.CNM={cN:"number",b:this.CNR,r:0};this.BNM={cN:"number",b:this.BNR,r:0};this.inherit=function(r,s){var p={};for(var q in r){p[q]=r[q]}if(s){for(var q in s){p[q]=s[q]}}return p}}();hljs.LANGUAGES.cpp=function(){var a={keyword:{"false":1,"int":1,"float":1,"while":1,"private":1,"char":1,"catch":1,"export":1,virtual:1,operator:2,sizeof:2,dynamic_cast:2,typedef:2,const_cast:2,"const":1,struct:1,"for":1,static_cast:2,union:1,namespace:1,unsigned:1,"long":1,"throw":1,"volatile":2,"static":1,"protected":1,bool:1,template:1,mutable:1,"if":1,"public":1,friend:2,"do":1,"return":1,"goto":1,auto:1,"void":2,"enum":1,"else":1,"break":1,"new":1,extern:1,using:1,"true":1,"class":1,asm:1,"case":1,typeid:1,"short":1,reinterpret_cast:2,"default":1,"double":1,register:1,explicit:1,signed:1,typename:1,"try":1,"this":1,"switch":1,"continue":1,wchar_t:1,inline:1,"delete":1,alignof:1,char16_t:1,char32_t:1,constexpr:1,decltype:1,noexcept:1,nullptr:1,static_assert:1,thread_local:1,restrict:1,_Bool:1,complex:1},built_in:{std:1,string:1,cin:1,cout:1,cerr:1,clog:1,stringstream:1,istringstream:1,ostringstream:1,auto_ptr:1,deque:1,list:1,queue:1,stack:1,vector:1,map:1,set:1,bitset:1,multiset:1,multimap:1,unordered_set:1,unordered_map:1,unordered_multiset:1,unordered_multimap:1,array:1,shared_ptr:1}};return{dM:{k:a,i:"</",c:[hljs.CLCM,hljs.CBLCLM,hljs.QSM,{cN:"string",b:"'\\\\?.",e:"'",i:"."},{cN:"number",b:"\\b(\\d+(\\.\\d*)?|\\.\\d+)(u|U|l|L|ul|UL|f|F)"},hljs.CNM,{cN:"preprocessor",b:"#",e:"$"},{cN:"stl_container",b:"\\b(deque|list|queue|stack|vector|map|set|bitset|multiset|multimap|unordered_map|unordered_set|unordered_multiset|unordered_multimap|array)\\s*<",e:">",k:a,r:10,c:["self"]}]}}}();hljs.LANGUAGES.r={dM:{c:[hljs.HCM,{cN:"number",b:"\\b0[xX][0-9a-fA-F]+[Li]?\\b",e:hljs.IMMEDIATE_RE,r:0},{cN:"number",b:"\\b\\d+(?:[eE][+\\-]?\\d*)?L\\b",e:hljs.IMMEDIATE_RE,r:0},{cN:"number",b:"\\b\\d+\\.(?!\\d)(?:i\\b)?",e:hljs.IMMEDIATE_RE,r:1},{cN:"number",b:"\\b\\d+(?:\\.\\d*)?(?:[eE][+\\-]?\\d*)?i?\\b",e:hljs.IMMEDIATE_RE,r:0},{cN:"number",b:"\\.\\d+(?:[eE][+\\-]?\\d*)?i?\\b",e:hljs.IMMEDIATE_RE,r:1},{cN:"keyword",b:"(?:tryCatch|library|setGeneric|setGroupGeneric)\\b",e:hljs.IMMEDIATE_RE,r:10},{cN:"keyword",b:"\\.\\.\\.",e:hljs.IMMEDIATE_RE,r:10},{cN:"keyword",b:"\\.\\.\\d+(?![\\w.])",e:hljs.IMMEDIATE_RE,r:10},{cN:"keyword",b:"\\b(?:function)",e:hljs.IMMEDIATE_RE,r:2},{cN:"keyword",b:"(?:if|in|break|next|repeat|else|for|return|switch|while|try|stop|warning|require|attach|detach|source|setMethod|setClass)\\b",e:hljs.IMMEDIATE_RE,r:1},{cN:"literal",b:"(?:NA|NA_integer_|NA_real_|NA_character_|NA_complex_)\\b",e:hljs.IMMEDIATE_RE,r:10},{cN:"literal",b:"(?:NULL|TRUE|FALSE|T|F|Inf|NaN)\\b",e:hljs.IMMEDIATE_RE,r:1},{cN:"identifier",b:"[a-zA-Z.][a-zA-Z0-9._]*\\b",e:hljs.IMMEDIATE_RE,r:0},{cN:"operator",b:"<\\-(?!\\s*\\d)",e:hljs.IMMEDIATE_RE,r:2},{cN:"operator",b:"\\->|<\\-",e:hljs.IMMEDIATE_RE,r:1},{cN:"operator",b:"%%|~",e:hljs.IMMEDIATE_RE},{cN:"operator",b:">=|<=|==|!=|\\|\\||&&|=|\\+|\\-|\\*|/|\\^|>|<|!|&|\\||\\$|:",e:hljs.IMMEDIATE_RE,r:0},{cN:"operator",b:"%",e:"%",i:"\\n",r:1},{cN:"identifier",b:"`",e:"`",r:0},{cN:"string",b:'"',e:'"',c:[hljs.BE],r:0},{cN:"string",b:"'",e:"'",c:[hljs.BE],r:0},{cN:"paren",b:"[[({\\])}]",e:hljs.IMMEDIATE_RE,r:0}]}};
hljs.initHighlightingOnLoad();
</script>

<!-- MathJax scripts -->
<script type="text/javascript" src="https://cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-MML-AM_CHTML">
</script>


<style type="text/css">
body, td {
   font-family: sans-serif;
   background-color: white;
   font-size: 13px;
}

body {
  max-width: 800px;
  margin: auto;
  padding: 1em;
  line-height: 20px;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a:visited {
   color: rgb(50%, 0%, 50%);
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: none;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>



</head>

<body>
<!-- TABLE OF CONTENTS -->

<p><aside>
  <div id="toc_container">
  <p class="toc_title">Contents</p>
  <ul class="toc_list">
    <a href="#repeated-measures-anova-in-r">Repeated measures ANOVA in R</a><br>
    <a href="#checking-assumptions">Checking assumptions</a><br>
       <a href="#sample-size">&emsp;Sample size</a><br>
       <a href="#outliers">&emsp;Outliers</a><br>
       <a href="#normality">&emsp;Normality</a><br>
       <a href="#correlations">&emsp;Correlations</a><br>
       <a href="#heteroskedasticity">&emsp;Heteroskedasticity</a><br>
    <a href="#one-way-manova">One-way MANOVA</a><br>
      <a href="#post-hoc-tests">&emsp;Post-hoc tests</a><br>
    <a href="#two-way-manova">Two-way MANOVA</a><br>
      <a href="#post-hoc2">&emsp;Post-hoc tests</a><br>
  </ul>
  </div>
</aside></p>

<h1><a href="https://repub.github.io/DLC_statistical_guides/docs/Info/repeated-measures-ANOVA">Repeated measures ANOVA</a> in <code>R</code></h1>

<p>In this guide we will be using a data set provided by <a href="https://online.stat.psu.edu/stat510/lesson/10/10.1">STAT 510 - Applied Time Series Analysis</a>, which contains observations from an experiment designed to detect phlebitis (venous inflammation) by measuring temperature during intravenous administration of a drug of interest in the ear of animals over time.  The data set has four variables:</p>

<ul>
<li><code>Animal</code> - the unique identifier for the 15 animals tested.</li>
<li><code>Treatment</code> - the treatment that was administered.</li>
<li><code>Time</code> - the timepoint in minutes when the temperature was recorded.</li>
<li><code>Y</code> - the difference in temperature between the treated and untreated ears of the animal.</li>
</ul>

<!-- Convert treatment to factor  -->

<!-- ```{r} -->

<!-- library(tidyverse) -->

<!-- phleb <- phleb %>% &ndash;>

<!--   mutate(Treatment = recode_factor(Treatment, -->

<!--                                    `1` = "drug", -->

<!--                                    `2` = "carrier", -->

<!--                                    `3` = "saline")) -->

<!-- phleb -->

<!-- write_csv(phleb, "../../dat/phlebitis.csv") -->

<!-- ``` -->

<p>First, we will load the data set using <code>read.csv()</code> and assign the resulting data frame to <code>phelb</code>, then we will use the <code>head()</code> and <code>str()</code> functions to take a peak at the data and its structure.</p>

<pre><code class="r">phleb &lt;- read.csv(&quot;../../dat/phlebitis.csv&quot;)

head(phleb)
str(phleb)
</code></pre>

<pre><code>##   Animal Treatment Time    Y
## 1      1      drug    0 -0.3
## 2      1      drug   30 -0.2
## 3      1      drug   60  1.2
## 4      1      drug   90  3.1
## 5      2      drug    0 -0.5
## 6      2      drug   30  2.2
## &#39;data.frame&#39;:    60 obs. of  4 variables:
##  $ Animal   : int  1 1 1 1 2 2 2 2 3 3 ...
##  $ Treatment: chr  &quot;drug&quot; &quot;drug&quot; &quot;drug&quot; &quot;drug&quot; ...
##  $ Time     : int  0 30 60 90 0 30 60 90 0 30 ...
##  $ Y        : num  -0.3 -0.2 1.2 3.1 -0.5 2.2 3.3 3.7 -1.1 2.4 ...
</code></pre>

<pre><code class="r">phleb$Treatment &lt;- factor(phleb$Treatment)
phleb$Animal &lt;- factor(phleb$Animal)
</code></pre>

<!-- For *Time*, since there are four time points we could also code it as a factor or keep it as numeric -->

<h2>Linear mixed effect (LME) approach</h2>

<h3>Time as a numeric variable</h3>

<p>Although the repeated measures ANOVA can be performed in base <code>R</code>, to reduce complexity we will instead use the <code>lme4</code>package, which contains functions built around mixed-effect modeling.  Additionally, we will load the <code>lmerTest</code> package which expands on the <code>lme4</code> package by including more readable statistical output.</p>

<pre><code class="r">library(lme4)
library(lmerTest)
library(car)
</code></pre>

<p>Each animal was only given one of the three treatments which was measured over four time points.  Therefore, the repeated measures are <code>Time</code> within <code>Animal</code>.  From the <code>lme4</code> package we will use the <code>lmer()</code> function to fit our model, in which we will code a model formula with the response varaible <em>Y</em> on the left side of a <code>~</code> and the predictor variables on thr right side.  In the predictor variables we will include <em>Treatment</em> and <em>Time</em> with their interaction.  To include our repeated measure, we will add <code>(1 | Animal)</code> to the model formula to indicate that measurements were repeated on each animal and is therefore our random effect.  We will then use the <code>anova()</code> function to print out the resulting ANOVA table.</p>

<pre><code class="r">phleb_lmer &lt;- lmer(Y ~ Treatment * Time + (1 | Animal), data = phleb)

Anova(phleb_lmer, type = 3)
</code></pre>

<pre><code>## Analysis of Deviance Table (Type III Wald chisquare tests)
## 
## Response: Y
##                  Chisq Df Pr(&gt;Chisq)   
## (Intercept)     3.9066  1   0.048096 * 
## Treatment       2.5515  2   0.279221   
## Time            2.0621  1   0.150997   
## Treatment:Time 13.2946  2   0.001298 **
## ---
## Signif. codes:  0 &#39;***&#39; 0.001 &#39;**&#39; 0.01 &#39;*&#39; 0.05 &#39;.&#39; 0.1 &#39; &#39; 1
</code></pre>

<p>From the results there is clearly a statistically significant interaction between <em>Treatment</em> and <em>Time</em>, indicating that the difference in ear temperature (<em>Y</em>) changes differently between at least some of the treatments and how those temperatures vary over time is also different for at least some of the treatments.  To visualize these variations we can use the <code>with()</code> and <code>interaction.plot()</code> functions to plot the differences in ear temperature over time for each treatment.</p>

<pre><code class="r">with(phleb,
     interaction.plot(Time, Treatment, Y))
</code></pre>

<p><img src="data:image/png;base64,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" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" style="display: block; margin: auto;" /></p>

<p>From the interaction plot it appears that both saline and carrier have somewhat similar responses, while the drug increases the mean difference in ear temperature over time.  However, as we have multiple comparisons to make here the results above of a significant interaction term do not inform us of exactly what comparisons are statistically different or not.  To make these pairwise comparisons we can use the <code>emmeans()</code> package.</p>

<pre><code class="r">library(emmeans)
</code></pre>

<pre><code class="r">phleb_emm &lt;- emmeans(phleb_lmer, list(pairwise ~ Treatment * Time), adjust = &quot;tukey&quot;)

phleb_emm$`pairwise differences of Treatment, Time`
</code></pre>

<pre><code>##  1                              estimate    SE df t.ratio p.value
##  carrier Time45 - drug Time45      -1.69 0.302 12  -5.597  0.0003
##  carrier Time45 - saline Time45    -0.13 0.302 12  -0.431  0.9037
##  drug Time45 - saline Time45        1.56 0.302 12   5.166  0.0006
## 
## Degrees-of-freedom method: kenward-roger 
## P value adjustment: tukey method for comparing a family of 3 estimates
</code></pre>

<p>From the results of the Tukey&#39;s pairwise comparisons we can see that the drug treatment is significantly higher than both the carrier and saline treatments, of which are not statistically different from one another.  However, it is important to note that since we used <em>Time</em> as a continuous variable this comparison was made over the average of the time measures and not for each time point.  If we are interested at what time points the differences in ear temperature vary then we should code <em>Time</em> as a factor.</p>

<h3>Time as a factor variable</h3>

<p>Now we will perform the same analysis but with <em>Time</em> as a factor rather than a continuous variable.  We will first use <code>factor()</code> to reassign <em>Time</em> then use the same code as above to fit and display the results of the repeated measures ANOVA.</p>

<pre><code class="r">phleb_lmer &lt;- lmer(Y ~ Treatment * factor(Time) + (1 | Animal), data = phleb)

Anova(phleb_lmer, type = 3)
</code></pre>

<pre><code>## Analysis of Deviance Table (Type III Wald chisquare tests)
## 
## Response: Y
##                          Chisq Df Pr(&gt;Chisq)   
## (Intercept)             2.5421  1    0.11085   
## Treatment               0.8040  2    0.66897   
## factor(Time)            2.3690  3    0.49943   
## Treatment:factor(Time) 17.4003  6    0.00792 **
## ---
## Signif. codes:  0 &#39;***&#39; 0.001 &#39;**&#39; 0.01 &#39;*&#39; 0.05 &#39;.&#39; 0.1 &#39; &#39; 1
</code></pre>

<p>Note that from the results we again have a statistically significant interaction term.  However, it also does not give us information regarding which comparisons in <em>Treatment</em> and <em>Time</em> are statistically different, so we will again employ the <code>emmeans()</code> function to test these pairwise comparisons.</p>

<pre><code class="r">phleb_emm &lt;- emmeans(phleb_lmer, list(pairwise ~ Treatment * factor(Time)), adjust = &quot;tukey&quot;)

phleb_emm$`pairwise differences of Treatment, Time`
</code></pre>

<pre><code>##  1                               estimate    SE   df t.ratio p.value
##  carrier Time0 - drug Time0         -0.34 0.514 45.8  -0.661  0.9999
##  carrier Time0 - saline Time0        0.10 0.514 45.8   0.194  1.0000
##  carrier Time0 - carrier Time30     -0.06 0.481 36.0  -0.125  1.0000
##  carrier Time0 - drug Time30        -1.96 0.514 45.8  -3.810  0.0188
##  carrier Time0 - saline Time30      -0.82 0.514 45.8  -1.594  0.9025
##  carrier Time0 - carrier Time60     -0.52 0.481 36.0  -1.081  0.9937
##  carrier Time0 - drug Time60        -2.46 0.514 45.8  -4.782  0.0010
##  carrier Time0 - saline Time60      -0.16 0.514 45.8  -0.311  1.0000
##  carrier Time0 - carrier Time90     -0.58 0.481 36.0  -1.206  0.9850
##  carrier Time0 - drug Time90        -3.16 0.514 45.8  -6.142  &lt;.0001
##  carrier Time0 - saline Time90      -0.80 0.514 45.8  -1.555  0.9160
##  drug Time0 - saline Time0           0.44 0.514 45.8   0.855  0.9993
##  drug Time0 - carrier Time30         0.28 0.514 45.8   0.544  1.0000
##  drug Time0 - drug Time30           -1.62 0.481 36.0  -3.368  0.0668
##  drug Time0 - saline Time30         -0.48 0.514 45.8  -0.933  0.9983
##  drug Time0 - carrier Time60        -0.18 0.514 45.8  -0.350  1.0000
##  drug Time0 - drug Time60           -2.12 0.481 36.0  -4.408  0.0044
##  drug Time0 - saline Time60          0.18 0.514 45.8   0.350  1.0000
##  drug Time0 - carrier Time90        -0.24 0.514 45.8  -0.467  1.0000
##  drug Time0 - drug Time90           -2.82 0.481 36.0  -5.863  0.0001
##  drug Time0 - saline Time90         -0.46 0.514 45.8  -0.894  0.9989
##  saline Time0 - carrier Time30      -0.16 0.514 45.8  -0.311  1.0000
##  saline Time0 - drug Time30         -2.06 0.514 45.8  -4.004  0.0108
##  saline Time0 - saline Time30       -0.92 0.481 36.0  -1.913  0.7439
##  saline Time0 - carrier Time60      -0.62 0.514 45.8  -1.205  0.9859
##  saline Time0 - drug Time60         -2.56 0.514 45.8  -4.976  0.0005
##  saline Time0 - saline Time60       -0.26 0.481 36.0  -0.541  1.0000
##  saline Time0 - carrier Time90      -0.68 0.514 45.8  -1.322  0.9718
##  saline Time0 - drug Time90         -3.26 0.514 45.8  -6.337  &lt;.0001
##  saline Time0 - saline Time90       -0.90 0.481 36.0  -1.871  0.7682
##  carrier Time30 - drug Time30       -1.90 0.514 45.8  -3.693  0.0258
##  carrier Time30 - saline Time30     -0.76 0.514 45.8  -1.477  0.9393
##  carrier Time30 - carrier Time60    -0.46 0.481 36.0  -0.956  0.9978
##  carrier Time30 - drug Time60       -2.40 0.514 45.8  -4.665  0.0015
##  carrier Time30 - saline Time60     -0.10 0.514 45.8  -0.194  1.0000
##  carrier Time30 - carrier Time90    -0.52 0.481 36.0  -1.081  0.9937
##  carrier Time30 - drug Time90       -3.10 0.514 45.8  -6.026  &lt;.0001
##  carrier Time30 - saline Time90     -0.74 0.514 45.8  -1.438  0.9491
##  drug Time30 - saline Time30         1.14 0.514 45.8   2.216  0.5472
##  drug Time30 - carrier Time60        1.44 0.514 45.8   2.799  0.2144
##  drug Time30 - drug Time60          -0.50 0.481 36.0  -1.040  0.9955
##  drug Time30 - saline Time60         1.80 0.514 45.8   3.499  0.0432
##  drug Time30 - carrier Time90        1.38 0.514 45.8   2.682  0.2673
##  drug Time30 - drug Time90          -1.20 0.481 36.0  -2.495  0.3746
##  drug Time30 - saline Time90         1.16 0.514 45.8   2.255  0.5211
##  saline Time30 - carrier Time60      0.30 0.514 45.8   0.583  1.0000
##  saline Time30 - drug Time60        -1.64 0.514 45.8  -3.188  0.0929
##  saline Time30 - saline Time60       0.66 0.481 36.0   1.372  0.9617
##  saline Time30 - carrier Time90      0.24 0.514 45.8   0.467  1.0000
##  saline Time30 - drug Time90        -2.34 0.514 45.8  -4.548  0.0021
##  saline Time30 - saline Time90       0.02 0.481 36.0   0.042  1.0000
##  carrier Time60 - drug Time60       -1.94 0.514 45.8  -3.771  0.0209
##  carrier Time60 - saline Time60      0.36 0.514 45.8   0.700  0.9999
##  carrier Time60 - carrier Time90    -0.06 0.481 36.0  -0.125  1.0000
##  carrier Time60 - drug Time90       -2.64 0.514 45.8  -5.132  0.0003
##  carrier Time60 - saline Time90     -0.28 0.514 45.8  -0.544  1.0000
##  drug Time60 - saline Time60         2.30 0.514 45.8   4.471  0.0027
##  drug Time60 - carrier Time90        1.88 0.514 45.8   3.654  0.0287
##  drug Time60 - drug Time90          -0.70 0.481 36.0  -1.455  0.9431
##  drug Time60 - saline Time90         1.66 0.514 45.8   3.227  0.0848
##  saline Time60 - carrier Time90     -0.42 0.514 45.8  -0.816  0.9995
##  saline Time60 - drug Time90        -3.00 0.514 45.8  -5.831  &lt;.0001
##  saline Time60 - saline Time90      -0.64 0.481 36.0  -1.331  0.9691
##  carrier Time90 - drug Time90       -2.58 0.514 45.8  -5.015  0.0005
##  carrier Time90 - saline Time90     -0.22 0.514 45.8  -0.428  1.0000
##  drug Time90 - saline Time90         2.36 0.514 45.8   4.587  0.0019
## 
## Degrees-of-freedom method: kenward-roger 
## P value adjustment: tukey method for comparing a family of 12 estimates
</code></pre>

<p>After setting <em>Time</em> to a factor and looking at all pairwise comparisons at each of the 4 levels of <em>Time</em> and 3 levels of <em>Treatment</em> we have a number of comparisons equal to:</p>

<p>\[\frac{k_{Time}*k_{Treatment}*(k_{Time}*k_{Treatment}-1)}{2} = 66\]</p>

<h2>Autoregressive model of order 1 (AR1) approach</h2>

<p>With some experiments where the response is expected to change over time we may also make the assumption that those changes per time point rely on their values at the previous timepoint.  In these cases we may want to use an AR(1) structured model to account for measurements at the previous time point.</p>

<p>To do so, we will need to employ another library, <code>nlme</code>, to fit a linear model using generalized least squares that includes the AR(1) component.  We will use the <code>gls()</code> function to fit this model with a similar model formula as above, however we will add the random effects component in <code>corAR1()</code> to specifiy the AR(1) structure for the model.  We then use the <code>anova()</code> function to print a summary of the results.</p>

<pre><code class="r">library(nlme)

phleb_gls &lt;- gls(Y ~ Treatment * Time, data = phleb, correlation = corAR1(form = ~ 1 | Animal))

anova(phleb_gls)

Anova(phleb_gls, type = 3, idata = phleb, imatrix = phleb, idesign = ~ Treatment * Time, phleb = phleb)
</code></pre>

<pre><code>## Denom. DF: 54 
##                numDF   F-value p-value
## (Intercept)        1  5.744409  0.0200
## Treatment          2 17.862772  &lt;.0001
## Time               1 21.527038  &lt;.0001
## Treatment:Time     2  5.502033  0.0067
## Analysis of Deviance Table (Type III tests)
## 
## Response: Y
##                Df   Chisq Pr(&gt;Chisq)   
## (Intercept)     1  3.2672   0.070677 . 
## Treatment       2  1.9336   0.380289   
## Time            1  1.5463   0.213678   
## Treatment:Time  2 11.0041   0.004078 **
## ---
## Signif. codes:  0 &#39;***&#39; 0.001 &#39;**&#39; 0.01 &#39;*&#39; 0.05 &#39;.&#39; 0.1 &#39; &#39; 1
</code></pre>

<p>From the ANOVA table above we see again that there is a statistically significant interaction between <em>Treatment</em> and <em>Time</em>.  To test which groups vary from one another we will again employ the <code>emmeans()</code> function to perform Tukey-adjusted pairwise comparisons.</p>

<pre><code class="r">phleb_emm &lt;- emmeans(phleb_gls, list(pairwise ~ Treatment * Time), adjust = &quot;tukey&quot;, data = phleb)

phleb_emm$`pairwise differences of Treatment, Time`
</code></pre>

<pre><code>##  1                              estimate    SE   df t.ratio p.value
##  carrier Time45 - drug Time45      -1.66 0.309 18.4  -5.360  0.0001
##  carrier Time45 - saline Time45    -0.12 0.309 18.4  -0.389  0.9205
##  drug Time45 - saline Time45        1.54 0.309 18.4   4.971  0.0003
## 
## Degrees-of-freedom method: satterthwaite 
## P value adjustment: tukey method for comparing a family of 3 estimates
</code></pre>

<p>Similar to the LME model above the drug treatment causes higher differences in ear temperature on average compared to the carrier and saline treatments which are not statistically different from one another.  </p>

</body>

</html>
