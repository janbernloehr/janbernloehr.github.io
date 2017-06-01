---
layout: post
title : "Vista-Style ListBox for WinXP"
date : 31.07.2006 17:02:00
tags: [WPF, Expression Products]
---
{% include JB / setup %}

Since the WPF Style engine is very flexible you can easily achieve a Vista-like style for ListBoxes. 

![A vista-style ListBox in WPF](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/Vista-stlye-Listbox-hover.PNG)

Xaml Code: 

<span style="FONT-FAMILY: Courier New">

<span style="COLOR: blue"><</span><span style="COLOR: maroon">ListBox</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">ListBox.ItemContainerStyle</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Style</span><span style="COLOR: blue"> </span><span style="COLOR: red">TargetType</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">{x:Type ListBoxItem}</span>"<span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">Background</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Value</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">#00FFFFFF</span>"<span style="COLOR: blue">/> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">HorizontalContentAlignment</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Value</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">{Binding HorizontalContentAlignment, RelativeSource={RelativeSource AncestorLevel=1, Mode=FindAncestor, AncestorType={x:Type ItemsControl}}}</span>"<span style="COLOR: blue">/> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">VerticalContentAlignment</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Value</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">{Binding VerticalContentAlignment, RelativeSource={RelativeSource AncestorLevel=1, Mode=FindAncestor, AncestorType={x:Type ItemsControl}}}</span>"<span style="COLOR: blue">/> </span>
</span>

<span style="FONT-FAMILY: Courier New">

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">Margin</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Value</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">0,0,0,1</span>"<span style="COLOR: blue"> /> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">Template</span>"<span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter.Value</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">ControlTemplate</span><span style="COLOR: blue"> </span><span style="COLOR: red">TargetType</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">{x:Type ListBoxItem}</span>"<span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Grid</span><span style="COLOR: blue">> </span>
</span>

<span style="FONT-FAMILY: Courier New"><span style="COLOR: blue"><</span><span style="COLOR: maroon">Rectangle</span><span style="COLOR: blue"> </span><span style="COLOR: red">SnapsToDevicePixels</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">True</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Fill</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">{TemplateBinding Background}</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Stroke</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">{TemplateBinding BorderBrush}</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">RadiusX</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">4</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">RadiusY</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">4</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">HorizontalAlignment</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">Stretch</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">VerticalAlignment</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">Stretch</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Width</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">Auto</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Height</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">Auto</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">x:Name</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">Rectangle</span>"<span style="COLOR: blue"> /> </span></span>

<span style="FONT-FAMILY: Courier New">

<span style="COLOR: blue"><</span><span style="COLOR: maroon">ContentPresenter</span><span style="COLOR: blue"> </span><span style="COLOR: red">Margin</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">10</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">HorizontalAlignment</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">{TemplateBinding HorizontalContentAlignment}</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">VerticalAlignment</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">{TemplateBinding VerticalContentAlignment}</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">x:Name</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">ContentPresenter</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">SnapsToDevicePixels</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">{TemplateBinding SnapsToDevicePixels}</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Content</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">{TemplateBinding Content}</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">ContentTemplate</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">{TemplateBinding ContentTemplate}</span>"<span style="COLOR: blue">/> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">Grid</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">ControlTemplate.Triggers</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Trigger</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">IsMouseOver</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Value</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">True</span>"<span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">Background</span>"<span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter.Value</span><span style="COLOR: blue">> </span>
</span>

<span style="FONT-FAMILY: Courier New"><span style="COLOR: blue"><</span><span style="COLOR: maroon">LinearGradientBrush</span><span style="COLOR: blue"> </span><span style="COLOR: red">StartPoint</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">0,0</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">EndPoint</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">0,1</span>"<span style="COLOR: blue">> </span></span>

<span style="FONT-FAMILY: Courier New">

<span style="COLOR: blue"><</span><span style="COLOR: maroon">GradientStop</span><span style="COLOR: blue"> </span><span style="COLOR: red">Color</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">sc#1, 0.813324332, 0.9436713, 1</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Offset</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">0.032051282051282048</span>"<span style="COLOR: blue">/> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">GradientStop</span><span style="COLOR: blue"> </span><span style="COLOR: red">Color</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">sc#1, 0.7551608, 0.8902375, 0.999111533</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Offset</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">1</span>"<span style="COLOR: blue">/> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">LinearGradientBrush</span><span style="COLOR: blue">> </span>
</span>

<span style="FONT-FAMILY: Courier New">

<span style="COLOR: blue"></</span><span style="COLOR: maroon">Setter.Value</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">BorderBrush</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Value</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">sc#1, 0.413786, 0.7415289, 0.917951047</span>"<span style="COLOR: blue">/> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">Trigger</span><span style="COLOR: blue">> </span>
</span>

