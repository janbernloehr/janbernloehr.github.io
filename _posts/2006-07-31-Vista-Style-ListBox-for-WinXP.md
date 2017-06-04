---
layout: post
title : "Vista-Style ListBox for WinXP"
date : 31.07.2006 17:02:00
tags: [wpf, expression]
---
{% include JB/setup %}

Since the WPF Style engine is very flexible you can easily achieve a Vista-like style for ListBoxes. 

![A vista-style ListBox in WPF](http://www.dev-jc-vb.de/dev-jc-vb/blog/images/Vista-stlye-Listbox-hover.PNG)

Xaml Code: 

``` xml

<ListBox> 

<ListBox.ItemContainerStyle> 

<Style TargetType="{x:Type ListBoxItem}"> 

<Setter Property="Background" Value="#00FFFFFF"/> 

<Setter Property="HorizontalContentAlignment" Value="{Binding HorizontalContentAlignment, RelativeSource={RelativeSource AncestorLevel=1, Mode=FindAncestor, AncestorType={x:Type ItemsControl}}}"/> 

<Setter Property="VerticalContentAlignment" Value="{Binding VerticalContentAlignment, RelativeSource={RelativeSource AncestorLevel=1, Mode=FindAncestor, AncestorType={x:Type ItemsControl}}}"/> 




<Setter Property="Margin" Value="0,0,0,1" /> 

<Setter Property="Template"> 

<Setter.Value> 

<ControlTemplate TargetType="{x:Type ListBoxItem}"> 

<Grid> 


<Rectangle SnapsToDevicePixels="True" Fill="{TemplateBinding Background}" Stroke="{TemplateBinding BorderBrush}" RadiusX="4" RadiusY="4" HorizontalAlignment="Stretch" VerticalAlignment="Stretch" Width="Auto" Height="Auto" x:Name="Rectangle" /> 



<ContentPresenter Margin="10" HorizontalAlignment="{TemplateBinding HorizontalContentAlignment}" VerticalAlignment="{TemplateBinding VerticalContentAlignment}" x:Name="ContentPresenter" SnapsToDevicePixels="{TemplateBinding SnapsToDevicePixels}" Content="{TemplateBinding Content}" ContentTemplate="{TemplateBinding ContentTemplate}"/> 

</Grid> 

<ControlTemplate.Triggers> 

<Trigger Property="IsMouseOver" Value="True"> 

<Setter Property="Background"> 

<Setter.Value> 


<LinearGradientBrush StartPoint="0,0" EndPoint="0,1"> 



<GradientStop Color="sc#1, 0.813324332, 0.9436713, 1" Offset="0.032051282051282048"/> 

<GradientStop Color="sc#1, 0.7551608, 0.8902375, 0.999111533" Offset="1"/> 

</LinearGradientBrush> 




</Setter.Value> 

</Setter> 

<Setter Property="BorderBrush" Value="sc#1, 0.413786, 0.7415289, 0.917951047"/> 

</Trigger> 




<Trigger Property="IsSelected" Value="True"> 

<Setter Property="Background"> 

<Setter.Value> 


<LinearGradientBrush StartPoint="0,0" EndPoint="0,1"> 



<GradientStop Color="sc#1, 0.7241676, 0.916768551, 1" Offset="0.032051282051282048"/> 

<GradientStop Color="sc#1, 0.5893368, 0.8144646, 0.995921433" Offset="1"/> 

</LinearGradientBrush> 




</Setter.Value> 

</Setter> 

<Setter Property="BorderBrush" Value="sc#1, 0.193802863, 0.6061246, 0.828075051"/> 

</Trigger> 

<MultiTrigger> 

<MultiTrigger.Conditions> 

<Condition Property="IsSelected" Value="True"/> 

<Condition Property="Selector.IsSelectionActive" Value="False"/> 

</MultiTrigger.Conditions> 

<Setter Property="Background"> 

<Setter.Value> 


<LinearGradientBrush StartPoint="0,0" EndPoint="0,1"> 



<GradientStop Color="sc#1, 0.7241676, 0.916768551, 1" Offset="0.032051282051282048"/> 

<GradientStop Color="sc#1, 0.5893368, 0.8144646, 0.995921433" Offset="1"/> 

</LinearGradientBrush> 




</Setter.Value> 

</Setter> 

<Setter Property="BorderBrush" Value="sc#1, 0.193802863, 0.6061246, 0.828075051"/> 

</MultiTrigger> 

</ControlTemplate.Triggers> 

</ControlTemplate> 

</Setter.Value> 

</Setter> 

</Style> 

</ListBox.ItemContainerStyle> 




<ListBoxItem Content="ListBoxItem1" /> 

<ListBoxItem Content="ListBoxItem2" /> 

<ListBoxItem Content="ListBoxItem3" /> 

</ListBox> 

```