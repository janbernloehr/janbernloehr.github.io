---
layout: post
title : "Sql Server ExceptionMessageBox in WPF nutzen"
date : 22.03.2009 13:27:30
tags: [.NET, WPF, Visual Studio 2008]
---
{% include JB / setup %}

Wer das Microsoft Sql Management Studio verwendet kennt sie vielleicht, die ExceptionMessageBox des Sql Servers. Sie bietet eine gute Übersicht über alle relevanten Informationen einer Exception und das Beste ist, dass sie als Assembly in jedes .NET Projekt eingebunden und damit in jeder .NET Anwendung angezeigt werden kann.

[![image](http://www.vb-magazin.de/forums/blogs/janm/image_thumb_3DB23A5A.png "image")](http://www.vb-magazin.de/forums/blogs/janm/image_3A346B17.png) 

Einziger Haken: Zur Anzeige der ExceptionMessageBox wird ein owner vom Typ **System.Windows.Forms.IWin32Owner** erwartet. Bewegt man sich in WPF, steht dieser einem natürlich nicht zur Verfügung. Abhilfe schafft der Typ ExceptionMessageBoxParent, da dieser mit einem gewöhnlichen Handle erstellt werden und somit als IWin32Owner verwendet werden kann.

Mit den WPF Interop Tools kann somit die ExceptionMessageBox problemlos in WPF einbinden.
  <div class="csharpcode">   

<span class="lnum">   1:  </span>    <span class="kwrd">public</span> <span class="kwrd">class</span> ExceptionMessageBoxWpf : ExceptionMessageBox

<span class="lnum">   2:  </span>    {

<span class="lnum">   3:  </span>        <span class="kwrd">public</span> ExceptionMessageBoxWpf() : <span class="kwrd">base</span>() { }

<span class="lnum">   4:  </span>        <span class="kwrd">public</span> ExceptionMessageBoxWpf(Exception exception) : <span class="kwrd">base</span>(exception) { }

<span class="lnum">   5:  </span>        <span class="kwrd">public</span> ExceptionMessageBoxWpf(<span class="kwrd">string</span> text) : <span class="kwrd">base</span>(text) { }

<span class="lnum">   6:  </span>        <span class="kwrd">public</span> ExceptionMessageBoxWpf(Exception exception, ExceptionMessageBoxButtons buttons) : <span class="kwrd">base</span>(exception, buttons) { }

<span class="lnum">   7:  </span>        <span class="kwrd">public</span> ExceptionMessageBoxWpf(<span class="kwrd">string</span> text, <span class="kwrd">string</span> caption) : <span class="kwrd">base</span>(text, caption) { }

<span class="lnum">   8:  </span>        <span class="kwrd">public</span> ExceptionMessageBoxWpf(Exception exception, ExceptionMessageBoxButtons buttons, ExceptionMessageBoxSymbol symbol) : <span class="kwrd">base</span>(exception, buttons, symbol) { }

<span class="lnum">   9:  </span>        <span class="kwrd">public</span> ExceptionMessageBoxWpf(<span class="kwrd">string</span> text, <span class="kwrd">string</span> caption, ExceptionMessageBoxButtons buttons) : <span class="kwrd">base</span>(text, caption, buttons) { }

<span class="lnum">  10:  </span>        <span class="kwrd">public</span> ExceptionMessageBoxWpf(Exception exception, ExceptionMessageBoxButtons buttons, ExceptionMessageBoxSymbol symbol, ExceptionMessageBoxDefaultButton defaultButton) : <span class="kwrd">base</span>(exception, buttons, symbol, defaultButton) { }

<span class="lnum">  11:  </span>        <span class="kwrd">public</span> ExceptionMessageBoxWpf(<span class="kwrd">string</span> text, <span class="kwrd">string</span> caption, ExceptionMessageBoxButtons buttons, ExceptionMessageBoxSymbol symbol) : <span class="kwrd">base</span>(text, caption, buttons, symbol) { }

<span class="lnum">  12:  </span>        <span class="kwrd">public</span> ExceptionMessageBoxWpf(Exception exception, ExceptionMessageBoxButtons buttons, ExceptionMessageBoxSymbol symbol, ExceptionMessageBoxDefaultButton defaultButton, ExceptionMessageBoxOptions options) : <span class="kwrd">base</span>(exception, buttons, symbol, defaultButton, options) { }

<span class="lnum">  13:  </span>        <span class="kwrd">public</span> ExceptionMessageBoxWpf(<span class="kwrd">string</span> text, <span class="kwrd">string</span> caption, ExceptionMessageBoxButtons buttons, ExceptionMessageBoxSymbol symbol, ExceptionMessageBoxDefaultButton defaultButton) : <span class="kwrd">base</span>(text, caption, buttons, symbol, defaultButton) { }

<span class="lnum">  14:  </span>        <span class="kwrd">public</span> ExceptionMessageBoxWpf(<span class="kwrd">string</span> text, <span class="kwrd">string</span> caption, ExceptionMessageBoxButtons buttons, ExceptionMessageBoxSymbol symbol, ExceptionMessageBoxDefaultButton defaultButton, ExceptionMessageBoxOptions options) : <span class="kwrd">base</span>(text, caption, buttons, symbol, defaultButton, options) { }

<span class="lnum">  15:  </span> 

<span class="lnum">  16:  </span>        <span class="kwrd">public</span> <span class="kwrd">void</span> Show(Window owner) {

<span class="lnum">  17:  </span>            <span class="rem">// the owner window is required</span>

<span class="lnum">  18:  </span>            <span class="kwrd">if</span> (owner == <span class="kwrd">null</span>) <span class="kwrd">throw</span> <span class="kwrd">new</span> ArgumentNullException(<span class="str">"owner"</span>);

<span class="lnum">  19:  </span> 

<span class="lnum">  20:  </span>            Type exbParentType;

<span class="lnum">  21:  </span>            Type exbType;

<span class="lnum">  22:  </span>            System.Windows.Interop.WindowInteropHelper helper;

<span class="lnum">  23:  </span> 

<span class="lnum">  24:  </span>            <span class="rem">// The ExceptionMessageBox Show method requires a owner which implements</span>

<span class="lnum">  25:  </span>            <span class="rem">// System.Windows.Forms.IWin32Window - the WPF Windows don't do that</span>

<span class="lnum">  26:  </span>            <span class="rem">// but we can use the internal class ExceptionMessageBoxParent</span>

<span class="lnum">  27:  </span>            <span class="rem">// the create a appropriate owner.</span>

<span class="lnum">  28:  </span>            exbParentType = Type.GetType(<span class="str">"Microsoft.SqlServer.MessageBox.ExceptionMessageBoxParent, Microsoft.ExceptionMessageBox, Version=10.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91"</span>);

<span class="lnum">  29:  </span>            exbType = <span class="kwrd">this</span>.GetType();

<span class="lnum">  30:  </span> 

<span class="lnum">  31:  </span>            <span class="rem">// WindowInteropHelper allows access to the root handle of the wpf window.</span>

<span class="lnum">  32:  </span>            helper = <span class="kwrd">new</span> System.Windows.Interop.WindowInteropHelper(owner);

<span class="lnum">  33:  </span> 

<span class="lnum">  34:  </span>            IWin32Window parent = (IWin32Window)Activator.CreateInstance(exbParentType, <span class="kwrd">new</span> <span class="kwrd">object</span>[] { helper.Handle });

<span class="lnum">  35:  </span> 

<span class="lnum">  36:  </span>            <span class="kwrd">this</span>.Show(parent);

<span class="lnum">  37:  </span>        }

<span class="lnum">  38:  </span>}

</div>

Als Parameter lässt sich somit jedes WPF Window übergeben.

<div class="csharpcode">

<span class="lnum">   1:  </span>            <span class="kwrd">try</span> {

<span class="lnum">   2:  </span>                <span class="kwrd">throw</span> <span class="kwrd">new</span> ApplicationException(<span class="str">"Hello from WPF!"</span>);

<span class="lnum">   3:  </span>            } <span class="kwrd">catch</span> (Exception ex) {

<span class="lnum">   4:  </span>                ExceptionMessageBoxWpf ebx = <span class="kwrd">new</span> ExceptionMessageBoxWpf(ex);

<span class="lnum">   5:  </span>                ebx.Show(<span class="kwrd">this</span>);

<span class="lnum">   6:  </span>                <span class="kwrd">throw</span>;

<span class="lnum">   7:  </span>            }

</div>
<style type="text/css">

.csharpcode, .csharpcode pre
{
	font-size: small;
	color: black;
	font-family: consolas, "Courier New", courier, monospace;
	background-color: #ffffff;
	/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt 
{
	background-color: #f4f4f4;
	width: 100%;
	margin: 0em;
}
.csharpcode .lnum { color: #606060; }</style>

Die ExceptionMessageBox ist jedoch erstaunlich vielseitig und lässt sich auch für Meldungen jeglicher Art verwenden (siehe [2]).

**Zum Weiterlesen**

[1] How to: Program Exception Message Box (MSDN)   
[http://msdn.microsoft.com/en-us/library/ms166340.aspx](http://msdn.microsoft.com/en-us/library/ms166340.aspx)

[2] Make your exceptions shine with SQL Server Exception Message Box   
[http://geekswithblogs.net/kobush/archive/2006/05/21/ExceptionMessageBox.aspx](http://geekswithblogs.net/kobush/archive/2006/05/21/ExceptionMessageBox.aspx)
