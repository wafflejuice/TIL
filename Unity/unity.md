# 2022-09-10
## Component.GetComponentInParent searches in itself, too.
`Component.GetComponentInParent` finds the component **in the GameObject itself** or its parents.
> Returns the Component of type in the GameObject or any of its parents.
[source: Unity doc](https://docs.unity3d.com/kr/2021.3/ScriptReference/Component.GetComponentInParent.html)

`Component.GetComponentInChildren` works similarly.

## Change the size of RectTransform
```C#
rectTransform = transform.GetComponent<RectTransform>();
rectTransform.sizeDelta = new Vector2(width, height);
```