<span style="FONT-FAMILY: Courier New">

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Trigger</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">IsSelected</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Value</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">True</span>"<span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">Background</span>"<span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter.Value</span><span style="COLOR: blue">> </span>
</span>

<span style="FONT-FAMILY: Courier New"><span style="COLOR: blue"><</span><span style="COLOR: maroon">LinearGradientBrush</span><span style="COLOR: blue"> </span><span style="COLOR: red">StartPoint</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">0,0</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">EndPoint</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">0,1</span>"<span style="COLOR: blue">> </span></span>

<span style="FONT-FAMILY: Courier New">

<span style="COLOR: blue"><</span><span style="COLOR: maroon">GradientStop</span><span style="COLOR: blue"> </span><span style="COLOR: red">Color</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">sc#1, 0.7241676, 0.916768551, 1</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Offset</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">0.032051282051282048</span>"<span style="COLOR: blue">/> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">GradientStop</span><span style="COLOR: blue"> </span><span style="COLOR: red">Color</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">sc#1, 0.5893368, 0.8144646, 0.995921433</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Offset</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">1</span>"<span style="COLOR: blue">/> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">LinearGradientBrush</span><span style="COLOR: blue">> </span>
</span>

<span style="FONT-FAMILY: Courier New">

<span style="COLOR: blue"></</span><span style="COLOR: maroon">Setter.Value</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">BorderBrush</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Value</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">sc#1, 0.193802863, 0.6061246, 0.828075051</span>"<span style="COLOR: blue">/> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">Trigger</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">MultiTrigger</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">MultiTrigger.Conditions</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Condition</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">IsSelected</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Value</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">True</span>"<span style="COLOR: blue">/> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Condition</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">Selector.IsSelectionActive</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Value</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">False</span>"<span style="COLOR: blue">/> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">MultiTrigger.Conditions</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">Background</span>"<span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter.Value</span><span style="COLOR: blue">> </span>
</span>

<span style="FONT-FAMILY: Courier New"><span style="COLOR: blue"><</span><span style="COLOR: maroon">LinearGradientBrush</span><span style="COLOR: blue"> </span><span style="COLOR: red">StartPoint</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">0,0</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">EndPoint</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">0,1</span>"<span style="COLOR: blue">> </span></span>

<span style="FONT-FAMILY: Courier New">

<span style="COLOR: blue"><</span><span style="COLOR: maroon">GradientStop</span><span style="COLOR: blue"> </span><span style="COLOR: red">Color</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">sc#1, 0.7241676, 0.916768551, 1</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Offset</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">0.032051282051282048</span>"<span style="COLOR: blue">/> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">GradientStop</span><span style="COLOR: blue"> </span><span style="COLOR: red">Color</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">sc#1, 0.5893368, 0.8144646, 0.995921433</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Offset</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">1</span>"<span style="COLOR: blue">/> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">LinearGradientBrush</span><span style="COLOR: blue">> </span>
</span>

<span style="FONT-FAMILY: Courier New">

<span style="COLOR: blue"></</span><span style="COLOR: maroon">Setter.Value</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue"> </span><span style="COLOR: red">Property</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">BorderBrush</span>"<span style="COLOR: blue"> </span><span style="COLOR: red">Value</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">sc#1, 0.193802863, 0.6061246, 0.828075051</span>"<span style="COLOR: blue">/> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">MultiTrigger</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">ControlTemplate.Triggers</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">ControlTemplate</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">Setter.Value</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">Setter</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">Style</span><span style="COLOR: blue">> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">ListBox.ItemContainerStyle</span><span style="COLOR: blue">> </span>
</span>

<span style="FONT-FAMILY: Courier New">

<span style="COLOR: blue"><</span><span style="COLOR: maroon">ListBoxItem</span><span style="COLOR: blue"> </span><span style="COLOR: red">Content</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">ListBoxItem1</span>"<span style="COLOR: blue"> /> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">ListBoxItem</span><span style="COLOR: blue"> </span><span style="COLOR: red">Content</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">ListBoxItem2</span>"<span style="COLOR: blue"> /> </span>

<span style="COLOR: blue"><</span><span style="COLOR: maroon">ListBoxItem</span><span style="COLOR: blue"> </span><span style="COLOR: red">Content</span><span style="COLOR: blue">=</span>"<span style="COLOR: blue">ListBoxItem3</span>"<span style="COLOR: blue"> /> </span>

<span style="COLOR: blue"></</span><span style="COLOR: maroon">ListBox</span><span style="COLOR: blue">> </span>
</span>
