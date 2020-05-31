Android Data Binding Codelab
=============================================
Follow the codelab in

https://github.com/googlecodelabs/android-databinding


License
--------

Copyright 2018 Google LLC. Licensed under the Apache License, Version 2.0.


Notes:

To convert a regular layout to Data Binding layout:
--------------------------------------------------------
Wrap your layout with a <layout> tag
        
Add layout variables (optional) - variables which will be used in expressions in xml

Add layout expressions (optional) - will be written in xml to set xml attributes

Changes in Activity/Fragment-
--------------------------------------------------------
We will replace setContentView(R.layout.plain_activity) with DataBindingUtil.setContentView(this, R.layout.plain_activity)

DataBindingUtil will be generated when data binding is enabled in gradle file.

binding.viewmodel= viewmodel - assigning the variable used for data binding

binding.lifecycleOwner= this - for observing livedata which acts as holder for name / likes in this example

Changes in XML-
--------------------------------------------------------
```
<data>
        <variable
                name="viewmodel"
                type="com.example.android.databinding.basicsample.data.SimpleViewModel"/> 
</data>

<TextView
                android:id="@+id/plain_name"
                android:text="@{viewmodel.name}" 
... />
<TextView
                android:id="@+id/likes"
                android:text="@{Integer.toString(viewmodel.likes)}"                
... />

android:onClick="@{() -> viewmodel.onLike()}"  - onLike() of viewmodel will be called
```

Using binding adapters with custom attributes
--------------------------------------------------------
If we want to show(hide/visible) Progress bar based on number of likes, we need to write a custom binding
adapter which will take two parameters : Param1 - progressBar view, Param2 - likes

```
@BindingAdapter("app:hideIfZero")
fun hideIfZero(view: ProgressBar, likes: int){
  //make view hide/show based on likes
}
```

In layout xml,
```
<ProgressBar
...
app:hideIfZero= "@{videmodel.likes}"
... />
```

Using binding adapters with multiple parameters:
--------------------------------------------------------
Suppose we want to update progress bar as per likes. For this, we would require two parameters

Param1: likes, Param2: max value

```
@BindingAdapter(value = ["app:progressScaled", "android:max"])
fun setProgressBar(progressBar: ProgressBar, likes: int, max: int){
  //write logic to update progress bar depending on likes count and max value
}
```

In layout xml,
```
<ProgressBar
...
app:progressScaled= "@{videmodel.likes}"
android:max="@{100}"
... />
```

By using layout expressions to bind components in the layout file, you can:
----------------------------------------------------------------------------------------------------------------
Improve your app's performance

Help prevent memory leaks and null pointer exceptions

Streamline your activity's code by removing UI framework calls
