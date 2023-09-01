The task is to implement a new feature in the provided fork of Less library (http://lesscss.org/) according to the specification below.

## Feature specification

Add a feature that allows removing properties defined inside a variable during mixin inclusion.

Here is the most basic usage example of the desired feature:

<em>@my_list: "color", "width"; // Here we define a list variable with 2 values: color and width<br></em> 
<em>.my-mixin () {<br>
width: 100px;<br>
background: red;<br>
color: green;<br>
}<br>
.my-class-with-excluded-props {<br>
/* Here we remove color and width properties from the included mixin. */<br>
.my-mixin() - @my_list;<br>
}<br>
.my-class-with-all-props {<br>
/* Here we simply include mixin. This feature is already supported by Less. */<br>
.my-mixin();<br>
}<br>
</em>

### Excepted output:
<em>.my-class-with-excluded-props {<br>
/* Here we remove color and width properties from the included mixin. */<br>
background: red;<br>
}<br>
.my-class-with-all-props {<br>
/* Here we simply include mixin. This feature is already supported by Less. */<br>
width: 100px;<br>
background: red;<br>
color: green;<br>
}<br>
</em>

### Solution should satisfy to the following requirements:

It should not break any of the existing features.

It should be able to handle removal of multiple lists during the same mixin inclusion. For example, it should be possible to write a code like this:
<em><br>
.my-mixin()<br>
-@list_1<br>
-@list_2;
</em>

It should work properly with all different kinds of variables.
- Regular variables like <em>@my_list: "color"</em>;
- “Reference” variables like <em>@@variable_ref</em>;
- Variables passed as command line arguments

In case the list contains a value which is not a property of the mixin, such property should be ignored.

When mixin inclusion has the modifier “important”, it should be still possible to remove properties. For example this syntax is valid: <em>.my-mixin() !important - @my_list;<br></em>
But the following syntax is not considered as valid, your program must report an error:
<em><br>.my-mixin() - @my_list !important;<br></em>
In case the modifier “important” is used, it should precede any possible property removal operations, like in the first example.

During property removal, only outer properties should be removed. Here is an example demonstrating this requirement:<br>
Input:<br>
<em>@my_list: "color";<br>
.my-mixin-1 () {<br>
width: 100px;<br>
color: green;<br>
.a {<br>
color: white;<br>
}}<br>
.my-class {<br>
.my-mixin-1() - @my_list;<br>
}<br>
</em>
Output:<br>
<em>
.my-class {<br>
width: 100px;<br>
}<br>
.my-class .a {<br>
color: white;<br>
}<br>
</em>