### Statically vs Dynamically Typed Languages
___
#### Type checking

_What_ :question: The **process of verifying** type constraints. Type checking occurs at compile-time (**static**) or at run-time (**dynamic**).

_Goal_ :bulb: **Reduce type errors**
- like adding integers and strings then bam. program will throw <span style="color:red">type errors</span> which stops run-time or compilation
https://miro.medium.com/max/1400/1*BddwVWW6hFU0miT9DCbUWQ.png

_How_ :seedling: Two methods: **statically** and **dynamically**

<code>
<i>Example: </i> a = “5” + 2;
</code>
<p></p>
<p style="color:grey">Additional notes:</p>

[Languages](/https://miro.medium.com/max/1400/1*BddwVWW6hFU0miT9DCbUWQ.png)

#### Statically typed
_What_ :question: Variable is known at **compile-time** instead of at run-time

_Goal_ :bulb: Errors caught **early**. **Faster** execution.

_How_ :seedling: **Declare** data types of variables.

<code>
<i>Example: </i>
</code>
```
int data;
data = 50;
data = “Hello World!”; // causes an compilation error
```
<p></p>
<p style="color:grey">
Additional notes: 
</p>

#### Dynamically typed
_What_ :question: Variable is checked during **run-time**. Variables can be bound to different object types during execution.

_Goal_ :bulb: **Don't have to wait for compiler**. Run-time objects have a **type tag**.
- Type tags allow: dynamic dispatch, late binding, down-casting, reflection, etc.

_How_ :seedling: **Run-time** type-checking. 

<code>
<i>Example: </i>
</code>
```
data = 10;
data = “Hello World!”; // no error caused
```

#### Strongly typed
_What_ :question: Variables bound to type.

_Goal_ :bulb: **Prevent run-time errors**.

_How_ :seedling: **Checks the object type** before an operation. Throw error if mismatch.

<code>
<i>Example: </i>
</code>
```
temp = “Hello World!”
temp = temp + 10; // program terminates with below stated error
```

#### Weakly typed
_What_ :question: Variables not bound to type. Allows implicit conversions.

_Goal_ :bulb: **Fast and flexible**.

<code>
<i>Example: </i>
</code>
```
$temp = “Hello World!”;
$temp = $temp + 10; // no error caused
echo $temp;
```