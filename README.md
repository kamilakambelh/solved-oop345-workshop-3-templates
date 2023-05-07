Download Link: https://assignmentchef.com/product/solved-oop345-workshop-3-templates
<br>
In this workshop, you design and code a couple of class templates and test them on different instantiations.## *In-Lab*

This workshop consists of three modules:– `w3` (supplied)– `List`– `LVPair`

Enclose all your source code within the `sdds` namespace and include the necessary guards in each header file.

### `w3` Module (supplied)

**Do not modify this module!**  Look at the code and make sure you understand it.

### `List` Module

This module defines a class template for a collection of elements of any data type (for example, a list of `int`, or a list `Student`, etc.)

Design and code a class template named `List` for managing a statically allocated array of any datatype.  The template parameters in order of their specification are:

– `T`: the type of any element in the array– `N`: the maximum number of elements in the array (an integer without sign)

Your design should be able to distinguish between– the capacity of the array (`N`)– the number of elements added to the list. Initially the list is empty.

***Public Members***– `size_t size() const`: returns the number of elements in the list– `const T&amp; operator[](size_t i) const`: returns the element at index `i` (assume the parameter is valid).– `void operator+=(const T&amp; tt)`: if the list didn’t reach the capacity, add a copy of the parameter to the array. Otherwise, do nothing.

### `LVPair` Module

Design and code a class template named `LVPair` for managing a *label-value* pair. The template parameters identify the types of the label and value objects that constitute an `LVPair` object:– `L`: the type of the label– `V`: the type of the value

***Public Members***– `LVPair()`: sets the object in an empty state– `LVPair(const L&amp; aa, const V&amp; bb)`: copies the values received in the parameters into the instance variables.– `const L&amp; key() const`: returns the **key** component of the pair– `const V&amp; value() const`: returns the **value** component of the pair– `void display(std::ostream&amp; os) const`: inserts into the parameter the stored values in the following format“`LABEL : VALUE&lt;endl&gt;“`

***Free Helpers***– `std::ostream&amp; operator&lt;&lt;(std::ostream&amp; os, const sdds::LVPair&lt;L, V&gt;&amp; pair)`: calls the function `LVPair&lt;L, V&gt;::display()` to insert a pair into the stream.

### Sample Output

When the program is started with the command (the file `sales.txt` is provided):“`w3.exe sales.txt“`the output should look like:“`Command Line:————————–1: w3.exe2: sales.txt————————–

Detail Ticket Sales————————–Student : 25Adult : 13Student : 12Adult : 6Student : 5Adult : 15“`

## *At-Home*

The *at-home* part of this workshop upgrades your *in-lab* solution to include– alignment of the label and value output in pretty columnar format– accumulation of the values stored in a `List`, for a specified label

To implement each upgrade, you will derive a templated class from your original templated class (one derived class from `List` and one derived class from `LVPair`) and specialize the class derived from `LVPair` as described below.

### `LVPair` Module

The `LVPair` module must be upgraded to include both summation and label alignment functionality.

Modify the function `display()` in the class `LVPair` to enable polymorphism on it (make it `virtual`). This is the only change that you need to make to your original template.

Add to this module a class template named `SummableLVPair` to manage the summation and pretty displaying of labeled values.

This class is derived from `LVPair&lt;L, V&gt;`, and receives 2 template parameters:– `L`: the type of the label– `V`: the type of the value

***Static Private Members for `SummableLVPair`***– an object of type `V` that holds the initial value for starting a summation. The initial value depends on the type of the value in the label-value pair and will be defined separately.– a variable of type `size_t` that holds the minimum field width for pretty columnar output of label-value pairs (initialize it with 0). This is the minimum number of characters needed to display any of the labels in a set of labels.

This value must be updated every time a new pair is added to the collection.

***Static Public Members for `SummableLVPair`***

– `static const V&amp; getInitialValue()`: return the initital value stored.

***Public Members for `SummableLVPair`***

– default constructor

– `SummableLVPair(const L&amp; lbl, const V&amp; val)`: stores the pair in the collection, and updates the field width if necessary.This functions assumes that the type `L` supports a function named `size()` that returns the number of characters required to display `lbl`.

– `V sum(const L&amp; lbl, const V&amp; val) const`:– If the label of the pair stored in the current instance is `lbl`, then add the value of the pair and `val` together and return the result. Use `+` for addition.– Otherwise, return `val`.

– `void display(std::ostream&amp; os) const`: set the alignment to left and the field width and then call `display()` from the parent class. At the end, restore the alignment to right.

***Specializations***

– for `SummableLVPair&lt;std::string, std::string&gt;`, the initial value for summation should be set to empty string (`””`).

– for `SummableLVPair&lt;std::string, int&gt;`, the initial value for summation should be set to 0.

– for `SummableLVPair&lt;std::string, std::string&gt;`, the function `sum()` should concatenate the values stored using `, ` as separator (use operator `+` to concatenate strings).

### `List` Module

The template class `List` doesn’t need any change.

Add to this module another **template** class called `LVList`, that can manage a collection of *summable* elements.

This class is derived from `List&lt;T, N&gt;`, and receives 4 template parameters:– `L`: the type of a label– `V`: the type summation value– `T`: the type of any element in the collection– `N`: the maximum number of elements in the collection (an integer without sign)

In this design, `summable` elements are objects of a type that supports the operation `V sum(const L&amp; l, const V&amp; v)`.

***Public Members for `LVList`***– `V accumulate(const L&amp; label) const`: this function accumulates all the values stored in the list that have the label `label` into a local object of type `V`.

– get the initial value from the type `T` and store it into a local variable of type `V`: this is the accumulator.In this design, the type `T` must have a static member called `getInitialValue()`.

– iterate over the collection and call the function `sum()` for each item (use the overloaded `operator[]` to access the item at index `i`). Store the result of `sum()` into the accumulator.

– return the accumulator to the client.

Once the implementation of this module is complete, if you attempt to instantiate the class `LVList` using a type `T` that doesn’t support the `sum()` and `getInitialValue()` operations, you will receive compilation errors **for that specific instantiation**.

### Sample Output

When the program is started with the command:“`w3.exe products.txt sales.txt“`the output should look like:“`Command Line:————————–1: w3.exe2: products.txt3: sales.txt————————–

Individual Index Entries————————–Grocerries  : tomatoesElectronics : computerTools       : hammerGrocerries  : lettuceGrocerries  : potatoesElectronics : Multimedia_PlayerElectronics : HDDGrocerries  : meatTools       : jigsaw

Collated Index Entries————————–Tools: hammer, jigsawGrocerries: tomatoes, lettuce, potatoes, meatElectrnics:Electronics: computer, Multimedia_Player, HDD

Detail Ticket Sales————————–Student : 25Adult   : 13Student : 12Adult   : 6Student : 5Adult   : 15

Summary of Ticket Sales————————–Student Tickets =   86.10Adult Tickets   =  110.50“`