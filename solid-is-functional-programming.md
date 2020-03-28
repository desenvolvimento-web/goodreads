Autor: Luiz Felipe Stangarlin ([@Luiz-Monad](https://github.com/Luiz-Monad))

# SOLID is FP

So, I decided to write a little thing, only to practice writing, it's about SOLID being compared between class-based-OO and FP, and there will be grammar errors and there will be a lot of formatting errors, 'bear' with me as I type this as fast as I can. Also correct if I'm wrong.
Warning, wall of text in English.
### Single responsibility principle
*" a class should have only a single responsibility "*

A pure function is something with a single responsibility, turning its input into an output.
What could be simpler?.

**C#**
```C#
class oo_bullshit { 
  public int mutable_state;
  public oo_bullshit(int state) { mutable_state = state; } 
  public change_it() { mutable_state++; }
  public int getState() { return mutable_state; }
}
```
**F#**
```F#
let single_responsibility input = input + 1
```

### Open/closed principle
*" software entities … should be open for extension, but closed for modification."*

You don't need to modify the body of the functions when you can just modify what they return, or pass a function in.
**C#**
```C#
class brittle_inheritance : oo_bullshit 
{ //luckly C# forces you to say a method should be overriden, java and C++ don't.
  public void extended() { mutable_state *= 2 } 
}
```
**F#**
```F#
let open_closed input = input * 2
let using_it = single_responsibility >> open_closed
//or
let open_closed fn input = ( fn input ) * 2
//or even, if you learn function composition, one of the "pillars" of FP.
let open_closed_2 fn = fn >> (*) 2
```
### Liskov substitution principle
*"objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program."*

Every function is defined by it's type, if you take functions with the same input and output type, you can use them without being affected by the change.

**C#**
```C#
interface IDrawable { virtual void draw(Graphics g); }
class CString : IDrawable { 
  String s;
  public CString(string s) { this.s = s; }
  override void draw(Graphics g) { g.DrawString(s); }
}

class Shape : IDrawable { 
  Point[] points;
  public Shape(Point[] points) { this.points = points; }
  override void draw(Graphics g) { 
    var last = points[points.Count - 1];
    foreach(var p in points) { g.DrawLine(last, p); last = p;
  }
}
//using
var points = new [] { new Point(1.0, 2.0), new Point(2.0, 2.0), new Point(2.0, 1.0) };
var items = new [] { new CString("test"), new Shape(points) };
forach (var i in items) i.draw(screen.Graphics);
```
**F#**
```F#
//lets start with the implementation, they are what's inside the "draw" in the C# version
let stringRasterizer s g = g.DrawString s
let lineRasterizer l g = 
  Seq.zipWith l ( Seq.last l ) ( 
    fun prev line -> g.DrawLine ( prev, line ) )
//underscore above should be spaces, fuck you zuckerzerg, and yes, in F# spaces are part of the grammar.
//here's the magic of Liskov, no matter what the drawFn does with the graphics the program will still work, same as above
let toScreen drawable drawFn = 
  drawFn drawable screen.Graphics
//using, easy isn't ?.
//that's it, you don't need inheritance or interfaces to use it
toScreen ( stringRasterizer "test" )
toScreen ( lineRasterizer [ (1.0, 2.0), (2.0, 2.0), (2.0, 1.0) ] )
//But wait, you would ask, how can I have a list of items of different types
//and use the same function?
//Of course FP also has polymorphism!
type Drawable = CString of string | Shape of Point list
//All the goodies from real "OO"
//We can have Message passing !
//draw : Drawable -> ( Graphics -> unit )
let drawable = function
| CString s -> stringRasterizer s
| Shape l -> lineRasterizer l
//using, a little bit of algebraic data types, or "just data structures".
//see, that's how you program without classes and RealOO (TM) and FP at the same time, and SOLID!
let toDraw = ( String "test", Shape [ (1.0, 2.0), (2.0, 2.0), (2.0, 1.0) ] )
Seq.map toDraw ( drawable >> toScreen )
```

### Interface segregation principle
*" many client-specific interfaces are better than one general-purpose interface. "*

What are better client interfaces than a single function? just a function of course.

**C#**
```C#
interface IDrawable { virtual void draw(Graphics g); }
```
**F#**
```F#
draw : Drawable -> ( Graphics -> unit )
//in F# you don't need to 'type' anything at all, any function
//with the above type would be an specific interface and would
//work the same way because functions are first-class objects
//and can be passed as arguments, so you get polymorphism.
```
### Dependency inversion principle
*"one should depend upon abstractions, [not] concretions."*

That's where FP shines, first-class functions are dependency inversion at the core. They're so pervasive in FP that you use without noticing all the boiler-plate code you didn't needed to write.

**C#**
```C#
class Dependency { 
  public int mutable_state;
  public Dependency(int state) { mutable_state = state; } 
  public MutateState() { return mutable_state++; }
}
class Something { 
  public Dependency dependency;
  public Something( Dependency dependency) { this.dependency = dependency; } 
  public Object DoIt() { return dependency.MutateState() * 2; }
}
//using
var s = new Something(new Dependency(1));
```
**F#**
```F#
let doIt dependency input = ( dependency input ) * 2
//using
let dependency i = i + 1
doIt dependency 1
```


I think I gave enough examples to prove my point that you can have all the good principles behind a SOLID architecture even when you're using FP, FP is even more aligned to those principles than class-based OO.

[SOURCE](https://www.facebook.com/groups/142918099147059?view=permalink&id=1240397219399136)
