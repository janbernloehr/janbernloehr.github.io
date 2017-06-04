---
layout: post
title : "Sql Server ExceptionMessageBox in WPF nutzen"
date : 22.03.2009 13:27:30
tags: [.net, wpf, visual-studio, sql-server]
---
{% include JB/setup %}

Wer das Microsoft Sql Management Studio verwendet kennt sie vielleicht, die `ExceptionMessageBox` des Sql Servers. Sie bietet eine gute Übersicht über alle relevanten Informationen einer Exception und das Beste ist, dass sie als Assembly in jedes .NET Projekt eingebunden und damit in jeder .NET Anwendung angezeigt werden kann.

![ExceptionMessageBox](https://gwb.blob.core.windows.net/kobush/1591/o_ErrorMessageBox.png)

Einziger Haken: Zur Anzeige der ExceptionMessageBox wird ein owner vom Typ **System.Windows.Forms.IWin32Owner** erwartet. Bewegt man sich in WPF, steht dieser einem natürlich nicht zur Verfügung. Abhilfe schafft der Typ ExceptionMessageBoxParent, da dieser mit einem gewöhnlichen Handle erstellt werden und somit als IWin32Owner verwendet werden kann.

Mit den WPF Interop Tools kann somit die ExceptionMessageBox problemlos in WPF einbinden.
``` cs
public class ExceptionMessageBoxWpf : ExceptionMessageBox
{
    public ExceptionMessageBoxWpf() : base() { }
    public ExceptionMessageBoxWpf(Exception exception) : base(exception) { }
    public ExceptionMessageBoxWpf(string text) : base(text) { }
    public ExceptionMessageBoxWpf(Exception exception, ExceptionMessageBoxButtons buttons) : base(exception, buttons) { }
    public ExceptionMessageBoxWpf(string text, string caption) : base(text, caption) { }
    public ExceptionMessageBoxWpf(Exception exception, ExceptionMessageBoxButtons buttons, ExceptionMessageBoxSymbol symbol) : base(exception, buttons, symbol) { }
    public ExceptionMessageBoxWpf(string text, string caption, ExceptionMessageBoxButtons buttons) : base(text, caption, buttons) { }
    public ExceptionMessageBoxWpf(Exception exception, ExceptionMessageBoxButtons buttons, ExceptionMessageBoxSymbol symbol, ExceptionMessageBoxDefaultButton defaultButton) : base(exception, buttons, symbol, defaultButton) { }
    public ExceptionMessageBoxWpf(string text, string caption, ExceptionMessageBoxButtons buttons, ExceptionMessageBoxSymbol symbol) : base(text, caption, buttons, symbol) { }
    public ExceptionMessageBoxWpf(Exception exception, ExceptionMessageBoxButtons buttons, ExceptionMessageBoxSymbol symbol, ExceptionMessageBoxDefaultButton defaultButton, ExceptionMessageBoxOptions options) : base(exception, buttons, symbol, defaultButton, options) { }
    public ExceptionMessageBoxWpf(string text, string caption, ExceptionMessageBoxButtons buttons, ExceptionMessageBoxSymbol symbol, ExceptionMessageBoxDefaultButton defaultButton) : base(text, caption, buttons, symbol, defaultButton) { }
    public ExceptionMessageBoxWpf(string text, string caption, ExceptionMessageBoxButtons buttons, ExceptionMessageBoxSymbol symbol, ExceptionMessageBoxDefaultButton defaultButton, ExceptionMessageBoxOptions options) : base(text, caption, buttons, symbol, defaultButton, options) { }

    public void Show(Window owner) {
        // the owner window is required
        if (owner == null) throw new ArgumentNullException("owner");

        Type exbParentType;
        Type exbType;
        System.Windows.Interop.WindowInteropHelper helper;

        // The ExceptionMessageBox Show method requires a owner which implements
        // System.Windows.Forms.IWin32Window - the WPF Windows don't do that
        // but we can use the internal class ExceptionMessageBoxParent
        // the create a appropriate owner.
        exbParentType = Type.GetType("Microsoft.SqlServer.MessageBox.ExceptionMessageBoxParent, Microsoft.ExceptionMessageBox, Version=10.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91");
        exbType = this.GetType();

        // WindowInteropHelper allows access to the root handle of the wpf window.
        helper = new System.Windows.Interop.WindowInteropHelper(owner);

        IWin32Window parent = (IWin32Window)Activator.CreateInstance(exbParentType, new object[] { helper.Handle });

        this.Show(parent);
    }
}
```

Als Parameter lässt sich somit jedes WPF Window übergeben.

``` cs
try {
    throw new ApplicationException("Hello from WPF!");
} catch (Exception ex) {
    ExceptionMessageBoxWpf ebx = new ExceptionMessageBoxWpf(ex);
    ebx.Show(this);
    throw;
}
```

Die ExceptionMessageBox ist jedoch erstaunlich vielseitig und lässt sich auch für Meldungen jeglicher Art verwenden (siehe [hier](http://geekswithblogs.net/kobush/archive/2006/05/21/ExceptionMessageBox.aspx)).

**Zum Weiterlesen**

[1] [How to: Program Exception Message Box (MSDN)](http://msdn.microsoft.com/en-us/library/ms166340.aspx)

[2] [Make your exceptions shine with SQL Server Exception Message Box](http://geekswithblogs.net/kobush/archive/2006/05/21/ExceptionMessageBox.aspx)