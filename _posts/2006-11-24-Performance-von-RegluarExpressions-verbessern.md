---
layout: post
title : "Performance von RegluarExpressions verbessern"
date : 24.11.2006 14:24:18
tags: [.net, regex]
---
{% include JB/setup %}

Im .NET Framework setllen die RegularExpressions (zu deutsch reguläre Ausdrücke) eine Vielzahl von Textoperationen bereit.

Beispiel: Format einer Email Adresse überprüfen

``` vb
Dim text As String = [someone@somecompany.com](mailto:someone@somecompany.com)

If System.Text.RegularExpressions.RegEx.IsMatch(text, "\w+([-+.']\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*")  
  ' Diese Email ist gültig  
Else  
  ' Diese Email ist ungültig  
End If
```

Jedoch müsste bei jedem Aufruf des Codes der reguläre Ausdruck bei jedem Aufruf erst interpretiert und dann angewendet werden.

Bei seltenem Aufruf bzw. dynamischem pattern ist das auch kein Problem.

Wird die Validierung jedoch sehr häufig benötigt vielleicht sogar mehrmals in der selben Prozedur, bietet es sich an den Ausdruck vorzukompilieren.

Dazu sollte man den das RegEx in einer Variablen speichern:

``` vb
Private Shared ReadOnly emailregex As New System.Text.RegularExpressions.Regex("\w+([-+.']\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*", System.Text.RegularExpressions.RegexOptions.Compiled) 
```

Der Ausdruck wird nun kompiliert und ist nun viel perfomanter bei häufiger Ausführung. 

Um zusätzlich noch Platz zu sparen und Übersichtlichkeit zu erhalten, kann man den Namespace System.Text.RegularExpressions importieren: 

``` vb
Private Shared ReadOnly emailregex As New Regex("\w+([-+.']\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*", RegexOptions.Compiled)
```