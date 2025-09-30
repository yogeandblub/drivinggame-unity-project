---
uid: input-system-processors
---
# Processor types

Below are predefined processors. You can also create custom processors. For guidance on when to use each type, refer to [Using Processors](UsingProcessors.md).

* [Predefined Processors](#predefined-processors)
    * [Clamp](#clamp)
    * [Invert](#invert)
    * [Invert Vector 2](#invert-vector-2)
    * [Invert Vector 3](#invert-vector-3)
    * [Normalize](#normalize)
    * [Normalize Vector 2](#normalize-vector-2)
    * [Normalize Vector 3](#normalize-vector-3)
    * [Scale](#scale)
    * [Scale Vector 2](#scale-vector-2)
    * [Scale Vector 3](#scale-vector-3)
    * [Axis deadzone](#axis-deadzone)
    * [Stick deadzone](#stick-deadzone)
* [Writing custom Processors](#writing-custom-processors)

## Predefined Processors

The Input System package comes with a set of useful Processors you can use.

### Clamp

|__Name__|[`Clamp`](../api/UnityEngine.InputSystem.Processors.ClampProcessor.html)|
|---|---|
|__Operand Type__|`float`|
|__Parameters__|`float min`<br>`float max`|

Clamps input values to the [`min`..`max`] range.

### Invert

|__Name__|[`Invert`](../api/UnityEngine.InputSystem.Processors.InvertProcessor.html)|
|---|---|
|__Operand Type__|`float`|

Inverts the values from a Control (that is, multiplies the values by -1).

### Invert Vector 2

|__Name__|[`InvertVector2`](../api/UnityEngine.InputSystem.Processors.InvertVector2Processor.html)|
|---|---|
|__Operand Type__|`Vector2`|
|__Parameters__|`bool invertX`<br>`bool invertY`|

Inverts the values from a Control (that is, multiplies the values by -1). Inverts the x axis of the vector if `invertX` is true, and the y axis if `invertY` is true.

### Invert Vector 3

|__Name__|[`Invert Vector 3`](../api/UnityEngine.InputSystem.Processors.InvertVector3Processor.html)|
|---|---|
|__Operand Type__|`Vector3`|
|__Parameters__|`bool invertX`<br>`bool invertY`<br>`bool invertZ`|

Inverts the values from a Control (that is, multiplies the values by -1). Inverts the x axis of the vector if `invertX` is true, the y axis if `invertY` is true, and the z axis if `invertZ` is true.

### Normalize

|__Name__|[`Normalize`](../api/UnityEngine.InputSystem.Processors.NormalizeProcessor.html)|
|---|---|
|__Operand Type__|`float`|
|__Parameters__|`float min`<br>`float max`<br>`float zero`|

Normalizes input values in the range [`min`..`max`] to unsigned normalized form [0..1] if `min` is >= `zero`, and to signed normalized form [-1..1] if `min` < `zero`.

### Normalize Vector 2

|__Name__|[`NormalizeVector2`](../api/UnityEngine.InputSystem.Processors.NormalizeVector2Processor.html)|
|---|---|
|__Operand Type__|`Vector2`|

Normalizes input vectors to be of unit length (1). This is the same as calling `Vector2.normalized`.

### Normalize Vector 3

|__Name__|[`NormalizeVector3`](../api/UnityEngine.InputSystem.Processors.NormalizeVector3Processor.html)|
|---|---|
|__Operand Type__|`Vector3`|

Normalizes input vectors to be of unit length (1). This is the same as calling `Vector3.normalized`.

### Scale

|__Name__|[`Scale`](../api/UnityEngine.InputSystem.Processors.ScaleProcessor.html)|
|---|---|
|__Operand Type__|`float`|
|__Parameters__|`float factor`|

Multiplies all input values by `factor`.

### Scale Vector 2

|__Name__|[`ScaleVector2`](../api/UnityEngine.InputSystem.Processors.ScaleVector2Processor.html)|
|---|---|
|__Operand Type__|`Vector2`|
|__Parameters__|`float x`<br>`float y`|

Multiplies all input values by `x` along the X axis and by `y` along the Y axis.

### Scale Vector 3

|__Name__|[`ScaleVector3`](../api/UnityEngine.InputSystem.Processors.ScaleVector3Processor.html)|
|---|---|
|__Operand Type__|`Vector3`|
|__Parameters__|`float x`<br>`float y`<br>`float x`|

Multiplies all input values by `x` along the X axis, by `y` along the Y axis, and by `z` along the Z axis.

### Axis deadzone

|__Name__|[`AxisDeadzone`](../api/UnityEngine.InputSystem.Processors.AxisDeadzoneProcessor.html)|
|---|---|
|__Operand Type__|`float`|
|__Parameters__|`float min`<br>`float max`|

An axis deadzone Processor scales the values of a Control so that any value with an absolute value smaller than `min` is 0, and any value with an absolute value larger than `max` is 1 or -1. Many Controls don't have a precise resting point (that is, they don't always report exactly 0 when the Control is in the center). Using the `min` value on a deadzone Processor avoids unintentional input from such Controls. Also, some Controls don't consistently report their maximum values when moving the axis all the way. Using the `max` value on a deadzone Processor ensures that you always get the maximum value in such cases.

### Stick deadzone

|__Name__|[`StickDeadzone`](../api/UnityEngine.InputSystem.Processors.StickDeadzoneProcessor.html)|
|---|---|
|__Operand Type__|`Vector2`|
|__Parameters__|`float min`<br>`float max`|

A stick deadzone Processor scales the values of a Vector2 Control, such as a stick, so that any input vector with a magnitude smaller than `min` results in (0,0), and any input vector with a magnitude greater than `max` is normalized to length 1. Many Controls don't have a precise resting point (that is, they don't always report exactly 0,0 when the Control is in the center). Using the `min` value on a deadzone Processor avoids unintentional input from such Controls. Also, some Controls don't consistently report their maximum values when moving the axis all the way. Using the `max` value on a deadzone Processor ensures that you always get the maximum value in such cases.

## Writing custom Processors

You can also write custom Processors to use in your Project. Custom Processors are available in the UI and code in the same way as the built-in Processors. Add a class derived from [`InputProcessor<TValue>`](../api/UnityEngine.InputSystem.InputProcessor-1.html), and implement the [`Process`](../api/UnityEngine.InputSystem.InputProcessor-1.html#UnityEngine_InputSystem_InputProcessor_1_Process__0_UnityEngine_InputSystem_InputControl_) method:

>__IMPORTANT__: Processors must be __stateless__. This means you cannot store local state in a processor that will change depending on the input being processed. The reason for this is because processors are not part of the [input state](./Controls.md#control-state) that the Input System keeps.

```CSharp
public class MyValueShiftProcessor : InputProcessor<float>
{
    [Tooltip("Number to add to incoming values.")]
    public float valueShift = 0;

    public override float Process(float value, InputControl control)
    {
        return value + valueShift;
    }
}
```

Now, you need to tell the Input System about your Processor. Call [`InputSystem.RegisterProcessor`](../api/UnityEngine.InputSystem.InputSystem.html#UnityEngine_InputSystem_InputSystem_RegisterProcessor__1_System_String_) in your initialization code. You can do so locally within the Processor class like this:

```CSharp
#if UNITY_EDITOR
[InitializeOnLoad]
#endif
public class MyValueShiftProcessor : InputProcessor<float>
{
    #if UNITY_EDITOR
    static MyValueShiftProcessor()
    {
        Initialize();
    }
    #endif

    [RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.BeforeSceneLoad)]
    static void Initialize()
    {
        InputSystem.RegisterProcessor<MyValueShiftProcessor>();
    }

    //...
}
```

Your new Processor is now available in the in the [Input Actions Editor](ActionsEditor.md) and you can also add it in code like this:

```CSharp
var action = new InputAction(processors: "myvalueshift(valueShift=2.3)");
```

If you want to customize the UI for editing your Processor, create a custom [`InputParameterEditor`](../api/UnityEngine.InputSystem.Editor.InputParameterEditor-1.html) class for it:

```CSharp
// No registration is necessary for an InputParameterEditor.
// The system will automatically find subclasses based on the
// <..> type parameter.
#if UNITY_EDITOR
public class MyValueShiftProcessorEditor : InputParameterEditor<MyValueShiftProcessor>
{
    private GUIContent m_SliderLabel = new GUIContent("Shift By");

    public override void OnEnable()
    {
        // Put initialization code here. Use 'target' to refer
        // to the instance of MyValueShiftProcessor that is being
        // edited.
    }

    public override void OnGUI()
    {
        // Define your custom UI here using EditorGUILayout.
        target.valueShift = EditorGUILayout.Slider(m_SliderLabel,
            target.valueShift, 0, 10);
    }
}
#endif
```
