# Lightweight reactivity for Unity

# Installation

  * Via .unitypackage file:

    Download package from [releases page](https://github.com/laphedhendad/Lightweight-Reactivity/releases).
    Add to project with Assets/Import Package/Custom Package...
    
  * Via git URL:

    Open Window/Package Manager and choose +/Add package from git URL...
    
    Set `https://github.com/laphedhendad/Lightweight-Reactivity.git` as URL.

    If you want to set a target version, UI Framework uses the \*.\*.\* release tag so you can specify a version like #1.0.0. For example `https://github.com/laphedhendad/Lightweight-Reactivity.git#1.0.0`.

# Documentation

The interaction between interface and data is built on a reactive approach. The package implements reactive property, collection and dictionary. They are lightweight versions of the corresponding classes from the [UniRx](https://github.com/neuecc/UniRx) plugin.

## ReactiveProperty

Reactive Property - a simple wrapper around data types. Triggers the OnChanged event when its value changes.

```csharp
  public class ReactiveProperty<T>: IReactiveProperty<T>
  {
      public event Action OnChanged;
      private T currentValue;

      public T Value
      {
          get => currentValue;
          set
          {
              if (EqualityComparer<T>.Default.Equals(value, currentValue)) return;
              currentValue = value;
              OnChanged?.Invoke();
          }
      }
  }
```

## ReactiveCollection

Reactive Collection - a wrapper around a type [Collection](https://learn.microsoft.com/ru-ru/dotnet/api/system.collections.objectmodel.collection-1). Triggers events:

* OnChanged
* OnAdd
* OnRemove
* OnReplace
* OnCleared

The events pass the index of the changed element as parameters.

## ReactiveDictionary

Reactive Dictionary - a wrapper around a type [Dictionary](https://learn.microsoft.com/ru-ru/dotnet/api/system.collections.generic.dictionary-2). Triggers events:

* OnChanged
* OnAdd
* OnRemove
* OnReplace
* OnCleared

The events pass the key of the changed element as parameters.

## Example

```csharp
  //booster class with reactive property
  public class Booster: IBooster
  {
      public IReactiveProperty<int> Amount { get; } = new ReactiveProperty<int>();
  }

  ...

  //subscribing to a reactive property
  public override void SubscribeModel(TReactive model)
  {
      if (model == null) return;
      this.model = model;
      model.OnChanged += HandleModelUpdate;
  }

  ...

  //changing value of a reactive property
  private void BuyBooster()
  {
      booster.Amount.Value++;
  }
```
