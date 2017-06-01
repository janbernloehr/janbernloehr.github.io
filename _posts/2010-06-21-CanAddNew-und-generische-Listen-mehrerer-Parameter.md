---
layout: post
title : "CanAddNew und generische Listen mehrerer Parameter"
date : 21.06.2010 14:42:54
tags: []
---
{% include JB / setup %}

Das WPF ListCollectionView unterstützt das Hinzufügen von Elementen bei generischen Listen in einem Parameter, wie List<T> oder ObservableCollection<T>.

Erstellt man jedoch ein View zu einer Collection der Bauart MyCollection<T,S>, unterstützt ListCollectionView lediglich ReadOnly Operationen, denn CanAddNew evaluiert zu “False”. Nach einigen Nachforschungen stellt man fest, dass CanCunstructItem zu “False” evaluiert und folglich auch CanAddNew.

Man folgendem dirty code lässt sich das Problem umgehen. In der Implementierung von ListCollectionView in .NET 4 tritt das Problem nicht auf.

public class TypedListCollectionView : ListCollectionView   
    {   
        public TypedListCollectionView(IList items, Type itemType)   
            : base(items) { 

            var itemConstructor = itemType.GetConstructor(Type.EmptyTypes); 

            Type type = typeof(ListCollectionView); 

            var itemConstructorField =   
            type.GetField("_itemConstructor", System.Reflection.BindingFlags.Instance | System.Reflection.BindingFlags.NonPublic);   
            var isItemConstructorValidField =   
            type.GetField("_isItemConstructorValid", System.Reflection.BindingFlags.Instance | System.Reflection.BindingFlags.NonPublic); 

            itemConstructorField.SetValue(this, itemConstructor);   
            isItemConstructorValidField.SetValue(this, true);   
        }   
    }
