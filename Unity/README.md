## Speed Up Setting Toggle With Custom Menu Item

#### Overview

I like to use Enable '**Enter Play Mode Options**', it allows the disabling of '**Reload Domain**', which speeds up the time it takes to enter play mode when developing the game. This setting speeds up the process but comes with the cost of not resetting static variables by default, which sometimes introduces bugs.

#### The Challenge

I frequently encountered bugs that I suspected were related to 'Reload Domain' being disabled. To quickly verify the cause, I needed an efficient way to toggle 'Enter Play Mode Options'. However, navigating through 'Project Settings' to 'Editor' and then scrolling down to find the 'Enter Play Mode Options' was becoming tedious, so I looked for ways to make this process a little easier.

#### The Solution

After some research, I found that the '**EditorSettings**' class has a '**enterPlayModeOptionsEnabled**' property. Coupled with '**MenuItem**' attribute, I was able to make a custom menu item that allows me to quickly toggle on/off 'Enter Play Mode Options'.

##### Adding a Validation Function

To make the menu item visually show the current state of the toggle, I used a menu item validate function, inside I call '**Menu.SetChecked**' to show a check mark if 'Enter Play Mode Options' is enabled.

Now I can finally save 16 seconds of my life everytime I have to toggle this thing on/off (yes, I actually timed it).

![](Images/Menu%20Item%20Editor%20Setting%20(July%2015,%2024).jpg)

Checkout the code: [Link To Gist](https://gist.github.com/visca-c/335d79c112a865c46ee6c6e813447a77)

<br>

## The Mystery of the Non-Saving Unity List
#### The Issue
While implementing an editor tool to edit a list of conditions for my game's A.I in Unity, I encountered an issue where the `Conditions` list unexpectedly became empty after exiting play mode. Normally, this would be solved by a simple call to `EditorUtility.SetDirty`, but it didn't do the trick this time. After some investigation, I uncovered some nuances about Unity's serialization system that are worth documenting.

The code for adding a condition looks like this:

```csharp
void AddCondition(Type type)
{
    var condition = CreateInstance(type) as AICondition;
    aiAction.Conditions.Add(condition);
    //Normally this SetDirty call would be suffice.
    EditorUtility.SetDirty(aiAction);
    Repaint();
}
```

The code for the definition of the condition list looked like this:

```csharp
private List<AICondition> _conditions = new();

public List<AICondition> Conditions
{
    get => _conditions;
    private set => _conditions = value;
}
```
#### The Solution
The problem was that my private list was not serialized. Since public fields are serialized, and public properties are often used like public fields, it was easy to ignore the detail.

The solution was simple: adding a `[SerializeField]` attribute to the private list. Here's the updated code:

```csharp
[SerializeField] private List<AICondition> _conditions = new();

public List<AICondition> Conditions
{
    get => _conditions;
    private set => _conditions = value;
}
```

### Conclusion

A simple `[SerializeField]` solved the issue, turning a frustrating bug into a valuable lesson. Public properties don’t serialize without explicit backing fields, and editing data in the editor requires serialization to save properly. With this fix, my editor tool is one step closer to completion—and hopefully, I’ll avoid similar pitfalls in the future.

<br>

## Use SessionState for initialization
#### Overview
Allows you to store and retrieve session-persistent data in the Editor. Useful for ensuring that certain initializations only run once per session.

E.G Example Usage: 
```csharp
    [InitializeOnLoadMethod]
    public static void InitializeOnLoad()
    {
        bool initialized = SessionState.GetBool(SessionStateKey, false);
        if (initialized) return;
        Initialize();
    }
```

<br>

## Dispaly confirmation dialog in editor
#### Description
[EditorUtility.DisplayDialog](https://docs.unity3d.com/6000.0/Documentation/ScriptReference/EditorUtility.DisplayDialog.html) is useful to show confirmation dialog when making editor tool.



E.G Example Usage: 
```csharp
    public static void PerformAction()
    {
        if (EditorUtility.DisplayDialog("Confirm Action", "Are you sure you want to proceed?", "Yes", "No"))
        {
            Debug.Log("Action confirmed.");
            // Perform the action here
        }
        else
        {
            Debug.Log("Action canceled.");
        }
    }
```

<br>

## Save Scriptable Object to asset
#### Context
I made an editor tool that draws a list of custom objects, these scriptable objects were created by code, and did not have .asset file. A bug arised when I try to use this editor tool on a prefab object, my list would not get saved. Later on I learned that scriptable objects' data need to be saved on file, typically their own .asset file. But in this use case I did not want to have explicit .asset files for each scriptable object, that's when I discovered `AssetDatabase.AddObjectToAsset`. I created an save data object, then add my custom scriptable object to it, upon removal, call `AssetDatabase.RemoveObjectFromAsset`, and now my list saves properly.



E.G Example Usage: 
```csharp
    public void OnAdd(MySo so)
    {
        AssetDatabase.AddObjectToAsset(so, _saveDataObject);
    }

    public void OnRemove(MySo so)
    {
        AssetDatabase.RemoveObjectFromAsset(so);
    }
```

<br>

## Skip Drawing Serialized Fields Using Custom Editor
#### Context
Sometimes I only want to expose certain field of a class in the inspector for a cleaner look. One way to do it is manually drawing them through custom editor, but it gets tedious when you want to draw most fields but only leave one or two. This is when skip drawing fields comes in handy, it allows you to only write out the fields you don't want to draw, and keep everything else the same.

Example: Skip drawing the script field.
```csharp
using UnityEditor;

[CustomEditor(typeof(ExampleClass))]
public class ExampleClass : Editor
{
    public override void OnInspectorGUI()
    {
        serializedObject.Update();
        SerializedProperty prop = serializedObject.GetIterator();
        bool enterChildren = true;
        while (prop.NextVisible(enterChildren))
        {
            if (prop.name == "m_Script")
            {
                enterChildren = false;
                continue;
            }
            EditorGUILayout.PropertyField(prop, true);
            enterChildren = false;
        }
        serializedObject.ApplyModifiedProperties();
    }
}
```

<br>
